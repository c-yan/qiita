# 「中央ディレクトリが壊れています」という意味不明なエラーが出たときの対処方法

C# のプロジェクトをビルドしている途中で以下のようなエラーが出た場合の対処方法です.

```
C:\xxx\.nuget\NuGet.targets(100,9): error : 中央ディレクトリが壊れています。 [C:\xxx\yyy\zzz.csproj]
C:\xxx\.nuget\NuGet.targets(100,9): error :   ファイルの先頭よりも前にファイル ポインターを移動しようとしました。 [C:\xxx\yyy\zzz.csproj]
C:\xxx\.nuget\NuGet.targets(100,9): error MSB3073: コマンド ""C:\xxx\.nuget\NuGet.exe" install "C:\xxx\yyy\packages.config" -source ""  -NonInteractive -RequireConsent -solutionDir "C:\xxx\ "" はコード 1 で終了しました。 [C:\xxx\yyy\zzz.csproj]
```

おそらく0バイトの .nupkg ファイルが出来ているので、ソリューションの中の packages フォルダを削除して、再取得させましょう.

中央ディレクトリとは、zip 形式の central directory のことです.
