# 動的コンテンツだけをキャッシュしないようにする (ASP.NET Core)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.24) となります.

セキュリティー監査ツールにコンテンツをキャッシュしないようにしろと怒られて、`Web.config` に以下のように記載したところ、静的コンテンツまで巻き込まれてキャッシュされなくなってしまいました.

```xml
--- a/WebApplication1/Web.config
+++ b/WebApplication1/Web.config
@@ -6,6 +6,9 @@
                <httpProtocol>
                        <customHeaders>
                                <remove name="X-Powered-By"/>
+                               <add name="Cache-Control" value="no-store"/>
+                               <add name="Pragma" value="no-cache"/>
+                               <add name="Expires" value="-1"/>
                        </customHeaders>
                </httpProtocol>
        </system.webServer>
```

色々知らべた結果、以下のように UseStaticFiles の後にキャッシュしないようにするレスポンスヘッダを出力するミドルウェアを書けば良さそうでした.

```csharp
--- a/WebApplication1/Startup.cs
+++ b/WebApplication1/Startup.cs
@@ -42,6 +42,18 @@ namespace WebApplication1
             app.UseHttpsRedirection();
             app.UseStaticFiles();

+            app.Use(async (context, next) =>
+            {
+                context.Response.OnStarting(() =>
+                {
+                    context.Response.Headers["Cache-Control"] = "no-store";
+                    context.Response.Headers["Pragma"] = "no-cache";
+                    context.Response.Headers["Expires"] = "-1";
+                    return Task.CompletedTask;
+                });
+                await next();
+            });
+
             app.UseRouting();

             app.UseAuthorization();
```

コントローラに `ResponseCache` をデコレーションして回るのも良さそうですが、`Pragma` が出なさそうです. こんな HTTP 1.0 時代の設定止めたいのですが、監査ツールがゴミなのでしょうがない.
