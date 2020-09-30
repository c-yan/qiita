# OpenSSL で暗号化したデータを C# で復号する

2006年頭に Java で書いて、2012年7月に DESede から AES に書き直したコードの C# 移植.
`-md md5` だと

```
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
```

と怒られるようになったので、`-pbkdf2` に変更.
デフォルトではハッシュアルゴリズムは sha256, イテレーション回数は 10000 のようだ(OpenSSL 1.1.1c 現在).
暗号化も AES-128-CBC から AES-256-CBC に変更してみた. (AES-128 に問題はないけど)

```sh
$ echo -n 'test' | openssl enc -e -a -pbkdf2 -aes-256-cbc -pass pass:passw0rd
U2FsdGVkX1+wtVTcahFqx0V/PigNukzjVdMyDRMGBhI=
```

この base64 化された暗号文を、C# 側で平文化できるのを確認した.
同様に C# 側で作成した base64 化された暗号文を、OpenSSL 側で平文化できるのを確認した.

```sh
$ echo 'U2FsdGVkX1+ymkCPB8EdLXrTUPQcL8q1q0f8YEryZ/Y=' | openssl enc -d -a -pbkdf2 -aes-256-cbc -pass pass:passw0rd
test
```

Rfc2898DeriveBytes のコンストラクタの引数に HashAlgorithmName のオプションが増えたのは .NET Framework 4.7.2 / .NET Core 2.0 以降で、以下のコードは .NET Standard 2.0 ではビルドできない. (追記: .NET Standard 2.1 でビルドできるようになった)

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Security.Cryptography;
using System.Text;

namespace ConsoleApp1
{
    class Program
    {
        static byte[] GenerateSalt(int saltLength)
        {
            var result = new byte[saltLength];
            using (var csp = new RNGCryptoServiceProvider())
            {
                csp.GetBytes(result);
                return result;
            }
        }

        static byte[] GenerateKIV(byte[] salt, string password, HashAlgorithmName hashAlgorithm, int iterationCount, int size)
        {
            return new Rfc2898DeriveBytes(Encoding.UTF8.GetBytes(password), salt, iterationCount, hashAlgorithm)
                .GetBytes(size);
        }

        public static string Encrypt(byte[] plainBytes, string password)
        {
            var salt = GenerateSalt(8);
            var kiv = GenerateKIV(salt, password, HashAlgorithmName.SHA256, 10000, 48).ToList();
            using (var csp = new AesManaged())
            {
                csp.KeySize = 256;
                csp.BlockSize = 128;
                csp.Mode = CipherMode.CBC;
                csp.Padding = PaddingMode.PKCS7;
                csp.Key = kiv.GetRange(0, 32).ToArray();
                csp.IV = kiv.GetRange(32, 16).ToArray();
                using (var encryptor = csp.CreateEncryptor())
                using (var mstream = new MemoryStream())
                {
                    mstream.Write(Encoding.UTF8.GetBytes("Salted__"), 0, 8);
                    mstream.Write(salt, 0, 8);
                    using (var cstream = new CryptoStream(mstream, encryptor, CryptoStreamMode.Write))
                    {
                        cstream.Write(plainBytes, 0, plainBytes.Length);
                    }
                    return Convert.ToBase64String(mstream.ToArray());
                }
            }
        }

        public static byte[] Decrypt(string encryptedString, string password)
        {
            var encryptedBytes = Convert.FromBase64String(encryptedString);
            var salt = encryptedBytes.Skip(8).Take(8).ToArray();
            var kiv = GenerateKIV(salt, password, HashAlgorithmName.SHA256, 10000, 48).ToList();
            using (var csp = new AesManaged())
            {
                csp.KeySize = 256;
                csp.BlockSize = 128;
                csp.Mode = CipherMode.CBC;
                csp.Padding = PaddingMode.PKCS7;
                csp.Key = kiv.GetRange(0, 32).ToArray();
                csp.IV = kiv.GetRange(32, 16).ToArray();
                using (var decryptor = csp.CreateDecryptor())
                using (var mstream1 = new MemoryStream(encryptedBytes.Skip(16).ToArray()))
                using (var cstream = new CryptoStream(mstream1, decryptor, CryptoStreamMode.Read))
                using (var mstream2 = new MemoryStream())
                {
                    cstream.CopyTo(mstream2);
                    return mstream2.ToArray();
                }
            }
        }

        static void Main(string[] args)
        {
            //Console.WriteLine(Encrypt(Encoding.UTF8.GetBytes("test"), "passw0rd"));
            Console.WriteLine(Encoding.UTF8.GetString(Decrypt("U2FsdGVkX1+wtVTcahFqx0V/PigNukzjVdMyDRMGBhI=", "passw0rd")));
            Console.Read();
        }
    }
}
```
