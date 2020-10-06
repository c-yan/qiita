# Update-Package をしたら Could not load file or assembly で Azure Functions が動かなくなった (C#)

Azure Portal で Azure Functions v2 から v3 に更新することを検討しろと言われたので、上げてみたところ Azure Functions が動かなくなった.

例外を確認してみると

```
System.IO.FileNotFoundException: Could not load file or assembly 'System.IdentityModel.Tokens.Jwt, Version=6.7.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. The system cannot find the file specified.
File name: 'System.IdentityModel.Tokens.Jwt, Version=6.7.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
```

と、もう見慣れてきた感のあるやつです.

[Could not load file or assembly 'System.IdentityModel.Tokens.Jwt, Version=6.7.1.0 #460](https://github.com/Azure/azure-functions-vs-build-sdk/issues/460) の途中にワークアラウンドとして [Could not load file or assembly System.IdentityModel.Tokens.Jwt, Version=5.6.0.0 #397](https://github.com/Azure/azure-functions-vs-build-sdk/issues/397) が紹介されており、

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <AzureFunctionsVersion>v3</AzureFunctionsVersion>
    <_FunctionsSkipCleanOutput>true</_FunctionsSkipCleanOutput>
  </PropertyGroup>
```

のように `<_FunctionsSkipCleanOutput>true</_FunctionsSkipCleanOutput>` を追加したところ動作するようになりました.

.NET の依存性管理、しょっちゅうトラブルになるのでなにか根本的に問題があるんじゃないかと思うけど、.NET 5 になって、.NET Core / .NET Standard / .NET Framework の三つ巴が終わったら解決するのかなあ. Azure Functions も v2 は .NET Standard 2.0 だったのに v3 は .NET Standard 2.1 ではなく .NET Core 3.1 という辺り混乱しすぎだろって感じる.
