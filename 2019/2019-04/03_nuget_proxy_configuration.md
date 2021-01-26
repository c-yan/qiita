# NuGet のプロキシ設定

\# 以下の記事が書かれた時の版数は NuGet Version: 4.9.2.5706 / 5.8.1.7021 となります.

NuGet の設定は %APPDATA%\NuGet\NuGet.Config に入っています.
(%APPDATA% は一般的には C:\Users\%USERNAME%\AppData\Roaming)

ただ設定はコマンドラインでやったほうが早いです

```
NuGet.exe config -set http_proxy=http://proxyserver:8080
```

恐ろしいことに認証プロキシである場合は、以下のように ID / PW も指定します.

```
NuGet.exe config -set http_proxy.user=xxx
NuGet.exe config -set http_proxy.password=yyy
```

設定を確認する場合は以下のようにやります.

```
> NuGet.exe config http_proxy
http://proxyserver:8080
```
