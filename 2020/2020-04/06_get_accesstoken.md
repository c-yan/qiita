# リフレッシュトークンを使用して Azure AD からアクセストークンを取得する (C#)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201) となります.

[Microsoft.Extensions.Configuration を使って、INI ファイルに書いた設定を読み込む](https://qiita.com/c-yan/items/3e0b2503d26693140457) と [JSON をクラスにバインドする (C#)](https://qiita.com/c-yan/items/9a6a5c1f59526c1a1ff2) で書いたコードを使っているので、その部分についてはそちらを参照してください.

```csharp
using ConsoleApp1.Configs;
using ConsoleApp1.Helpers;
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static readonly AppConfig Config = AppConfig.Get();
        static readonly HttpClient HttpClient = new HttpClient();

        static async Task<string> GetAccessToken()
        {
            var body = new Dictionary<string, string>()
            {
                { "client_id", Config.OAuth.ClientID },
                { "client_secret", Config.OAuth.ClientSecret },
                { "grant_type", "refresh_token" },
                { "refresh_token", Config.OAuth.RefreshToken },
                { "scope", Config.OAuth.Scope },
                { "redirect_uri", Config.OAuth.RedirectUri }
            };
            var url = $"https://login.microsoftonline.com/{Config.OAuth.TenantID}/oauth2/v2.0/token";
            var response = await HttpClient.PostAsync(url, new FormUrlEncodedContent(body));
            return JsonHelper.ToObject<AadResponse>(await response.Content.ReadAsStringAsync()).AccessToken;
        }

        static void Main(string[] args)
        {
            Console.WriteLine(GetAccessToken().Result);
            Console.ReadLine();
        }
    }
}
```
