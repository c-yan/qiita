# 一定時間経過したらアクセスできなくなるデータキャッシュを使う (C#)

外部リソースから拾ってきたデータをキャッシュしたい場合、ずっとキャッシュすると外部リソース側が更新されたときに困る. そういう時とかに使えるデータキャッシュ. 以前はそういう用途が多い Web アプリケーション用に ASP.NET だけそういう機能があったが、今はデスクトップアプリでも普通に使えるようになった. 最後にアクセスしてから一定時間経過後にキャッシュを無効化したいのであれば `AbsoluteExpiration` の代わりに `SlidingExpiration` を使う.

とりあえず今回は有効期間1分で、10秒ごとにアクセスしてみた.


```csharp
using System;
using System.Runtime.Caching;
using System.Threading;

namespace ConsoleApp1
{
    class Program
    {
        static readonly ObjectCache Cache = MemoryCache.Default;

        static void Main(string[] args)
        {
            while (true)
            {
                var message = (string)Cache["test"];
                if (message == null)
                {
                    message = $"cached at {DateTime.Now:yyyy-MM-dd HH:mm:ss}";
                    Cache.Add("test", message, new CacheItemPolicy()
                    {
                        AbsoluteExpiration = DateTime.Now.AddMinutes(1)
                    });
                }
                Console.WriteLine(message);
                Thread.Sleep(TimeSpan.FromSeconds(10));
            }
        }
    }
}
```

なお、System.Runtime.Caching 使用時には Install-Package System.Runtime.Caching で nuget からインストールする必要がある、念の為.
