# Microsoft.Extensions.Configuration を使って、INI ファイルに書いた設定を読み込む

exe.config はずっと微妙だなあと思っていたのですが、Microsoft 謹製の INI ファイルから設定を読む手段が提供されたので、それを使ってみる. 例として以下の INI ファイルを読むコードを作成する.

```ini:config.ini
[AppSettings]
UserName=foo
Password=bar
RetryCount=3

[ConnectionStrings]
Database=xxx
```

Console App (.NET Core) プロジェクトを作成する. TargetFramework が netcoreapp3.1 (.NET Core 3.1) で作成される. 要求は .NET Standard 2.0 なので .NET Framework 4.6.1 や、.NET Core 2.0 でも動くはず.

次に `Microsoft.Extensions.Configuration.Ini` と `Install-Package Microsoft.Extensions.Configuration.Binder` を nuget インストールする. これを書いている時点では 3.1.3 がインストールされる.

```powershell
Install-Package Microsoft.Extensions.Configuration.Ini
Install-Package Microsoft.Extensions.Configuration.Binder
```

そして、セクション毎に情報を詰めるクラスを書いていく.

```csharp:AppSettingsConfig.cs
namespace ConsoleApp1
{
    public class AppSettingsConfig
    {
        public string UserName { get; set; }
        public string Password { get; set; }
        public int RetryCount { get; set; }
    }
}
```

```csharp:ConnectionStringsConfig.cs
namespace ConsoleApp1
{
    public class ConnectionStringsConfig
    {
        public string Database { get; set; }
    }
}
```

最後にそれらセクション毎のクラスをまとめ上げるクラスと、INI ファイルから設定を読み込むメソッドを書いて完成.


```csharp:AppConfig.cs
using Microsoft.Extensions.Configuration;

namespace ConsoleApp1
{
    public class AppConfig
    {
        static AppConfig Instance;

        public ConnectionStringsConfig ConnectionStrings { get; set; }
        public AppSettingsConfig AppSettings { get; set; }

        public AppConfig() { }
        public static AppConfig Get()
        {
            if (Instance != null) return Instance;

            Instance = new ConfigurationBuilder()
                .AddIniFile(".\\config.ini")
                .Build()
                .Get<AppConfig>();

            return Instance;
        }
    }
}
```

以下のように簡単に設定内容にアクセスできる.

```csharp:Program.cs
using System;

namespace ConsoleApp1
{
    class Program
    {
        static readonly AppConfig Config = AppConfig.Get();

        static void Main(string[] args)
        {
            Console.WriteLine(Config.ConnectionStrings.Database);
            Console.WriteLine(Config.AppSettings.UserName);
            Console.WriteLine(Config.AppSettings.RetryCount);
            Console.Read();
        }
    }
}
```
