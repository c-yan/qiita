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
