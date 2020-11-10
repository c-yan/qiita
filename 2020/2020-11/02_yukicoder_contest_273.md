# yukicoder contest 273 参戦記

## [A 1279 Array Battle](https://yukicoder.me/problems/no/1279)

a<sub>i</sub>を降順にソートし、b<sub>i</sub>を昇順にソートして出した時の得点が常に最大値ではありそうだが、同じ最大値のものが複数ある場合いくつあるかを解く方法がなかなか思いつかない. よくよく考えると N≦9 で 9!≒3.6×10<sup>5</sup> なので、全部試すことができた.

```python
from itertools import permutations

N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

d = {}
for x in permutations(a):
    t = 0
    for i in range(N):
        t += max(x[i] - b[i], 0)
    d.setdefault(t, 0)
    d[t] += 1
print(d[max(d)])
```

## [B 1280 Beyond C](https://yukicoder.me/problems/no/1280)

確率はN×Mの組み合わせのうちCを超えている組み合わせの数をN×Mで割ったもの. b をソートすれば、ある a<sub>i</sub> との組み合わせでCを超える b<sub>i</sub> の数はにぶたんで *O*(log<i>M</i>) で求まるので、*O*((*N*+*M*)log<i>M</i>) となり解ける. a をソートしてもよい.

```python
from bisect import bisect_left

N, M, C = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

b.sort()
c = 0
for x in a:
    t = ((C + 1) + x - 1) // x
    c += M - bisect_left(b, t)
print(c / (N * M))
```

## [D 1282 Display Elements](https://yukicoder.me/problems/no/1282)

後に出したほうが大得点のチャンスなので、a を昇順に出していくのが最大値になりそう. 累積されていく b から a<sub>i</sub> より小さい枚数を求めるのににぶたんを使おうと思うと、ソートを維持しないといけなくてナイーブに毎回ソートすると *O*(*N*<sup>2</sup>log<i>N</i>) となり当然 TLE. ソート済配列を小さいのと大きいのの2つに分けて、毎回ソートするのは小さい方だけにして計算量を下げてやれば AC できる.

```python
from bisect import bisect_left

N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

a.sort()
t0 = []
t1 = []
result = 0
for i in range(N):
    result += bisect_left(t0, a[i])
    t1.append(b[i])
    t1.sort()
    result += bisect_left(t1, a[i])
    if len(t1) == 300:
        t0.extend(t1)
        t0.sort()
        t1.clear()
print(result)
```

追記: 想定解は座標圧縮+BITだったようなので、それでも解いた.

```python
def bit_add(bit, i, x):
    i += 1
    n = len(bit)
    while i <= n:
        bit[i - 1] += x
        i += i & -i


def bit_sum(bit, i):
    result = 0
    i += 1
    while i > 0:
        result += bit[i - 1]
        i -= i & -i
    return result


def bit_query(bit, start, stop):
    if start == 0:
        return bit_sum(bit, stop - 1)
    else:
        return bit_sum(bit, stop - 1) - bit_sum(bit, start - 1)


N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

t = {j: i for i, j in enumerate(sorted(set(a + b)))}
bit = [0] * len(t)

a.sort()
result = 0
for i in range(N):
    bit_add(bit, t[b[i]], 1)
    result += bit_query(bit, 0, t[a[i]])
print(result)
```

## [E 1283 Extra Fee](https://yukicoder.me/problems/no/1283)

一回だけの通行料金を支払わなくていい権利を使う前と使ったあとの DP テーブルを分けて幅優先探索したら AC した. ダイクストラ法要らなかった. 初期値を `math.MaxInt64` にしたらオーバーフローして死んで、`math.MaxInt32` にしたら今度は小さすぎて WA して、`math.MaxInt64 >> 1` に落ち着いた.

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

