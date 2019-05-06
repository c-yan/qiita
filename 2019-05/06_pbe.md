# OpenSSL で暗号化したデータを PowerShell で復号する

2006年頭に Java で書いて、2012年7月に DESede から AES に書き直したコードの PowerShell 移植.
いつの間にか deprecated になってる上に、鍵導出に使うハッシュアルゴリズムのデフォルト値も md5 から sha256 に変更されているし(笑).

```sh
$ openssl version
OpenSSL 1.1.1b  26 Feb 2019
$ echo -n "secret message" | openssl enc -e -aes-128-cbc -md md5 -a -pass pass:password
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
U2FsdGVkX18U/R0E7RLGe1YX9DMAQ7lxSaUpRngdj8s=
```

```powershell
PS> $encryptedText = [Convert]::FromBase64String("U2FsdGVkX18U/R0E7RLGe1YX9DMAQ7lxSaUpRngdj8s=")
PS> $salt = $encryptedText[8..15]
PS> $md5 = [Security.Cryptography.MD5]::Create()
PS> $seed = [Text.Encoding]::UTF8.GetBytes("password") + $salt
PS> $kiv = $md5.ComputeHash($seed)
PS> $kiv += $md5.ComputeHash($kiv + $seed)
PS> $md5.Dispose()
PS> $aes = New-Object Security.Cryptography.AesManaged
PS> $aes.Key = $kiv[0..15]
PS> $aes.IV = $kiv[16..31]
PS> $aes.BlockSize = 128
PS> $aes.Mode = [Security.Cryptography.CipherMode]::CBC
PS> $aes.Padding = [Security.Cryptography.PaddingMode]::PKCS7
PS> $decryptor = $aes.CreateDecryptor()
PS> [Text.Encoding]::UTF8.GetString($decryptor.TransformFinalBlock($encryptedText[16..($encryptedText.Length - 1)], 0, $encryptedText.Length - 16))
secret message
PS> $decryptor.Dispose()
PS> $aes.Dispose()
PS>
```

PowerShell で暗号化したデータを OpenSSL で復号するのは以下.

```powershell
PS> $plainText = "secret message"
PS> $salt = 0..7 | % { Get-Random -Maximum 255 }
PS> $md5 = [Security.Cryptography.MD5]::Create()
PS> $seed = [Text.Encoding]::UTF8.GetBytes("password") + $salt
PS> $kiv = $md5.ComputeHash($seed)
PS> $kiv += $md5.ComputeHash($kiv + $seed)
PS> $md5.Dispose()
PS> $aes = New-Object Security.Cryptography.AesManaged
PS> $aes.Key = $kiv[0..15]
PS> $aes.IV = $kiv[16..31]
PS> $aes.BlockSize = 128
PS> $aes.Mode = [Security.Cryptography.CipherMode]::CBC
PS> $aes.Padding = [Security.Cryptography.PaddingMode]::PKCS7
PS> $decryptor = $aes.CreateEncryptor()
PS> $plainBytes = [Text.Encoding]::UTF8.GetBytes($plainText)
PS> [Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("Salted__") + $salt + $decryptor.TransformFinalBlock($plainBytes, 0, $plainBytes.Length))
U2FsdGVkX18h9BZVxCv8LUEoORGQg4XKivd4mq6r/q4=
PS> $decryptor.Dispose()
PS> $aes.Dispose()
PS>
```

```sh
$ echo -n "U2FsdGVkX18h9BZVxCv8LUEoORGQg4XKivd4mq6r/q4=" | base64 -d | openssl enc -d -aes-128-cbc -md md5 -pass pass:password
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
secret message
```
