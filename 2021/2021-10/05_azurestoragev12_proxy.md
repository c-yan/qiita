# HTTP Proxy 経由で Azure Storage にアクセスする (v12 バージョン) (.NET Core)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.412), Azure.Storage.Blobs 12.10.0 となります.

.NET 用 Azure Blob Storage クライアント ライブラリ v12 でまたマイクロソフトがオーバーホールをして、プロキシの設定方法が変わっていたので調査した. 9.4.1 から使えた .NET 用 Azure Blob Storage クライアント ライブラリ v11 で使える設定方法は [HTTP Proxy 経由で Azure Storage にアクセスする (.NET Core)](https://qiita.com/c-yan/items/0d86259bc3be8304e8b5) をご確認ください.

v12 は async 強制がなくなって先祖返りした感. v11 より v12 のほうが良い気がする.

```csharp
using Azure.Core;
using Azure.Core.Pipeline;
using Azure.Storage.Blobs;
using System;
using System.Net;
using System.Net.Http;

namespace ConsoleApp
{
    class Program
    {
        static readonly string ConnectionString = "...";
        static readonly string ProxyUrl = "...";
        static readonly string ProxyUserId = "...";
        static readonly string ProxyPassword = "...";

        static void Main(string[] args)
        {
            var serviceClient = new BlobServiceClient(ConnectionString, new BlobClientOptions()
            {
                Transport = new HttpClientTransport(new HttpClientHandler()
                {
                    Proxy = new WebProxy()
                    {
                        Address = new Uri(ProxyUrl),
                        Credentials = new NetworkCredential(ProxyUserId, ProxyPassword)
                    },
                }),
                Retry = {
                    Delay = TimeSpan.FromSeconds(5),
                    MaxRetries = 5,
                    Mode = RetryMode.Exponential,
                }
            });
            var containerClient = serviceClient.GetBlobContainerClient("...");
            containerClient.CreateIfNotExists();
            containerClient.UploadBlob("...", new BinaryData("..."));
        }
    }
}
```
