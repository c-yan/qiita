# HTTP Proxy 経由で Azure Storage にアクセスする (.NET Core)

.NET Framework の場合、System.Net.WebRequest.DefaultWebProxy を設定すれば概ね何とかなるものの、それが封じられている .NET Core の場合、対象がサポートしていないと手も足も出ない. Azure Storage Client Library の場合は手も足も出なかったわけですが、最近の版で対応が入ったのでメモ.

前提条件: Storage Client Library 9.4.1 以降であること

Storage Client Library は 9.3.x までは All in One で一つの nuget で Blob, File, Queu, Table に対応していたが、9.4.0 からは Microsoft.Azure.Storage.Blob のように4分割されたので、Update-Package WindowsAzure.Storage をどれだけしても 9.4.x にアップデートされることはないので注意.

Storage Client Library 9.4.1 のリリースノートに以下のようにある.

```
Added support for passing in a DelegatingHandler to use for HTTP calls. A chain of DelegatingHandler instances is supported, but the innermost handler must have a null InnerHandler, which will be assigned our own HttpClientHandler instance for authentication purposes. The DelegatingHandler can modify properties on the HttpClientHandler, such as Proxy. This instance will be unique to the client in that case. Not passing in a DelegatingHandler will continue to use a singleton HttpClientHandler.
```

ここでも触れられているように Proxy のプロパティが変更できる DelegatingHandler を使う. 最初にそのための DelegatingHandler の実装を作る.

```csharp:ProxyInjectionHandler.cs
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace Hoge
{
    public class ProxyInjectionHandler : DelegatingHandler
    {
        private readonly IWebProxy Proxy;
        private bool FirstCall = true;

        public ProxyInjectionHandler(IWebProxy proxy)
        {
            this.Proxy = proxy;
        }

        protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (FirstCall)
            {
                var handler = (HttpClientHandler)this.InnerHandler;
                handler.Proxy = this.Proxy;
                handler.UseProxy = true;
                FirstCall = false;
            }
            return base.SendAsync(request, cancellationToken);
        }
    }
}
```

これを Client のコンストラクタに渡せば OK!

```csharp
var account = CloudStorageAccount.Parse(connectionString);
var client = new CloudBlobClient(account.BlobEndpoint, account.Credentials, new ProxyInjectionHandler(new WebProxy(new Uri(proxyUrl))));
var container = client.GetContainerReference("...");
await container.CreateIfNotExistsAsync();
```
