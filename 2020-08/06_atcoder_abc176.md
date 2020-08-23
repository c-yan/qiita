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

func main() {
	defer flush()

	H := readInt()
	W := readInt()
	Ch := readInt() - 1
	Cw := readInt() - 1
	Dh := readInt() - 1
	Dw := readInt() - 1
	S := make([]string, H)
	for i := 0; i < H; i++ {
		S[i] = readString()
	}

	t := make([][]int, H)
	for i := 0; i < H; i++ {
		t[i] = make([]int, W)
		for j := 0; j < W; j++ {
			if S[i][j] == '#' {
				t[i][j] = -1
			} else {
				t[i][j] = math.MaxInt64
			}
		}
	}

	t[Ch][Cw] = 0
	q := make([][2]int, 0, 1024)
	q = append(q, [2]int{Ch, Cw})

	for len(q) != 0 {
		warpq := make([][2]int, 0, 1024)
		for len(q) != 0 {
			h, w := q[0][0], q[0][1]
			warpq = append(warpq, [2]int{h, w})
			q = q[1:]
			if h-1 >= 0 && t[h-1][w] > t[h][w] {
				q = append(q, [2]int{h - 1, w})
				t[h-1][w] = t[h][w]
			}
			if h+1 < H && t[h+1][w] > t[h][w] {
				q = append(q, [2]int{h + 1, w})
				t[h+1][w] = t[h][w]
			}
			if w-1 >= 0 && t[h][w-1] > t[h][w] {
				q = append(q, [2]int{h, w - 1})
				t[h][w-1] = t[h][w]
			}
			if w+1 < W && t[h][w+1] > t[h][w] {
				q = append(q, [2]int{h, w + 1})
				t[h][w+1] = t[h][w]
			}
		}

		if t[Dh][Dw] != math.MaxInt64 {
			break
		}

		for i := 0; i < len(warpq); i++ {
			h, w := warpq[i][0], warpq[i][1]
			for i := max(0, h-2); i <= min(H-1, h+2); i++ {
				for j := max(0, w-2); j <= min(W-1, w+2); j++ {
					if t[i][j] <= t[h][w]+1 {
						continue
					}
					t[i][j] = t[h][w] + 1
					q = append(q, [2]int{i, j})
				}
			}
		}
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

カリカリにチューンする羽目になったが、Python でもなんとか通せた.

```python
from collections import deque


def main():
    from sys import stdin
    readline = stdin.readline

    from builtins import max, min, range

    INF = 10 ** 6

    H, W = map(int, readline().split())
    Ch, Cw = map(lambda x: int(x) - 1, readline().split())
    Dh, Dw = map(lambda x: int(x) - 1, readline().split())
    S = [readline()[:-1] for _ in range(H)]

    t = [[INF] * W for _ in range(H)]
    for h in range(H):
        th = t[h]
        Sh = S[h]
        for w in range(W):
            if Sh[w] == '#':
                th[w] = -1

    t[Ch][Cw] = 0
    q = deque([(Ch, Cw)])
    a = 0
    while q:
        warpq = []
        while q:
            h, w = q.popleft()
            warpq.append((h, w))
            if h - 1 >= 0 and t[h - 1][w] > a:
                q.append((h - 1, w))
                t[h - 1][w] = a
            if h + 1 < H and t[h + 1][w] > a:
                q.append((h + 1, w))
                t[h + 1][w] = a
            if w - 1 >= 0 and t[h][w - 1] > a:
                q.append((h, w - 1))
                t[h][w - 1] = a
            if w + 1 < W and t[h][w + 1] > a:
                q.append((h, w + 1))
                t[h][w + 1] = a

        if t[Dh][Dw] != INF:
            break

        a += 1
        for h, w in warpq:
            for i in range(max(0, h - 2), min(H, h + 3)):
                ti = t[i]
                for j in range(max(0, w - 2), min(W, w + 3)):
                    if ti[j] > a:
                        ti[j] = a
                        q.append((i, j))

    if t[Dh][Dw] == INF:
        print(-1)
    else:
        print(t[Dh][Dw])


main()
```

## [ABC176E - Bomber](https://atcoder.jp/contests/abc176/tasks/abc176_e)

突破できず. あと10分早くD問題が解けていたら……. というか、Dを飛ばしてこっちに手を付けていたら…….

追記: D問題より明らかにこっちのほうが簡単. 一番爆破対象の多い縦と横をそれぞれ選ぶ(複数候補がある可能性があることに注意). 爆弾設置位置に爆破対象がある場合はダブルカウントになるので1引く必要がある. 爆弾設置位置に爆破対象があるかを単純な二次元配列で覚えようとすると 6×10<sup>10</sup> ビット= 7.5GB の記憶容量が必要になるので、位置のタプルの set で記憶する.

```python
H, W, M = map(int, input().split())

rows = [0] * H
cols = [0] * W
s = set()
for _ in range(M):
    h, w = map(lambda x: int(x) - 1,input().split())
    rows[h] += 1
    cols[w] += 1
    s.add((h, w))

max_rows = max(rows)
max_cols = max(cols)

max_rows_indexes = [i for i in range(H) if rows[i] == max_rows]
max_cols_indexes = [i for i in range(W) if cols[i] == max_cols]

duplicate = True
if M >= len(max_rows_indexes) * len(max_cols_indexes):
    for h in max_rows_indexes:
        for w in max_cols_indexes:
            if (h, w) not in s:
                duplicate = False
                break
        if not duplicate:
            break
else:
    duplicate = False

if duplicate:
    print(max_rows + max_cols - 1)
else:
    print(max_rows + max_cols)
```
