# Go 言語の path と path/filepath の違い

Go 言語でのプログラミング中に Delphi の`ChangeFileExt`、.NET Framework の`Path.ChangeExtension`に相当するものが欲しくなって調べたところ、ビルトインには残念ながら無いようだった.
そして不思議なことに渡した`path`の拡張子を取得する関数が2つある.
`path.Ext`と`filepath.Ext`だ.

それぞれのパッケージの Overview を読むに`path`パッケージはパスの区切り文字として`/`を固定で用い、`path/filepath`はOS定義の文字を用いるようだ. 確かに`path.Ext`を読むと`/`が固定で用いられており、`filepath.Ext`では`IsPathSeparator`を使用していた.

`IsPathSeparator`は[https://golang.org/src/os/path_windows.go#L13](https://golang.org/src/os/path_windows.go#L13)や[https://golang.org/src/os/path_unix.go#L15](https://golang.org/src/os/path_unix.go#L15)に定義されているとおりである.

正直なところ、Linux でアプリを書いている人が`path`パッケージを使って、Windowsでビルドしても動かないトラブルを起こす可能性が増えるだけなので`/`固定のビルトイン関数は廃止してほしいと思った.

それはさておき拡張子を置換するコードはこうなった.

```
func changeExt(path string, ext string) string {
	return path[:len(path)-len(filepath.Ext(path))] + ext
}
```

Python だったらスライスを取るときに負の値が使えて`path[:-len(filepath.Ext(path))]`で済むのになーと思った.
