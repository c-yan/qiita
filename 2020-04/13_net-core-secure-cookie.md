# ASP.NET Core アプリの Cookie に secure フラグを立てる

セキュリティ監査とかで secure フラグを立てないといけない人向け.

Startup.cs を開き、先頭に `using Microsoft.AspNetCore.Http;` を追加. 次に `ConfigureServices` メソッドに

```csharp
services.Configure<CookiePolicyOptions>(options =>
{
    options.Secure = CookieSecurePolicy.Always;
});
```

を追加する. 最後に `Configure` メソッドに

```csharp
app.UseCookiePolicy();
```

を追加する. どの行に設定するのがベストなのかはわからないが、`app.UseStaticFiles();` の次の行において動作することだけは確認した.
