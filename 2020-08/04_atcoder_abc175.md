# AtCoder Beginner Contest 175 バーチャル参戦記

山に行ってて、時間までに帰ってこれなかったのでバーチャル参戦になった. AtCoder 始めてから、ABC に出れないのは初めて? 結果はABCD完(TLE1)で75:49でした. パフォ1465相当. 出ればレーティングアップだったか、ぐっすん

## [ABC175A - Rainy Season](https://atcoder.jp/contests/abc175/tasks/abc175_a)

2分で突破. 書くだけ.

```python
S = input()

t = 0
result = 0
for c in S:
    if c == 'R':
        t += 1
    else:
        t = 0
    result = max(result, t)
print(result)
```

## [ABC175B - Making Triangle](https://atcoder.jp/contests/abc175/tasks/abc175_b)

7分で突破. まず三角形の作れる辺の長さの条件がわからずググった. 次に「L<sub>i</sub>,L<sub>j</sub>,L<sub>k</sub>がすべて異なる」を読み落とした. 結果としてB問題にしては長い回答時間に.

```python
from itertools import combinations

N, *L = map(int, open(0).read().split())

result = 0
for a, b, c in combinations(L, 3):
    if a == b or b == c or c == a:
        continue
    if a + b > c and b + c > a and c + a > b:
        result += 1
print(result)
```

## [ABC175C - Walking Takahashi](https://atcoder.jp/contests/abc175/tasks/abc175_c)

10分で突破. 取り敢えず近づいて、後は行ったり来たりするだけだなとは、過去に類題を解いているのですぐ分かったが、あちこちでバグらせて無駄に時間がかかった.

```python
X, K, D = map(int, input().split())

if X > D:
    t = min(X // D, K)
    K -= t
    X -= t * D
else:
    t = min(-X // D, K)
    K -= t
    X += t * D

K %= 2

if K == 0:
    print(abs(X))
else:
    if abs(X + D) < abs(X - D):
        print(abs(X + D))
    else:
        print(abs(X - D))
```

## [ABC175D - Moving Piece](https://atcoder.jp/contests/abc175/tasks/abc175_d)

52分で突破. TLE×1. 最初「K 回以下」の「以下」を読み落としていて、ダブリングじゃ～んでコードを書いて、出力例1が7でないのは何故だとけっこう悩んだ. 「以下」に気づいて考え直し、O(<i>N</i><sup>2</sup>)でも通るなと思い、ループ検出をしてそのループでの合計がプラスなら可能なループ回数×ループでの合計を足してやればいいんだなと思って書いたが TLE. しょうがないので Go 言語で書き直したらサクッと通った.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
func main() {
	defer flush()

	N := readInt()
	K := readInt()

	P := make([]int, N)
	for i := 0; i < N; i++ {
		P[i] = readInt()
	}
	C := make([]int, N)
	for i := 0; i < N; i++ {
		C[i] = readInt()
	}

	result := math.MinInt64
	for i := 0; i < N; i++ {
		p := P[i] - 1
		c := C[p]
		k := K - 1
		t := c
		for p != i && k != 0 {
			p = P[p] - 1
			c += C[p]
			t = max(t, c)
			k--
		}
		if k == 0 || c <= 0 {
			result = max(result, t)
			continue
		}
		l := K - k
		c = c * (K/l - 1)
		k = K - (K/l-1)*l
		t = c
		for k != 0 {
			p = P[p] - 1
			c += C[p]
			t = max(t, c)
			k--
		}
		result = max(result, t)
	}
	println(result)
}

const (
	ioBufferSize = 1 * 1024 * 1024 // 1 MB
)

var stdinScanner = func() *bufio.Scanner {
	result := bufio.NewScanner(os.Stdin)
	result.Buffer(make([]byte, ioBufferSize), ioBufferSize)
	result.Split(bufio.ScanWords)
	return result
}()

func readString() string {
	stdinScanner.Scan()
	return stdinScanner.Text()
}

func readInt() int {
	result, err := strconv.Atoi(readString())
	if err != nil {
		panic(err)
	}
	return result
}

var stdoutWriter = bufio.NewWriter(os.Stdout)

func flush() {
	stdoutWriter.Flush()
}

