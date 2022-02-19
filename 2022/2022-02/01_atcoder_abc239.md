# デンソークリエイトプログラミングコンテスト2022(AtCoder Beginner Contest 239) 参戦記

## [ABC239A - Horizon](https://atcoder.jp/contests/abc239/tasks/abc239_a)

2分で突破. 書くだけ.

```python
from math import sqrt

H = int(input())

print(sqrt(H * (12800000 + H)))
```

## [ABC239B - Integer Division](https://atcoder.jp/contests/abc239/tasks/abc239_b)

4分半で突破. 試したら Python の除算そのままであれってなった.

```python
X = int(input())

print(X // 10)
```

## [ABC239C - Knight Fork](https://atcoder.jp/contests/abc239/tasks/abc239_c)

9分で突破. 2点の周辺を全探索すればいいかなと思い、素直にそれをすると TLE するので、2点が近い場合という条件を足した.

```python
x1, y1, x2, y2 = map(int, input().split())

def dist(x1, x2, y1, y2):
    return (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)

if dist(x1, x2, y1, y2) <= 100:
    l = min(x1, x2) - 5
    r = max(x1, x2) + 5
    t = min(y1, y2) - 5
    d = max(y1, y2) + 5
    for y in range(t, d + 1):
        for x in range(l, r + 1):
            if dist(x, x1, y, y1) == 5 and dist(x, x2, y, y2) == 5:
                print('Yes')
                exit()
print('No')
```

## [ABC239D - Prime Sum Game](https://atcoder.jp/contests/abc239/tasks/abc239_d)

5分半で突破. 全探索しても100×100=10<sup>4</sup>でしかないので、全探索すれば良い. 簡単すぎん?

```python
A, B, C, D = map(int, input().split())

primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199]

for x in range(A, B + 1):
    for y in range(C, D + 1):
        if x + y in primes:
            break
    else:
        print('Takahashi')
        exit()
print('Aoki')
```

## [ABC239E - Subtree K-th Max](https://atcoder.jp/contests/abc239/tasks/abc239_e)

86分で突破、TLE1、WA1. 1≤K<sub>i</sub>​≤20 と K<sub>i</sub>​ の値域が狭いので、全ての K<sub>i</sub> について先に計算すれば OK かなと思ったけど、意外と制限時間ギリギリだった.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

var (
	X     []int
	links [][]int
	lut   [21]map[int]int
)

func f(k int, i int, p int) []int {
	t := make([]int, 0, 20)
	t = append(t, X[i])
	for _, x := range links[i] {
		if x == p {
			continue
		}
		t = append(t, f(k, x, i)...)
	}
	sort.Sort(sort.Reverse(sort.IntSlice(t)))
	if len(t) >= k {
		lut[k][i] = t[k-1]
	}
	return t[:k]
}

func main() {
	defer flush()

	N := readInt()
	Q := readInt()
	X = make([]int, N)
	for i := 0; i < N; i++ {
		X[i] = readInt()
	}

	links = make([][]int, N)

	for i := 0; i < N-1; i++ {
		A := readInt() - 1
		B := readInt() - 1
		links[A] = append(links[A], B)
		links[B] = append(links[B], A)
	}

	for i := 0; i < Q; i++ {
		V := readInt() - 1
		K := readInt()
		if lut[K] == nil {
			lut[K] = make(map[int]int)
			f(K, 0, -1)
		}
		println(lut[K][V])
	}
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
