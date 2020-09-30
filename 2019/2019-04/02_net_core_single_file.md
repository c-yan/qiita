# .NET Core 3.0 の目玉の single exe を preview3 で試してみたら出来なかった

.NET Core 3.0 の目玉の single exe がどれくらいのサイズになるのか興味があって、 preview3 で試してみようとしたら、出来なかった話.

~~それはそれとして、.NET Core SDK 3.0.100-preview3-010431 を入れても、Visual Studio 2019 がうんともすんとも .NET Core 3.0 を認識しないのは何故だろう. インストール画面眺めてたときに TargetPack がインストールされているのは確認できたのに…….~~

preview チャネルの VS2019 じゃないと駄目だよとダウンロードページに書かれていることに preview4 をダウンロードに行って気づいた orz. (推奨されないけど、設定で有効にすることもできる [.NET Core tooling update for Visual Studio 2017 version 15.9](https://devblogs.microsoft.com/dotnet/net-core-tooling-update-for-visual-studio-2017-version-15-9/))

というわけで、.NET Core 2.1 で ConsoleApp を作成し、.csproj の TargetFramework を手で netcoreapp3.0 に書き換えるところからスタート.

[designs/design.md at master · dotnet/designs](https://github.com/dotnet/designs/blob/master/accepted/single-file/design.md) を見ると

```
# Alias for dotnet publish /p:PublishSingleFile=true
dotnet publish --single-file
```

と書いてあるので実行すると

```
C:\Users\user\source\repos\ConsoleApp1>dotnet publish --single-file
Microsoft (R) Build Engine version 16.0.443+g5775d0d6bb for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

MSBUILD : error MSB1001: Unknown switch.
Switch: --single-file

For switch syntax, type "MSBuild -help"
```

全然駄目.
じゃあエイリアスじゃないほうでやるかーと.

```
C:\Users\user\source\repos\ConsoleApp1>dotnet publish -r win10-x64 --self-contained /p:PublishSingleFile=true
Microsoft (R) Build Engine version 16.0.443+g5775d0d6bb for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

C:\Program Files\dotnet\sdk\3.0.100-preview3-010431\Sdks\Microsoft.NET.Sdk\targets\Microsoft.NET.Sdk.TargetingPackResolution.targets(90,33): error MSB4030: "/p:PublishSingleFile=true" is an invalid value for the "SelfContained" parameter of the "ResolveFrameworkReferences" task. The "SelfContained" parameter is of type "System.Boolean".
```

やっぱり駄目.

そもそも

```
C:\Users\user\source\repos\ConsoleApp1>dir "C:\Program Files\dotnet\dotnet.exe" | findstr dotnet.exe
03/03/2019  04:44 PM           324,992 dotnet.exe
```

dotnet.exe はタイムスタンプから .NET Core 2.1 のモノっぽさ?

```xml
<PropertyGroup>
    <PublishSingleFile>true</PublishSingleFile>
</PropertyGroup>
```

同ページに .csproj で指定できるようなことが書かれているけど、書いて `dotnet publish -r win-x64 -c release --self-contained` してもやっぱり駄目.

ググったところ

[win-x64 publish with Core3 Preview and VS2019 results in ""CheckEmbeddedRootDescriptor" task was not given a value for the required parameter "AssemblyPath"" · Issue #210 · dotnet/winforms](https://github.com/dotnet/winforms/issues/210)

↓

[win-x64 publish with Core3 Preview and VS2019 results in ""CheckEmbeddedRootDescriptor" task was not given a value for the required parameter "AssemblyPath"" · Issue #428 · mono/linker](https://github.com/mono/linker/issues/428)

という感じで、preview2 の頃から動いていないのが分かっていて、preview3 でも直っていないようであった(合掌)

[Brainstorming - Creating a small single self-contained executable out of a .NET Core application - Scott Hanselman](https://www.hanselman.com/blog/BrainstormingCreatingASmallSingleSelfcontainedExecutableOutOfANETCoreApplication.aspx)

ここを見る限りは10MB未満でいけそうで大変期待できる.
