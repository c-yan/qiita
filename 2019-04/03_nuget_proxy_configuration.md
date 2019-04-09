# NuGet のプロキシ設定

NuGet の設定は %APPDATA%\NuGet\NuGet.Config に入っています.
(%APPDATA% は一般的には C:\Users\%USERNAME%\AppData\Roaming)

ただ設定はコマンドラインでやったほうが早いです

```
NuGet.exe config -set http://proxyserver:8080
```

恐ろしいことに認証プロキシである場合は、以下のように ID / PW も指定します.

```
NuGet.exe config -set http_proxy.user=xxx
NuGet.exe config -set http_proxy.password=yyy
```
