# 続 JSON をクラスにバインドする (C#)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.24) となります.

過去に `DataContractJsonSerializer` で書いた [JSON をクラスにバインドする (C#)](https://qiita.com/c-yan/items/9a6a5c1f59526c1a1ff2) を `System.Text.Json` で焼き直し.

JSON を C# のクラスにバインドする. まずバインド先のクラスを作る. 都合により Azure AD のレスポンス.

```csharp
using System.Runtime.Serialization;

namespace ConsoleApp1
{
    public sealed class AzureADResponse
    {
        [JsonPropertyName("token_type")]
        public string TokenType { get; set; }

        [JsonPropertyName("scope")]
        public string Scope { get; set; }

        [JsonPropertyName("expires_in")]
        public int ExpiresIn { get; set; }

        [JsonPropertyName("ext_expires_in")]
        public int ExtExpiresIn { get; set; }

        [JsonPropertyName("access_token")]
        public string AccessToken { get; set; }

        [JsonPropertyName("refresh_token")]
        public string RefreshToken { get; set; }

        [JsonPropertyName("id_token")]
        public string IdToken { get; set; }
    }
}
```

後は Azure AD のレスポンスの文字列が変数 s に入っているとすれば、以下でバインドできる.

```csharp
JsonSerializer.Deserialize<AzureADResponse>(s);
```
