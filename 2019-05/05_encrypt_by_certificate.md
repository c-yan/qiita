# 証明書で暗号化する (PowerShell)

PKCS#7 をベースに作られた RFC 3852 CMS (Cryptographic Message Syntax) を利用する.

暗号化側のコード.
公開鍵が必要.

```powershell
[void] [Reflection.Assembly]::LoadWithPartialName("System.Security")
$plainText = "secret message"
$content = New-Object Security.Cryptography.Pkcs.ContentInfo (,[Text.Encoding]::UTF8.GetBytes($plainText))
$ai = New-Object Security.Cryptography.Pkcs.AlgorithmIdentifier (New-Object Security.Cryptography.Oid "2.16.840.1.101.3.4.1.2") # aes128-CBC
$env = New-Object Security.Cryptography.Pkcs.EnvelopedCms $content, $ai
$cert = Get-Item Cert:\CurrentUser\My\25653B72F81733249EB18E53D8AF6D9C79485EA4
$env.Encrypt((New-Object Security.Cryptography.Pkcs.CmsRecipient($cert)))
$result = [Convert]::ToBase64String($env.Encode())
```

復号側のコード.
秘密鍵が必要.

```powershell
[void] [Reflection.Assembly]::LoadWithPartialName("System.Security")
$encryptedText = "MIIB9gYJKoZIhvcNAQcDoIIB5zCCAeMCAQAxggGeMIIBmgIBADCBgTBpMQswCQYDVQQGEwJKUDEOMAwGA1UECAwFVG9reW8xDDAKBgNVBAcMA090YTEPMA0GA1UECgwGY3lhbmV0MREwDwYDVQQLDAhQZXJzb25hbDEYMBYGA1UEAwwPZW5jcnlwdGlvbiB0ZXN0AhQsGXvNo7nyxnBGULV65zHPq4MTHjANBgkqhkiG9w0BAQcwAASCAQCKbEgWL4ZfPALJXbvGEOS5nDfEzLRgn08/vEE8Ktg9ZzAfo9g7Eioq9I2q3GjW9E61mZh2iHACPUyFO1qC84yBqt5mQTxpcI/PQTYUuwXLumX2AL50E79Xw48Jzt0FJgMXLWE02tJcJ+chspNOKKOOorq3pmbUz/pWxYlX3Dax7TTvqB99gsOHfxu+sHCYPBtnkCPEzF15tCfwn5xVinaG4wHaLWoZuo1y0G0h+aDVYTdRbzzngm4W9zcZ7LmVKm8THTZr2JZ3sqOGv23cDFMeIndWZYszxj0/HTPApCbuavXRrTeGEJAieCXFqJ6clnUXrEI2PepNMe5LmSrtAnyoMDwGCSqGSIb3DQEHATAdBglghkgBZQMEAQIEEJutl+SI5+4LTblQDMcMHKqAEGjf2nsGD9qVI88m3TKE1D4="
$env = New-Object Security.Cryptography.Pkcs.EnvelopedCms
$env.Decode([Convert]::FromBase64String($encryptedText))
$env.Decrypt()
$result = [Text.Encoding]::UTF8.GetString($env.ContentInfo.Content)
```

電文の中に証明書情報が埋まっているので、復号側は証明書の指定が不要だが、証明書ストアから秘密鍵が取得できないとエラーになる.
(電文の構造は `openssl asn1parse` で確認できる)

一応、以下のように整形してやれば OpenSSL でも復号できた.
S/MIME のヘッダは `$ echo "secret message" | openssl cms -encrypt -recip public.key` で出てきた電文から流用した.
ちなみにボディが1行のままだとエラーになった(笑).

```
MIME-Version: 1.0
Content-Disposition: attachment; filename="smime.p7m"
Content-Type: application/pkcs7-mime; smime-type=enveloped-data; name="smime.p7m"
Content-Transfer-Encoding: base64

MIIB9gYJKoZIhvcNAQcDoIIB5zCCAeMCAQAxggGeMIIBmgIBADCBgTBpMQswCQYD
VQQGEwJKUDEOMAwGA1UECAwFVG9reW8xDDAKBgNVBAcMA090YTEPMA0GA1UECgwG
Y3lhbmV0MREwDwYDVQQLDAhQZXJzb25hbDEYMBYGA1UEAwwPZW5jcnlwdGlvbiB0
ZXN0AhQsGXvNo7nyxnBGULV65zHPq4MTHjANBgkqhkiG9w0BAQcwAASCAQCKbEgW
L4ZfPALJXbvGEOS5nDfEzLRgn08/vEE8Ktg9ZzAfo9g7Eioq9I2q3GjW9E61mZh2
iHACPUyFO1qC84yBqt5mQTxpcI/PQTYUuwXLumX2AL50E79Xw48Jzt0FJgMXLWE0
2tJcJ+chspNOKKOOorq3pmbUz/pWxYlX3Dax7TTvqB99gsOHfxu+sHCYPBtnkCPE
zF15tCfwn5xVinaG4wHaLWoZuo1y0G0h+aDVYTdRbzzngm4W9zcZ7LmVKm8THTZr
2JZ3sqOGv23cDFMeIndWZYszxj0/HTPApCbuavXRrTeGEJAieCXFqJ6clnUXrEI2
PepNMe5LmSrtAnyoMDwGCSqGSIb3DQEHATAdBglghkgBZQMEAQIEEJutl+SI5+4L
TblQDMcMHKqAEGjf2nsGD9qVI88m3TKE1D4=
```

```sh
$ cat encrypted.txt | openssl cms -decrypt -inkey private.key
secret message
```