func main() {
	defer flush()

	N := readInt()
	M := readInt()

	c := make([][]int, N)
	for i := 0; i < N; i++ {
		c[i] = make([]int, N)
	}

	for i := 0; i < M; i++ {
		h := readInt() - 1
		w := readInt() - 1
		c[h][w] = readInt()
	}

	dpa := make([][]int, N)
	dpb := make([][]int, N)
	for i := 0; i < N; i++ {
		dpa[i] = make([]int, N)
		dpb[i] = make([]int, N)
		for j := 0; j < N; j++ {
			dpa[i][j] = math.MaxInt64 >> 1
			dpb[i][j] = math.MaxInt64 >> 1
		}
	}
	dpa[0][0] = 0

	q := make([][3]int, 0)
	q = append(q, [3]int{0, 0, 0})
	for len(q) != 0 {
		y := q[0][0]
		x := q[0][1]
		t := q[0][2]
		q = q[1:]

		if t == 0 {
			if y != 0 {
				if dpa[y-1][x] > dpa[y][x]+1+c[y-1][x] {
					dpa[y-1][x] = dpa[y][x] + 1 + c[y-1][x]
					q = append(q, [3]int{y - 1, x, 0})
				}
				if c[y-1][x] > 0 {
					if dpb[y-1][x] > dpa[y][x]+1 {
						dpb[y-1][x] = dpa[y][x] + 1
						q = append(q, [3]int{y - 1, x, 1})
					}
				}
			}
			if y != N-1 {
				if dpa[y+1][x] > dpa[y][x]+1+c[y+1][x] {
					dpa[y+1][x] = dpa[y][x] + 1 + c[y+1][x]
					q = append(q, [3]int{y + 1, x, 0})
				}
				if c[y+1][x] > 0 {
					if dpb[y+1][x] > dpa[y][x]+1 {
						dpb[y+1][x] = dpa[y][x] + 1
						q = append(q, [3]int{y + 1, x, 1})
					}
				}
			}
			if x != 0 {
				if dpa[y][x-1] > dpa[y][x]+1+c[y][x-1] {
					dpa[y][x-1] = dpa[y][x] + 1 + c[y][x-1]
					q = append(q, [3]int{y, x - 1, 0})
				}
				if c[y][x-1] > 0 {
					if dpb[y][x-1] > dpa[y][x]+1 {
						dpb[y][x-1] = dpa[y][x] + 1
						q = append(q, [3]int{y, x - 1, 1})
					}
				}
			}
			if x != N-1 {
				if dpa[y][x+1] > dpa[y][x]+1+c[y][x+1] {
					dpa[y][x+1] = dpa[y][x] + 1 + c[y][x+1]
					q = append(q, [3]int{y, x + 1, 0})
				}
				if c[y][x+1] > 0 {
					if dpb[y][x+1] > dpa[y][x]+1 {
						dpb[y][x+1] = dpa[y][x] + 1
						q = append(q, [3]int{y, x + 1, 1})
					}
				}
			}
		} else {
			if y != 0 {
				if dpb[y-1][x] > dpb[y][x]+1+c[y-1][x] {
					dpb[y-1][x] = dpb[y][x] + 1 + c[y-1][x]
					q = append(q, [3]int{y - 1, x, 1})
				}
			}
			if y != N-1 {
				if dpb[y+1][x] > dpb[y][x]+1+c[y+1][x] {
					dpb[y+1][x] = dpb[y][x] + 1 + c[y+1][x]
					q = append(q, [3]int{y + 1, x, 1})
				}
			}
			if x != 0 {
				if dpb[y][x-1] > dpb[y][x]+1+c[y][x-1] {
					dpb[y][x-1] = dpb[y][x] + 1 + c[y][x-1]
					q = append(q, [3]int{y, x - 1, 1})
				}
			}
			if x != N-1 {
				if dpb[y][x+1] > dpb[y][x]+1+c[y][x+1] {
					dpb[y][x+1] = dpb[y][x] + 1 + c[y][x+1]
					q = append(q, [3]int{y, x + 1, 1})
				}
			}
		}
	}
	println(min(dpa[N-1][N-1], dpb[N-1][N-1]))
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
