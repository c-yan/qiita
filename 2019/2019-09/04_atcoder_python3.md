# Python で AtCoder をするあれこれ

## 入力

概ね以下5種類のどれかになる. 大量の行を読むには `input` が遅いことだけ分かっていれば問題ないかと思われる.

### 整数が1つ書かれた行

```python
x = int(input())
```

### 文字列が1つ書かれた行

```python
s = input()
```

### 整数が複数書かれた行

```python
x, y, z = map(int, input().split())

or

# xyz = [int(t) for t in input().split()] より速い
xyz = list(map(int, input().split()))

or

# xyz = list(map(lambda x: int(x) - 1, input().split())) より速い
xyz = [int(t) - 1 for t in input().split()]
```

### 整数が複数書かれた複数行

大量の行を読むときは、`input` だと遅いので `stdin.readline` を使う.
使わないと、実行制限時間2秒の4割の0.8秒を読み込みに取られたりする可能性がある.
改行が残ってしまうが、`int` が読み捨ててくれるので問題なし.

```python
from sys import stdin
for _ in range(n):
  x, y, z = map(int, stdin.readline().split())
  ...

or

xyzs = [list(map(int, stdin.readline().split())) for _ in range(n)]
```

### 文字列が書かれた複数行

文字列の場合には改行が残ることが問題になるので、`[:-1]` スライスで改行を除去する.

```python
from sys import stdin
for _ in range(n):
  # stdin.readline().rstrip('\n') より速い
  s = stdin.readline()[:-1]
  ...

or

ss = [stdin.readline()[:-1] for _ in range(n)]
```

## 出力

概ね以下3種類のどれかになる.

### 一つ出力

```python
print(x)
```

### 空白区切りで複数出力

```python
print(*xs)
```

### 改行区切りで複数出力

```python
print(*xs, sep='\n')
```

## リスト(配列)の用意

DP(動的計画法 / Dynamic Programming) を含め、計算途中で配列を埋めるようなプログラムは多いので、作成の仕方を理解しておく必要がある.

### 1次元リスト

```python
# [0 for _ in range(n)] より速い
xs = [0] * n
```

### 2次元リスト

```python
# [[0] * m] * n とすると、t[0][0] = 1 とした瞬間に t[i][0] が全て1になるので注意
t = [[0] * m for _ in range(n)]
```

## 高速化

Python は残念ながら遅く、PyPy が代わりに AC を通してくれる場合もあるが、Python で小細工をしないと AC が取れないことがある. 以下はそういった小細工である. また、どんなに小細工をしてもダメな問題も稀にある.

### 変数のローカル化

Python ではグローバル変数とローカル変数で速度が違う(※)ので、コードを関数の中に入れてやるだけで速くなる.

main 関数を作って、その下にコードを入れて、main を呼んでやれば良い.

```python
...
...
...

↓

def main():
  ...
  ...
  ...
main()
```

※ Python インタプリタでは、変数のロードは、グローバル変数の場合は LOAD_GLOBAL、ローカル変数の場合は LOAD_FAST と別のバイトコードになる. どちらも *O*(*1*) ではあるが、LOAD_GLOBAL は辞書(ハッシュテーブル)にアクセスするような速度、LOAD_FAST は配列にアクセスするような速度になる.

### 関数のローカル化

変数同様に関数もグローバル関数とローカル関数で速度が違う. 関数の中に `import` してやれば、ローカル関数となり速くなる.

```python
def main():
  from builtins import input, int, map
  ...
  ...
main()
```

### sys.stdin.readline の高速化

`stdin.readline` と書くより、メソッドを変数に入れてしまったほうが速い.

```python
from sys import stdin
def main():
  readline = stdin.readline
  A, B = map(int, readline().split())
  ...
  ...
main()
```

### リスト(配列)アクセス

`a[i][j]` みたいな場合、Python は毎回2回リスト(配列)アクセスをするので遅い. 以下のような2重ループになっている場合には１つ目のリスト(配列)にアクセスしたものを変数に入れておくと速くできる.

```python
for i in range(Y):
  for j in range(X):
    a[i + 1][j] = ... a[i][j] ...

↓

for i in range(Y):
  ai = a[i]
  ai1 = a[i + 1]
  for j in range(X):
    ai1[j] = ... ai[j] ...
```

### 部分リスト(配列)への代入

部分リスト(配列)への代入はループを回すより、スライスにリスト(配列)を代入するほうが速い.

```python
for x in range(i, j):
  a[x] = y

↓

a[i:j] = [y] * (j - i)
```