func println(args ...interface{}) (int, error) {
	return fmt.Fprintln(stdoutWriter, args...)
}
```

Go 言語で書き直さず、PyPy で出せばいいだけだった.

```python
N, K = map(int, input().split())
P = list(map(int, input().split()))
C = list(map(int, input().split()))

result = -float('inf')
for i in range(N):
    p = P[i] - 1
    c = C[p]
    k = K - 1
    t = c
    while p != i and k != 0:
        p = P[p] - 1
        c += C[p]
        t = max(t, c)
        k -= 1
    if k == 0 or c <= 0:
        result = max(result, t)
        continue
    l = K - k
    c = c * (K // l - 1)
    k = K - (K // l - 1) * l
    t = c
    while k != 0:
        p = P[p] - 1
        c += C[p]
        t = max(t, c)
        k -= 1
    result = max(result, t)
print(result)
```

## [ABC175E - Picking Goods](https://atcoder.jp/contests/abc175/tasks/abc175_e)

突破できず. 「ただし、マス目の同じ行では 3 個までしかアイテムを拾うことができません。」が無ければ DP で簡単なのになあと思った. Dを飛ばしてEを解いている勢が結構いたので、何かが分かっていればそこまで難易度差はない?

追記: 冷静になって解いてみると簡単だったが、Go 言語では AC 出来るが、PyPy では TLE になってしまった. しょうがないので高速化を検討する. まず R×C×4 の3次元配列は要らない. 何故なら次の行に行くときは拾ったアイテム数がリセットされるからである. また、各列のここまでの最大値は同時には1つしか要らないので、dp の配列は長さ C の1次元配列だけあれば良い. 後は現在見ているマス目のときの情報を持つ長さ4の配列を用意してこれを更新しながら dp 配列を書き換えていけば良い.

```python
from sys import stdin
readline = stdin.readline

R, C, K = map(int, readline().split())

goods = [[0] * C for _ in range(R)]
for _ in range(K):
    r, c, v = map(int, readline().split())
    goods[r - 1][c - 1] = v

dp = [0] * (C + 1)
for i in range(R):
    cur = [0] * 4
    cur[0] = dp[0]
    for j in range(C):
        if goods[i][j] != 0:
            for k in range(2, -1, -1):
                cur[k + 1] = max(cur[k + 1], cur[k] + goods[i][j])
        dp[j] = max(cur)
        cur[0] = max(cur[0], dp[j + 1])

print(max(cur))
```

一応捻りのない Go 言語のコードも提示しておく.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

var (
	dp    [3001][3001][4]int
	goods [3000][3000]int
)

func main() {
	defer flush()

	R := readInt()
	C := readInt()
	K := readInt()
	for i := 0; i < K; i++ {
		r := readInt() - 1
		c := readInt() - 1
		v := readInt()
		goods[r][c] = v
	}

	for i := 0; i < R; i++ {
		for j := 0; j < C; j++ {
			if goods[i][j] != 0 {
				for k := 2; k >= 0; k-- {
					dp[i][j][k+1] = max(dp[i][j][k+1], dp[i][j][k]+goods[i][j])
				}
			}
			for k := 0; k < 4; k++ {
				dp[i][j+1][k] = max(dp[i][j+1][k], dp[i][j][k])
				dp[i+1][j][0] = max(dp[i+1][j][0], dp[i][j][k])
			}
		}
	}

	result := -1
	for k := 0; k < 4; k++ {
		result = max(result, dp[R-1][C-1][k])
	}

	println(result)
}

const (
	ioBufferSize = 1 * 1024 * 1024 // 1 MB
)

var stdinScanner = func() *bufio.Scanner {
	result := bufio.NewScanner(os.Stdin)
	result.Buffer(make([]byte, ioBufferSize), ioBufferSize)
	result.Split(bufio.ScanWords)
	return result
}()

func readString() string {
	stdinScanner.Scan()
	return stdinScanner.Text()
}

func readInt() int {
	result, err := strconv.Atoi(readString())
	if err != nil {
		panic(err)
	}
	return result
}

var stdoutWriter = bufio.NewWriter(os.Stdout)

func flush() {
	stdoutWriter.Flush()
}

func println(args ...interface{}) (int, error) {
	return fmt.Fprintln(stdoutWriter, args...)
}
```
