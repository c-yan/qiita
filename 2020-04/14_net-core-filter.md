# ASP.NET Core で全アクション共通で実行するロジックを実行する

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201), ASP.NET Core 3.1 (3.1.3) となります.

例えば `_Layout.cshtml` 内で本日のドル円レートを表示するとして、全てのコントローラの、全てのアクションに `ViewData` への代入を書いて回るのは流石にエレファント. そういう場合はどうすればいいかという話です.

プロジェクトに `Filters` フォルダを掘って、`MyFilter.cs` を作ります. 以下のように `IActionFilter` を継承したクラスを作ります. 非同期の場合は `IAsyncActionFilter` を継承します. `OnActionExecuting` はアクション実行前、`OnActionExecuted` はアクション実行後に発火します. `context.Controller` を `Controller` にキャストすると `ViewData` にアクセスできます.

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Filters;

namespace WebApplication1.Filters
{
    public class MyFilter : IActionFilter
    {
        public void OnActionExecuting(ActionExecutingContext context)
        {
        }

        public void OnActionExecuted(ActionExecutedContext context)
        {
            var c = context.Controller as Controller;
            c.ViewData["foo"] = "bar";
        }
    }
}
```

このフィルタが使われるようにするため、`Startup.cs` を開き、`ConfigureServices` メソッドの中の `AddControllersWithViews` を以下のように書き換えます.

```csharp
services.AddControllersWithViews(options =>
{
    options.Filters.Add(typeof(MyFilter));
});
```
