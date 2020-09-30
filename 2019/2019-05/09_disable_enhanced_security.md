# Internet Explorer セキュリティ強化の構成 (IE Enhanced Security Configuration)をコマンドラインで無効にする

新規に Windows Server の VM を作成し、.NET Framework を最新化するためにダウンロードしようと思ったらダウンロードができない、このクソ設定め.

```powershell
$AdminPath = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}"
$UserPath = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}"
$AdminPath, $UserPath | % { Set-ItemProperty -Path $_ -Name "IsInstalled" -Value 0 }
Stop-Process -Name Explorer
```
