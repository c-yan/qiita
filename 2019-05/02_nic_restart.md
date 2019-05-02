# コマンドラインからネットワークアダプタを停止して、起動する

論よりコード.

```
wmic PATH Win32_NetworkAdapter where "index = 4" call disable
wmic PATH Win32_NetworkAdapter where "index = 4" call enable
```

index は事前に `wmic PATH Win32_NetworkAdapter` を叩いて、対象のネットワークアダプタの InterfaceIndex の値をチェックしておく.
言うまでもなく管理者権限でコマンドプロンプトを起動してないと動かないので注意.
