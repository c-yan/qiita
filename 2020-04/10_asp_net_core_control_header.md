# Azure Web Apps にデプロイした ASP.NET Core アプリのレスポンスヘッダを制御する

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201), ASP.NET Core 3.1 (3.1.3) となります.

セキュリティ監査とかでヘッダを制御しないといけない人向け.

まず、そもそも Azure Web Apps ではアプリは IIS 上にホスティングされるので、IIS の設定で制御できる. IIS の設定は当然 web.config です.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <remove name="X-Powered-By"/>
        <add name="X-Custom-By-Configuration" value="USO800"/>
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
```

これで X-Powered-By を消しつつ、X-Custom-By-Configuration というフィールドをレスポンスヘッダに追加できます.

コードでやりたい場合には Startup.cs の Configure の中で

```csharp
app.Use((context, next) =>
{
    context.Response.Headers["X-Custom-By-Code"] = "USO800";
    return next.Invoke();
});
```

みたいに書けば、X-Custom-By-Code というフィールドをレスポンスヘッダに追加できます.
