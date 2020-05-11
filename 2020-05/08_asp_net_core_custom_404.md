# ASP.NET Core でカスタム 404 エラーページを返す

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.201), ASP.NET Core 3.1 (3.1.3) となります.

Visual Studio 2019 の新しいプロジェクトの作成で「ASP.NET Core Web アプリケーション」を選び、ASP.NET Core 3.1 の Web アプリケーション(モデル ビュー コントローラ) を選んで生成されるソースコードだと 404 エラーは、レスポンスボディが無い状態となる. UseStatusCodePagesWithRedirects か UseStatusCodePagesWithReExecute を使うことによってカスタムエラーページを表示することができる.

どちらが良いかだが、

1. UseStatusCodePagesWithRedirects だと 302 レスポンスの後に 200 レスポンスが返るので、本来のステータスコードが消えてしまう.
2. URL 欄に typo して 404 エラーになったときは、ちょちょっと直せば正しく表示できると思うが、UseStatusCodePagesWithRedirects だと 302 リダイレクトされてしまうので URL が変わってしまい、また最初から書き直しになる.

の2点からして、UseStatusCodePagesWithReExecute の一択だと思う. ということで、Startup.cs に UseStatusCodePagesWithReExecute を追加して、あとは HomeController の Error メソッドをチョチョイっと改造して完成.

```csharp:WebApplication1/Controllers/HomeController.cs
         }

         [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
-        public IActionResult Error()
+        public IActionResult Error(int? id)
         {
+            if ((id ?? 500) == 404)
+            {
+                return View("Error404");
+            }
             return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
         }
     }
```

```csharp:WebApplication1/Startup.cs

         // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
-            if (env.IsDevelopment())
-            {
-                app.UseDeveloperExceptionPage();
-            }
-            else
-            {
-                app.UseExceptionHandler("/Home/Error");
-                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
-                app.UseHsts();
-            }
+            app.UseExceptionHandler("/Home/Error");
+            // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
+            app.UseHsts();
+            app.UseStatusCodePagesWithReExecute("/Home/Error/{0}");
+
             app.UseHttpsRedirection();
             app.UseStaticFiles();

```

```WebApplication1/Views/Shared/Error404.cshtml
+@{
+    ViewData["Title"] = "Error404";
+}
+
+<h1>カスタム Error404</h1>
+
```
