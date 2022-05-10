# JSON をクラスにバインドする (C#)

内容が古くなってしまったので、こちらを見てください＞[続 JSON をクラスにバインドする (C#)](https://qiita.com/c-yan/items/c0a226c83d339a9662a7)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201) となります.

JSON を C# のクラスにバインドする. まずバインド先のクラスを作る. 都合により Azure AD のレスポンス.

```csharp
using System.Runtime.Serialization;

namespace ConsoleApp1
{
    [DataContract]
    public sealed class AadResponse
    {
        [DataMember(Name = "token_type")]
        public string TokenType { get; set; }

        [DataMember(Name = "scope")]
        public string Scope { get; set; }

        [DataMember(Name = "expires_in")]
        public string ExpiresIn { get; set; }

        [DataMember(Name = "ext_expires_in")]
        public string ExtExpiresIn { get; set; }

        [DataMember(Name = "access_token")]
        public string AccessToken { get; set; }

        [DataMember(Name = "refresh_token")]
        public string RefreshToken { get; set; }

        [DataMember(Name = "id_token")]
        public string IdToken { get; set; }
    }
}
```

次に DataContract のヘルパーを作る.

```csharp
using System.IO;
using System.Runtime.Serialization;
using System.Text;

namespace ConsoleApp1.Helpers
{
    public static class DataContractHelper
    {
        public static string Serialize<T>(XmlObjectSerializer serializer, T obj)
        {
            using (var stream = new MemoryStream()) {
                serializer.WriteObject(stream, obj);
                stream.Position = 0;
                using (var sr = new StreamReader(stream))
                {
                    return sr.ReadToEnd();
                }
            }
        }

        public static T Deserialize<T>(XmlObjectSerializer serializer, string s)
        {
            using (var stream = new MemoryStream())
            {
                var sBytes = Encoding.UTF8.GetBytes(s);
                stream.Write(sBytes, 0, sBytes.Length);
                stream.Position = 0;
                return (T)serializer.ReadObject(stream);
            }
        }
    }
}
```

最後に DataContract を利用して、JSON を処理するヘルパーを作る.

```csharp
using System.Runtime.Serialization.Json;

namespace ConsoleApp1.Helpers
{
    public static class JsonHelper
    {
        static readonly DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings() { UseSimpleDictionaryFormat = true };

        public static string ToJson<T>(T obj)
        {
            return DataContractHelper.Serialize(new DataContractJsonSerializer(typeof(T), settings), obj);
        }

        public static T ToObject<T>(string json)
        {
            return DataContractHelper.Deserialize<T>(new DataContractJsonSerializer(typeof(T), settings), json);
        }
    }
}
```

後は Azure AD のレスポンスの文字列が変数 s に入っているとすれば、以下でバインドできる.

```csharp
JsonHelper.ToObject<AadResponse>(s)
```

また、AadResponse 型の変数 a から JSON 文字列を作るのは以下になる.

```csharp
JsonHelper.ToJson(a)
```
