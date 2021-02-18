# ASP.NET MVC はデフォルトでレスポンスをフルバッファリングする

\# 以下の記事が書かれた時の版数は .NET Framework 4.7.2 / Microsoft.AspNet.Mvc 5.2.7 となります.

ASP.NET MVC はデフォルトでレスポンスボディをフルバッファリングしてから返すようで、何も考えずに大きいストリームとかを返すと `OutOfMemoryException` になります.

```cs
        public ActionResult Download()
        {
            return new FileStreamResult(new BlobClient("connection-string", "container", "large.zip").OpenRead(), "application/zip")
            {
                FileDownloadName = "large.zip"
            };
        }
```

これを回避するには、以下のように `Response.BufferOutput` を `false` にします.

```cs
        public ActionResult Download()
        {
            Response.BufferOutput = false;
            return new FileStreamResult(new BlobClient("connection-string", "container", "large.zip").OpenRead(), "application/zip")
            {
                FileDownloadName = "large.zip"
            };
        }
```

ただし、`Transfer-Encoding` が `chunked` になってしまうので、ブラウザ側でサイズが分からなくなり、ダウンロード残り時間なども表示されなくなってしまいます.

ちなみに ASP.NET Core にはこれらの問題はありません. (ASP.NET Core 3.1 で確認)

`OutOfMemoryException` になりませんし、`Transfer-Encoding: chunked` にもならないので、ダウンロードの残り時間もブラウザが表示してくれます. 古い ASP.NET MVC なんか使ってないで、さっさと ASP.NET Core MVC に移行しろってことですね.

```cs
        public ActionResult Download()
        {
            return new FileStreamResult(new BlobClient("connection-string", "container", "large.zip").OpenRead(), "application/zip")
            {
                FileDownloadName = "large.zip"
            };
        }
```
