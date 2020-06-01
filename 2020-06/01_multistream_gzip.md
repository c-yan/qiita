# マルチストリーム GZIP は GZipStream でそのまま読み込める (C#)

\# 以下の記事が書かれた時の版数は .NET Core 3.1 (3.1.300) となります.

あるシステムで複数回追記しないといけない CSV(中間ファイル)が大きくて困っていて、サイズ・データのチャンクの連続にするか、データ・サイズのチャンクの連続にするか悩んでいたのだが(※1)、[.NET Frameworkのbzip2ライブラリを調査](https://qiita.com/7shi/items/235328dbdc5c0c85edcb) の記事で世の中にマルチストリーム bzip2 というものがあることに気づいた. bzip2 だと外部ライブラリが必要でちょっと嫌なので、世の中にマルチストリーム GZIP は存在しないのかを調査した. Go 言語のビルトインの GZIP パッケージは[マルチストリーム対応](https://golang.org/pkg/compress/gzip/#Reader.Multistream)していた. また gzip コマンドも普通に対応していた.

```sh
$ echo -n "hello " | gzip > /tmp/hello.gz
$ echo -n "world" | gzip >> /tmp/hello.gz
$ gzip -dc /tmp/hello.gz
hello world
```

.NET の `GZipStream` を試したところ、マルチストリームでもそのまま読み込めた. マルチストリームで BOM 付き UTF-8 を書き出すのめんどくさいっすね…….

```cs
[TestMethod]
public void GZipMultiStreamTest()
{
    var text1 = "Hello ";
    var text2 = "World!";
    var expected = text1 + text2;
    var ms = new MemoryStream();
    using (var writer = new StreamWriter(new GZipStream(ms, CompressionLevel.Optimal, true), Encoding.UTF8))
    {
        writer.Write(text1);
    }
    using (var writer = new StreamWriter(new GZipStream(ms, CompressionLevel.Optimal), new UTF8Encoding(false)))
    {
        writer.Write(text2);
    }
    ms = new MemoryStream(ms.ToArray());
    using (var reader = new StreamReader(new GZipStream(ms, CompressionMode.Decompress), Encoding.UTF8))
    {
        Assert.AreEqual(expected, reader.ReadToEnd());
    }
}
```

※1: xxx.001.csv.gz, xxx.002.csv.gz みたいな連番ファイルにすることや、xxx.csv.zip の中に 001.csv, 002.csv みたいな連番データを入れることも考えたが、002 以降に CSV ヘッダを入れても入れなくても微妙だなあと思いボツに. 前者はファイル数が多いことも微妙.
