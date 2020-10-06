# メモリが足りていても OutOfMemoryException が出る可能性がある (C#)

Microsoft の [OutOfMemoryException Class](https://docs.microsoft.com/en-us/dotnet/api/system.outofmemoryexception?view=netcore-3.1) のドキュメントに以下のような記載があります.

> An OutOfMemoryException exception has two major causes:
> - You are attempting to expand a StringBuilder object beyond the length defined by its StringBuilder.MaxCapacity property.

適当訳:

> OutOfMemoryException 例外には2つの大きな要因があります:
> - StringBuilder.MaxCapacity プロパティで定義された長さを超えて StringBuilder オブジェクトを拡大しようとしている。

つまりメモリが足りていたとしても、`StringBuilder` に `MaxCapacity` を超えて文字を追加しようとすると `OutOfMemoryException` が発生します.

でもそれってメモリ不足と一致するんでしょって思いますよね? 実はそうではありません.

[StringBuilder.MaxCapacity Property](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder.maxcapacity?view=netcore-3.1)

```cs
public int MaxCapacity { get; }
```

`MaxCapacity` は Int32 なんです. なので最大文字数は `Int32.MaxValue` = 2147483648 となります. つまりざっくりで 4GB となります(.NET の `Char` は 16bit です). 結果として 64bit 環境ではメモリが余っていても OutOfMemoryException が発生することがあります.

お手軽便利でみんな大好き `File.ReadAllText` は内部で `StreamReader.ReadToEnd` を呼び出しており、`StreamReader.ReadToEnd` は内部で `StringBuilder` を使っているため、`File.ReadAllText` は ASCII 文字のみのファイルの場合は最大でも 2GB までのファイルしか読み込めません. 悲しいね!