[ABC129D - Lamp](https://atcoder.jp/contests/abc129/tasks/abc129_d) でしか役に立たなそうだけど(笑).

### 無限大

Python では `float('inf')` で無限大が利用できるが、そのまま書いてしまうと、都度関数呼び出しなので低速となる. 最初に変数に代入して、その変数を使うと速くなる.

```python
INF = float('inf')
```

`float('inf')` は正の整数を足そうが、掛けようが値が変わらなくて便利だが、制約が許すなら大きな整数を使用したほうがやや高速になる.

```python
INF = 10 ** 18
```

特に PyPy は INF として、64bit 範囲内の値を使わないと著しく遅くなる. 計算途中で INF への加算が出てくることもあるので、制約が許すなら 2<sup>63</sup>-1 などのギリギリは狙わないほうがいい.

### 反転

反転には `reversed` 関数や `reverse` メソッドなどがあるが、使える場合には `[::-1]` スライスがベスト. 特に文字列はこれでないと、文字リストになってしまい、`''.join` で文字列に戻す羽目になる.

```python
s = ''.join(reversed(input()))

↓

s = input()[::-1]
```

### 文字列の累積

Python の文字列の足し算はとても遅い. ループで足し込むと *O*(*N*<sup>2</sup>) になる. リストで足し込んで最後に `join` すれば *O*(<i>N</i>log<i>N</i>) になる.

```python
result = ''
for c in s:
  if ...:
    result += c
print(result)

↓

result = []
for c in s:
  if ...:
    result.append(c)
print(''.join(result))
```

### 数値演算より if 文を使う

if は CPU が分岐予測に失敗するとパイプラインが乱れて遅くなる原因であり、できるだけ避けるべきものなのだが、避けるために数値演算をしてしまうと、Python の数値演算は遅いため逆効果になってしまう.

CPU の処理では加算、減算、ビット演算(AND, OR, XOR 等)、シフト演算等は高速で、乗算がちょっと遅く、除算がだいぶ遅いのだが、Python の場合はどの演算もだいたい同じ速度で全部遅い. というのは、演算が行われるたびに long オブジェクトが malloc されるため、malloc 律速になっているためである. なので if 文を回避するために演算を一個増やすと、それが加算であったとしても、逆に遅くなるのである.

```python
x += speed * direction

↓

if direction == 1:
  x += speed
else:
  x -= speed
```

### min, max より if 文を使う

関数呼び出しは重いので、if 文で書くほうが速くなる.

```python
dp[i + a] = max(dp[i + a], dp[i] + b)

↓

if dp[i] + b > dp[i + a]:
  dp[i + a] = dp[i] + b
```

### くり返し同じループをするときはリスト(配列)を使う

くり返し同じループをするときは、range をリスト(配列)に変換すると速くなる. range は1ループ毎に long オブジェクトを生成するため malloc をするのだが、リスト(配列)にしてしまえばそのコストはループ一周分だけで済む.

```python
for i in range(Y):
  for j in range(X):
    ...

↓

listX = list(range(X))
for i in range(Y):
  for j in listX:
    ...
```

### インデックスの要らないループでは range を使わない

単にループを回したいだけであれば、range を使わずに回したい回数の長さのすべて同じ値が入ったリスト(配列)を使うほうが速くなる.

```python
for _ in range(N):
  ..

↓

for _ in [0] * N:
  ..
```

### Lookup Table (LUT) を使う

LUT を使うことによって同じ計算をやり直すことを回避するのは、別に Python だけのテクニックではないが、数値演算が遅い Python では更に輝く.

```python
for j in range(13):
  for k in range(10):
    dp[i + 1][(j * 10 + k) % 13] += dp[i][j]

↓

lut = [i % 13 for i in range(130)]
for j in range(13):
  for k in range(10):
    dp[i + 1][lut[j * 10 + k]] += dp[i][j]
```

### 逆元を求めるときに -1 を引数に渡す

剰余の逆元を必要とする問題が AtCoder では比較的多く出題され、`pow(x, p - 2, p)` (p は 1000000007 であることが多く、ついで 998244353 が多い)のようにフェルマーの小定理を用いて求めることができる. Python 3.8 から、第2引数に `-1` を渡すと、線形ディオファントス方程式を用いてやや高速に計算してくれる. AtCoder の現状の PyPy は 7.3 で Python 3.7 相当なので、`-1` を渡すとエラーになるので注意.

## その他

### 再帰限界数の設定

コードの中に再帰関数があって、入出力例が通って、提出したら RE になった場合はだいたいこれ. Python は再帰関数の再帰回数の制限が厳しく、設定を変えないとだいたい通らない. AtCoder ではメモ化再帰で再帰関数をよく使うので忘れないよう注意.

```python
from sys import setrecursionlimit
setrecursionlimit(1000000)
```

### メモ化再帰

Python ではビルトインの `lru_cache` を使って簡単にメモ化再帰が実装できる. `lru_cache` をデコレータで関数に適用するだけとなる.

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def dfs(x):
  ...
```
