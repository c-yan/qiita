# PowerShell で ArrayShuffle

PowerShell で ArrayShuffle をしたくなったので調べた.

一つ目は Get-Random のドキュメントで Randomize an entire collection と Microsoft が説明しているくらいなので正攻法. -Count に指定した数だけ、重複しないように配列から乱択してくれる.

```powershell
PS> 1..10 | Get-Random -Count ([int]::MaxValue)
1
4
7
2
8
6
10
5
3
9
```

二つ目はちょいハック. ドキュメントを見てもどこに書いてあるのか分からないのだが、スクリプトブロックを渡すと、そのブロックで出力した値を使って並び替えてくれる模様なので、そこでランダム値を出力すればシャッフルになる.

```powershell
PS> 1..10 | Sort-Object { Get-Random }
10
2
6
8
7
1
3
5
4
9
```
