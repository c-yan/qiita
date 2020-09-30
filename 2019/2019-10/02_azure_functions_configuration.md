# Azure Functions の設定を Microsoft.Extensions.Configuration.Binder を使ってバインドする

さくっとできるかと思ったら大変だったので大変だったのでメモ.

Azure Portal から入れた`アプリケーション設定`や`接続文字列`は環境変数を通して関数に渡される. Azure Functions v2 では以下のような感じになる.

■ Key1=Value1のアプリケーション設定 (2行出る)

```
APPSETTING_Key1=Vaue1
Key1=Vaue1
```

■ Key1=Value1の接続文字列 (種類=Custom)

```
CUSTOMCONNSTR_Key1=Value1
```

EnvironmentVariablesConfigurationProvider が接続文字列についてはこれを `ConnectionStrings:Key1=Value1` に書き換えてくれるが、アプリケーション設定については放置である. しょうがないので自分で書き換えてくれるものを作った.

```AppSettingsProvider.cs
using Microsoft.Extensions.Configuration;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;

namespace FunctionApp1.Configs
{
    public static class AppSettingsExtensions
    {
        public static IConfigurationBuilder AddAppSettings(this IConfigurationBuilder configurationBuilder)
        {
            configurationBuilder.Add(new AppSettingsConfigurationSource());
            return configurationBuilder;
        }
    }

    public class AppSettingsConfigurationSource : IConfigurationSource
    {
        public IConfigurationProvider Build(IConfigurationBuilder builder)
        {
            return new AppSettingsProvider();
        }
    }

    public class AppSettingsProvider : ConfigurationProvider
    {
        static readonly string Prefix = "APPSETTING_";

        public override void Load()
        {
            var data = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase);

            foreach (var e in Environment.GetEnvironmentVariables().Cast<DictionaryEntry>())
            {
                var key = (string)e.Key;
                if (!key.StartsWith(Prefix)) continue;

                data[$"AppSettings:{key.Substring(Prefix.Length)}"] = (string)e.Value;
            }

            Data = data;
        }
    }
}
```

後は設定を入れる POCO クラスを作ってバインドするだけである.

```AppConfigs.cs
namespace FunctionApp1.Configs
{
    public class AppConfigs
    {
        public ConnectionStrings ConnectionStrings { get; set; }
        public AppSettings AppSettings { get; set; }
    }
}
```

```AppSettings.cs
namespace FunctionApp1.Configs
{
    public class AppSettings
    {
        public string Key1 { get; set; }
        public string Key2 { get; set; }
    }
}
```

```ConnectionStrings.cs
namespace FunctionApp1.Configs
{
    public class ConnectionStrings
    {
        public string Database { get; set; }
        public string AzureStorage { get; set; }
    }
}
```

```Function1.cs
using FunctionApp1.Configs;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Configuration;
using System.Text;

namespace FunctionApp1
{
    public static class Function1
    {
        [FunctionName("Function1")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequest req, TraceWriter log)
        {
            var configs = new ConfigurationBuilder()
                .AddEnvironmentVariables()
                .AddAppSettings()
                .Build()
                .Get<AppConfigs>();

            var sb = new StringBuilder()
                .AppendLine(configs.AppSettings.Key1)
                .AppendLine(configs.AppSettings.Key2)
                .AppendLine(configs.ConnectionStrings.Database)
                .AppendLine(configs.ConnectionStrings.AzureStorage);

            return new ContentResult()
            {
                Content = sb.ToString(),
                ContentType = "text/plain",
                StatusCode = StatusCodes.Status200OK
            };
        }
    }
}
```

`アプリケーション設定`に `Key1=Value1, Key2=Value2`、`接続文字列`に `Database=Database ConnectionString!!, Azure Storage=AzureStorage ConnectionString!!` を入れてアクセスしたら以下の出力が得られた. 成功!

```
Value1
Value2
Database ConnectionString!!
AzureStorage ConnectionString!!
```
