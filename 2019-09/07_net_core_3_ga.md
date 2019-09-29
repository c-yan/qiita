# .NET Core 3.0 のトピック箇条書き

[Announcing .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/) を斜め読みして、.NET Core 3.0 の **自分が** 興味のある点を箇条書きした. まあ、ほとんど既出ではある.

* 性能がものすごく上がった (Tiered Compilation 他)
* C# 8 になった
* .NET Standard 2.1 が出た
* Windows Desktop アプリ(Windows Forms, WPF) を作れるようになった
* exe になった(今までは dotnet コマンドで実行する dll)
* 11月に出る .NET Core 3.1 が LTS です(3.0 は LTS ではない)
* EXCEL の自動操作ができるような COM interop
* C# 8 の新機能
  * async stream
  * range/index
  * nullable reference types
  * interface のメンバのデフォルト実装
  * using 宣言(自動 dispose な using を使うと、インデントする必要があったのが無くなった)
  * switch 式
* 高性能な JSON API: Utf8JsonReader, Utf8JsonWriter, JsonDocument, JSON Serializer
* 新しい SqlClient (.NET Core / .NET Framework 共通)
* .NET Core バージョン API
* SSE2/SSE 等が使える Intrinsics
* 暗号の追加 (AES-GCM とか AES-CCM とか)
* ランタイムを含んだ単体 EXE ファイルの生成が可能に

去年末以来何の音沙汰もなかった .NET Standard 2.1 がしれっとでてきたことは驚いた. ~~Rfc2898DeriveBytes にちゃんとハッシュ関数を指定するコンストラクタが生えただろうか. 生えてなかったら呪詛.~~ さすがに生えていた。

LTS になる .NET Core 3.1 までの2ヶ月間はどういう開発をするのだろうか. ロードマップもマイルストーン(GitHub Issues の)もない状況でさっぱり分からない. バグ修正だけなんですかねえ.
