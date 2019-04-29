# Rfc2898DeriveBytes と OpenSSL PBKDF2 で相互運用できることを確認した

RFC の規格で相互運用できなかったら詐欺じゃーんの世界だけど、念のため.

```csharp
Microsoft (R) Roslyn C# Compiler version 2.10.0.0
Loading context from 'CSharpInteractive.rsp'.
Type "#help" for more information.
> using System.Security.Cryptography;
> new Rfc2898DeriveBytes("pass", Encoding.ASCII.GetBytes("saltsalt"), 1000).GetBytes(20)
byte[20] { 100, 56, 132, 29, 150, 178, 121, 214, 208, 80, 62, 64, 40, 83, 211, 156, 131, 69, 34, 128 }
```

```ruby
$ irb
irb(main):001:0> require 'openssl'
=> true
irb(main):002:0> OpenSSL::PKCS5.pbkdf2_hmac("pass", "saltsalt", 1000, 20, "sha1").unpack("C*")
=> [100, 56, 132, 29, 150, 178, 121, 214, 208, 80, 62, 64, 40, 83, 211, 156, 131, 69, 34, 128]
```

おっけー.

一応注意事項はあって、

```csharp
> new Rfc2898DeriveBytes("pass", Encoding.ASCII.GetBytes("salt"), 1000).GetBytes(20)
Salt is not at least eight bytes.
  + System.Security.Cryptography.Rfc2898DeriveBytes.set_Salt(byte[])
  + System.Security.Cryptography.Rfc2898DeriveBytes..ctor(byte[], byte[], int, System.Security.Cryptography.HashAlgorithmName)
  + System.Security.Cryptography.Rfc2898DeriveBytes..ctor(string, byte[], int)
```

```ruby
irb(main):003:0> OpenSSL::PKCS5.pbkdf2_hmac("pass", "salt", 1000, 20, "sha1").unpack("C*")
=> [59, 207, 44, 177, 178, 30, 14, 32, 48, 209, 13, 36, 152, 42, 10, 175, 56, 141, 117, 36]
```

Rfc2898DeriveBytes は規格に則って8バイト未満の salt を弾くけど、OpenSSL は気にせず計算する.
