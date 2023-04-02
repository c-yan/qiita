# bindingRedirect の再生成をする

\# 以下の記事が書かれた時の版数は .NET Framework 4.8 となります.

`app.config` の `bindingRedirect` がおかしくなったときの再生成方法. というか、Git submodule と `bindingRedirect` の相性悪い.

1. app.config の `bindingRedirect` を全部手で消す
2. Visual Studio のパッケージマネージャのコンソールで `Add-BindingRedirect` を実行する
