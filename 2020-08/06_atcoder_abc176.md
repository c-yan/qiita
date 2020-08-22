# AtCoder Beginner Contest 176 参戦記

## [ABC176A - Takoyaki](https://atcoder.jp/contests/abc176/tasks/abc176_a)

2分で突破. 書くだけ.

```python
N, X, T = map(int, input().split())

print((N + X - 1) // X * T)
```

## [ABC176B - Multiple of 9](https://atcoder.jp/contests/abc176/tasks/abc176_b)

1分半で突破. 書くだけ.

```python
N = input()

if sum(int(c) for c in N) % 9 == 0:
    print('Yes')
else:
    print('No')
```

Python では以下でも良かったらしい. なんだかなあ.

```python
N = int(input())

if N % 9 == 0:
    print('Yes')
else:
    print('No')
```

## [ABC176C - Step](https://atcoder.jp/contests/abc176/tasks/abc176_c)

3分くらいで突破. 前の人のほうが高ければ、前の人と同じ高さまで台で足すを繰り返すだけ.

```python
N, *A = map(int, open(0).read().split())

result = 0
p = A[0]
for i in range(1, N):
    if p >= A[i]:
        result += p - A[i]
    else:
        p = A[i]
print(result)
```

## [ABC176D - Wizard in Maze](https://atcoder.jp/contests/abc176/tasks/abc176_d)

81分で突破、TLE×2. 下手に実装すると1回のワープで行けるところに2回で行ってしまいそうで気を使った. キューが一つだとワープの処理で *O*(*HW*) のスキャンが必要になってしまうので、キューを2つにして、BFS で訪れたマスからだけワープを試みるようにしたら、AC できる計算量になった.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

// S
var (
	S []string
	t [][]int
	H int
	W int
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

func printIntln(v ...int) {
	if len(v) == 0 {
		return
	}
	b := make([]byte, 0, 4096)
	for i := 0; i < len(v)-1; i++ {
		b = append(b, strconv.Itoa(v[i])...)
		b = append(b, " "...)
	}
	b = append(b, strconv.Itoa(v[len(v)-1])...)
	fmt.Println(string(b))
}

func dfs(q [][2]int) [][2]int {
	nq := make([][2]int, 0)
	for len(q) != 0 {
		h, w := q[0][0], q[0][1]
		nq = append(nq, [2]int{h, w})
		q = q[1:]
		if h-1 >= 0 && S[h-1][w] != '#' && t[h-1][w] == math.MaxInt64 {
			q = append(q, [2]int{h - 1, w})
			nq = append(nq, [2]int{h - 1, w})
			t[h-1][w] = t[h][w]
		}
		if h+1 < H && S[h+1][w] != '#' && t[h+1][w] == math.MaxInt64 {
			q = append(q, [2]int{h + 1, w})
			nq = append(nq, [2]int{h + 1, w})
			t[h+1][w] = t[h][w]
		}
		if w-1 >= 0 && S[h][w-1] != '#' && t[h][w-1] == math.MaxInt64 {
			q = append(q, [2]int{h, w - 1})
			nq = append(nq, [2]int{h, w - 1})
			t[h][w-1] = t[h][w]
		}
		if w+1 < W && S[h][w+1] != '#' && t[h][w+1] == math.MaxInt64 {
			q = append(q, [2]int{h, w + 1})
			nq = append(nq, [2]int{h, w + 1})
			t[h][w+1] = t[h][w]
		}
	}
	return nq
}

func main() {
	defer flush()

	H = readInt()
	W = readInt()
	Ch := readInt() - 1
	Cw := readInt() - 1
	Dh := readInt() - 1
	Dw := readInt() - 1
	S = make([]string, H)
	for i := 0; i < H; i++ {
		S[i] = readString()
	}

	t = make([][]int, H)
	for i := 0; i < H; i++ {
		t[i] = make([]int, W)
		for j := 0; j < W; j++ {
			t[i][j] = math.MaxInt64
		}
	}

	t[Ch][Cw] = 0
	q := make([][2]int, 0)
	q = append(q, [2]int{Ch, Cw})
	nq := dfs(q)
	if t[Dh][Dw] != math.MaxInt64 {
		println(t[Dh][Dw])
		return
	}

	for {
		q := make([][2]int, 0)
		for len(nq) != 0 {
			h, w := nq[0][0], nq[0][1]
			nq = nq[1:]
			for i := max(0, h-2); i <= min(H-1, h+2); i++ {
				for j := max(0, w-2); j <= min(W-1, w+2); j++ {
					if S[i][j] == '#' {
						continue
					}
					if t[i][j] != math.MaxInt64 {
						continue
					}
					t[i][j] = t[h][w] + 1
					q = append(q, [2]int{i, j})
				}
			}
		}
		if len(q) == 0 {
			break
		}
		nq = dfs(q)
		if t[Dh][Dw] != math.MaxInt64 {
			break
		}
	}

	for h := 0; h < H; h++ {
		//printIntln(t[h]...)
	}

	if t[Dh][Dw] == math.MaxInt64 {
		println(-1)
	} else {
		println(t[Dh][Dw])
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

## [ABC176E - Bomber](https://atcoder.jp/contests/abc176/tasks/abc176_e)

突破できず. あと10分早くD問題が解けていたら……. というか、Dを飛ばしてこっちに手を付けていたら…….

追記: D問題より明らかにこっちのほうが簡単. 一番爆破対象の多い縦と横をそれぞれ選ぶ(複数候補がある可能性があることに注意). 爆弾設置位置に爆破対象がある場合はダブルカウントになるので1引く必要がある. 爆弾設置位置に爆破対象があるかを単純な二次元配列で覚えようとすると 6×10<sup>10</sup> ビット= 7.5GB の記憶容量が必要になるので、辞書で記憶する.

```python
H, W, M = map(int, input().split())

hc = [0] * H
wc = [0] * W
d = {}
for _ in range(M):
    h, w = map(lambda x: int(x) - 1,input().split())
    hc[h] += 1
    wc[w] += 1
    d[(h, w)] = 1

maxh = max(hc)
maxw = max(wc)

result = maxh + maxw - 1
wi = [i for i in range(W) if wc[i] == maxw]
for h in [i for i in range(H) if hc[i] == maxh]:
    for w in wi:
        if (h, w) not in d:
            result = maxh + maxw
            break
print(result)
```
