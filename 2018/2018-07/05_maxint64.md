# math.MaxInt64 の代入に注意 (Go)

`math.MaxInt64` は当然 `int32` には入り切らないわけですが

```golang
x := math.MaxInt64
```

と記述すると、`x` の型は `int` となってしまいます.
(`math.MaxInt64` は `const MaxInt64  = 1<<63 - 1` と型を設定せずに定義されているので)

64bit 環境では何も問題ありませんが、32bit 環境では `constant 9223372036854775807 overflows int` でコンパイルエラーになります.

64bit/32bit どちらでも動作するようにするのであれば `math.MaxInt32` に変更するか、

```golang
x := int64(math.MaxInt64)
```

と記述しましょう.

追記: Go 1.17 で `math.MaxInt` が追加されたので、64bit/32bit どちらでも動作するようにするのであれば、これがお勧めになりました.
