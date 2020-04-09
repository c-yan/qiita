# AtCoder Beginner Contest 144 参戦記

## [ABC144A - 9x9](https://atcoder.jp/contests/abc144/tasks/abc144_a)

2分で突破. 書くだけ.

```python
A, B = map(int, input().split())

if A < 10 and B < 10:
    print(A * B)
else:
    print(-1)
```

## [ABC144B - 81](https://atcoder.jp/contests/abc144/tasks/abc144_b)

7分で突破. 書くだけだったんだけど、コードテストが正しく動かなくて(何故か標準出力が戻ってこない)、無駄に時間を使ってしまった.

```python
from sys import exit

N = int(input())

for i in range(1, 10):
    for j in range(1, 10):
        if N == i * j:
            print('Yes')
            exit()
print('No')
```

## [ABC144C - Walk on Multiplication Table](https://atcoder.jp/contests/abc144/tasks/abc144_c)

5分で突破. できるだけ小さい数の掛け合わせがいいので、sqrt から下ろしていけばいいよなと瞬時に思いついたので、あとは書くだけだったのだが、コードテストが不調でローカルで入出力例を動かす羽目になったので時間が無駄にかかってしまった.

```python
from math import sqrt
from sys import exit

N = int(input())

for i in range(int(sqrt(N)) + 1, -1, -1):
    if N % i == 0:
        print((i - 1) + (N // i - 1))
        exit()
```

## [ABC144D - Water Bottle](https://atcoder.jp/contests/abc144/tasks/abc144_d)

突破できず. WA, RE が1個づつ残ってしまった. 解説を見たら、半分未満の方の式の atan を asin と取り違えていた. 三角関数なんて嫌いだー. (というか、atan とか初めて使ったぞ)

```python
from math import atan, pi

a, b, x = map(int, input().split())

if x >= a * a * b / 2:
    print(atan((a*a*b-x)/(a*a*a/2))/(2 * pi)*360)
else:
    print(90 - atan(x/(a*b*b/2))/(2 * pi)*360)
```

## [ABC144E - Gluttony](https://atcoder.jp/contests/abc144/tasks/abc144_e)

突破できず. A を昇順ソートして、F を降順ソートするところまではひと目だったけど、A を1づつ減らしていくのは K≤10<sup>18</sup> だから TLE 必死だしでフリーズして終了.

解説動画を見てゴールの方を二分探索で動かすのかーと感動. 動かすものが違うだけの Educational DP Contest の2つのナップサック問題を思い出して、応用できなかったなあと思ったり. 予習してはあったけど、過去問で必要とする問題にまだ遭遇できてなくて使ったことがなかっためぐる式二分探索を初めて書いて AC.

```python
# PyPy なら通る
def is_ok(x):
    result = 0
    for i in range(N):
        a = A[i]
        t = x // F[i]
        if t < a:
            result += a - t
    return result <= K


N, K = map(int, input().split())
A = list(map(int, input().split()))
F = list(map(int, input().split()))

A.sort()
F.sort(reverse=True)

l = -1
r = A[-1] * F[0]
while r > l+1:
    m = l + (r - l) // 2
    if is_ok(m):
        r = m
    else:
        l = m
print(r)
```

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func main() {
	N := readInt()
	K := readInt()
	A := readInts(N)
	F := readInts(N)

	sort.Ints(A)
	sort.Sort(sort.Reverse(sort.IntSlice(F)))

	l := -1            // ng
	r := A[N-1] * F[0] // ok
	for r > l+1 {
		m := l + (r-l)/2
		isOk := func(x int) bool {
			result := 0
			for i := 0; i < N; i++ {
				t := A[i] - x/F[i]
				if t > 0 {
					result += t
				}
			}
			return result <= K
		}
		if isOk(m) {
			r = m
		} else {
			l = m
		}
	}
	fmt.Println(r)
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

func readInts(n int) []int {
	result := make([]int, n)
	for i := 0; i < n; i++ {
		result[i] = readInt()
	}
	return result
}
```
