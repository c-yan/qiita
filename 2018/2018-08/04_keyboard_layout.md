# Windows のキーボードレイアウトを日本語配列に変更する

IaaS で Windows Server を作成して RDP すると、キーボード配列が英語で面倒くさいのを一発で直す PowerShell スクリプト.

```powershell
Import-Module International
$x = New-WinUserLanguageList en-US
$x[0].InputMethodTips.Clear()
$x[0].InputMethodTips.Add("0409:00000411")
Set-WinUserLanguageList $x -Force
```
