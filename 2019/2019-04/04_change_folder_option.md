# フォルダーオプションをコマンドラインで変更する

新しく作った VM で毎度フォルダーオプションを GUI で設定すると面倒くさいので、一発で変更する PowerShell スクリプト.

```powershell
# フォルダオプションの"登録されている拡張子は表示しない"のチェックを外す
Set-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced -name "HideFileExt" -Value 0
# フォルダオプションの"自動的に現在のフォルダーまで展開する"のチェックを入れる
Set-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced -name "NavPaneExpandToCurrentFolder" -Value 1
```

ちなみにレジストリのどこをイジればいいか調べたい場合には、GUI での設定変更前と変更後のレジストリをダンプしておいて、diff を取ればいい.

```
reg.exe export HKCU hkcu.reg
```

ただし、export したファイルは UTF-16 なので、diff する前に UTF-8 あたりに変更しないと、ツールによっては上手く diff が取れないかもしれない.
