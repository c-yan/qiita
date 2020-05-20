# AtCoder Beginner Contest 151 参戦記

## [ABC151A - Next Alphabet](https://atcoder.jp/contests/abc151/tasks/abc151_a)

1分半で突破. 書くだけ.

```python
C = input()

print(chr(ord(C[0]) + 1))
```

## [ABC151B - Achieve the Goal](https://atcoder.jp/contests/abc151/tasks/abc151_b)

4分で突破. 書くだけ.

```python
N, K, M = map(int, input().split())
A = list(map(int, input().split()))

t = sum(A)
if N * M > t + K:
    print(-1)
else:
    print(max(N * M - t, 0))
```

## [ABC151C - Count Order](https://atcoder.jp/contests/abc151/tasks/abc151_c)

7分半で突破. AC 後の WA は無視しないといけないところだけを気にすればよい.

```python
N, M = map(int, input().split())

ac = [False] * N
wa = [0] * N
for _ in range(M):
    p, S = input().split()
    p = int(p) - 1
    if S == 'AC':
        ac[p] = True
    else:
        if not ac[p]:
            wa[p] += 1

a = 0
b = 0
for i in range(N):
    if ac[i]:
        a += 1
        b += wa[i]
print(*[a, b])
```

## [ABC151D - Maze Master](https://atcoder.jp/contests/abc151/tasks/abc151_d)

17分で突破. 見た瞬間に AtCoder Typical Contest 002A - 幅優先探索 を思い出したので、全箇所から幅優先探索して、最長をかき集める方向性で解いた.

```python
H, W = map(int, input().split())
S = [input() for _ in range(H)]


def f(i, j):
    t = [[-1] * W for _ in range(H)]
    t[i][j] = 0
    q = [(i, j)]
    while q:
        y, x = q.pop(0)
        if y - 1 >= 0 and S[y - 1][x] != '#' and t[y - 1][x] == -1:
            t[y - 1][x] = t[y][x] + 1
            q.append((y - 1, x))
        if y + 1 < H and S[y + 1][x] != '#' and t[y + 1][x] == -1:
            t[y + 1][x] = t[y][x] + 1
            q.append((y + 1, x))
        if x - 1 >= 0 and S[y][x - 1] != '#' and t[y][x - 1] == -1:
            t[y][x - 1] = t[y][x] + 1
            q.append((y, x - 1))
        if x + 1 < W and S[y][x + 1] != '#' and t[y][x + 1] == -1:
            t[y][x + 1] = t[y][x] + 1
            q.append((y, x + 1))
    return max(max(tt) for tt in t)


result = 0
for i in range(H):
    for j in range(W):
        if S[i][j] != '#':
            result = max(result, f(i, j))
print(result)
```

追記: ワーシャルフロイドでも解ける.

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

func warshallFloyd(n int, d [][]int) {
	for k := 0; k < n; k++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				d[i][j] = min(d[i][j], d[i][k]+d[k][j])
			}
		}
	}
}

func main() {
	H := readInt()
	W := readInt()

	S := make([]string, H)
	for i := 0; i < H; i++ {
		S[i] = readString()
	}

	n := H * W
	d := make([][]int, n)
	for i := 0; i < n; i++ {
		d[i] = make([]int, n)
		fillInts(d[i], math.MaxInt32)
		d[i][i] = 0
	}

	for y := 0; y < H; y++ {
		for x := 0; x < W; x++ {
			if S[y][x] == '#' {
				continue
			}
			if y-1 >= 0 && S[y-1][x] != '#' {
				d[y*W+x][(y-1)*W+x] = 1
			}
			if y+1 < H && S[y+1][x] != '#' {
				d[y*W+x][(y+1)*W+x] = 1
			}
			if x-1 >= 0 && S[y][x-1] != '#' {
				d[y*W+x][y*W+x-1] = 1
			}
			if x+1 < W && S[y][x+1] != '#' {
				d[y*W+x][y*W+x+1] = 1
			}
		}
	}

	warshallFloyd(n, d)

	result := 0
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if d[i][j] < math.MaxInt32 && d[i][j] > result {
				result = d[i][j]
			}
		}
	}

	fmt.Println(result)
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

func fillInts(a []int, x int) {
	for i := 0; i < len(a); i++ {
		a[i] = x
	}
}
```

## [ABC151E - Max-Min Sums](https://atcoder.jp/contests/abc151/tasks/abc151_e)

糸口すらつかめず敗退. 70分の間、順位が800番台から1300番台までずり落ちるのを眺める羽目になった.

追記: 2ヶ月もせずに簡単じゃんと解くことになろうとは(笑).

まず、max(X) と min(X) を別々にその合計を求めていいことに注意.
max(X) の最小値はソートした A の A<sub>K</sub>、最大値は A<sub>N</sub> となることは分かる. max(X) が最大値 A<sub>N</sub> になる組み合わせは A<sub>N</sub> を1個確定として、残り N - 1 個から残り K - 1 個を選ぶので <sub>N - 1</sub>C<sub>K - 1</sub>、max(X) が最大値の次に大きい A<sub>N - 1</sub> になる組み合わせは最大値 A<sub>N</sub> は選ばないのを確定として、その次に大きい A<sub>N - 1</sub> を1個確定として残り K - 1 個を選ぶので <sub>N - 2</sub>C<sub>K - 1</sub>、そんな感じで最小値の A<sub>K</sub>まで数え上げればいい. min(X) も同様にやれば解ける.

コンビネーションの計算は、事前に (N-1)! まで求めておけば、*O*(log*N*) で計算できるので、最終的に*O*(*N*log*N*)で計算できる.

Python は % の計算の結果が常に正の整数だが、言語によっては負の整数にもなりうるので、その場合は正の整数に戻す必要があるので注意.

```python
p = 1000000007

N, K = map(int, input().split())
A = list(map(int, input().split()))

fac = [0] * (N + 1)
fac[0] = 1
for i in range(N):
    fac[i + 1] = fac[i] * (i + 1) % p


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], p - 2, p) * pow(fac[k], p - 2, p) % p


A.sort(reverse=True)
result = 0
for i in range(N - K + 1):
    result += A[i] * mcomb(N - (i + 1), K - 1)
    result %= p

A.reverse()
for i in range(N - K + 1):
    result -= A[i] * mcomb(N - (i + 1), K - 1)
    result %= p

print(result)
```
