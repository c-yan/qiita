# Azure App Service でエラーを BLOB にロギングする

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201), ASP.NET Core 3.1 (3.1.3), Microsoft.Extensions.Logging.AzureAppServices 3.1.3 となります.

最低限エラーが残っていないと困るので…….

Azure Portal にログインして、該当の Azure App Service にアクセス. 左メニューの 監視 / App Service ログ から「アプリケーション ログ (Blob)」を設定.

nuget で Microsoft.Extensions.Logging.AzureAppServices をインストール. あとは Program.cs に AddAzureWebAppDiagnostics を追加すれば、Startup.cs の UseExceptionHandler の効果で、最低限 unhandled exception がロギングされるようになる.


```csharp:WebApplication1/Program.cs
                 .ConfigureWebHostDefaults(webBuilder =>
                 {
                     webBuilder.UseStartup<Startup>();
-                });
+                })
+                .ConfigureLogging(logging => logging.AddAzureWebAppDiagnostics());
     }
 }
```
