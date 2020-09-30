# Easy Auth で認証したユーザIDを入手する

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201), ASP.NET Core 3.1 (3.1.3) となります.

Azure App Service に組み込まれた Easy Auth を使った Azure AD 認証連携、設定が簡単すぎて感動的. ただ、`User.Identity.Name` が空だったのでコードを書いた.

Visual Studio 2019 の新しいプロジェクトの作成で「ASP.NET Core Web アプリケーション」を選び、ASP.NET Core 3.1 の Web アプリケーション(モデル ビュー コントローラ) を選んで生成されるソースコードがまず出発地点.

次に Startup.cs を開き、先頭に `using System.Security.Principal;` を追加して、`Configure` メソッドの `app.UseAuthorization();` と `app.UseEndpoints(endpoints =>` の間に

```csharp
app.Use(async (context, next) =>
{
    if (context.Request.Headers.ContainsKey("X-MS-CLIENT-PRINCIPAL-NAME"))
    {
        var azureAppServicePrincipalNameHeader = context.Request.Headers["X-MS-CLIENT-PRINCIPAL-NAME"][0];
        var identity = new GenericIdentity(azureAppServicePrincipalNameHeader);
        context.User = new GenericPrincipal(identity, null);
    }
    await next.Invoke();
});
```

を埋め込む. 要するに Easy Auth はリクエストヘッダの `X-MS-CLIENT-PRINCIPAL-NAME` にユーザ ID を入れてくれている.

あとは `Views/Home/Index.cshtml` を開いて `<h1 class="display-4">Welcome</h1>` を `<h1 class="display-4">Welcome @User.Identity.Name</h1>` に書き換えるだけ.

デプロイして、確認. 画面に Azure AD にログインしたユーザ ID が表示された. OK!!
