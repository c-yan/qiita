# AtCoder Beginner Contest 174 参戦記

9ペナの大爆死. 終了1分半前にギリギリE問題を通したおかげで、レーティングが小ダウンで済んだ.

## [ABC174A - Air Conditioner](https://atcoder.jp/contests/abc174/tasks/abc174_a)

1分で突破. 書くだけ.

```python
X = int(input())

if X >= 30:
    print('Yes')
else:
    print('No')
```

## [ABC174B - Distance](https://atcoder.jp/contests/abc174/tasks/abc174_b)

2分半で突破. 書くだけ.

```python
N, D = map(int, input().split())

result = 0
for _ in range(N):
    X, Y = map(int, input().split())
    if X * X + Y * Y <= D * D:
        result += 1
print(result)
```

## [ABC174D - Alter Altar](https://atcoder.jp/contests/abc174/tasks/abc174_d)

20分くらい?で突破. RE×1, WA×4. 一番左にある白い石を一番右にある赤い石とスワップするを繰り返して、白い石の右側に赤い石がない状態にすれば良い. 言うのは簡単なのに、実装でハマりました…….

```python
N = int(input())
c = input()

d = list(c)

i = 0
j = N - 1
result = 0
while True:
    while i < N and d[i] != 'W':
        i += 1
    while j > 0 and d[j] != 'R':
        j -= 1
    if i == N or j == -1 or i >= j:
        break
    d[i] = 'R'
    d[j] = 'W'
    result += 1
print(result)
```

## [ABC174C - Repsept](https://atcoder.jp/contests/abc174/tasks/abc174_c)

28分くらい?で突破. C問題だからナイーブに書いても行けるかと思ったら、入力例3で TLE 食らって真っ青. 多分最大値は K - 1 だろうとヤマを張って出したら通った…….

```python
K = int(input())

t = 7
for i in range(K):
    if t % K == 0:
        print(i + 1)
        break
    t = (t * 10 + 7) % K
else:
    print(-1)
```

追記: K で割った余りの0以外は K - 1 種類なので、最大値は K - 1 に決まっていた. 途中で同じ値がでてくるとループなので0になることはない.

## [ABC174E - Logs](https://atcoder.jp/contests/abc174/tasks/abc174_e)

47分で突破. TLE×1、RE×3. heapq で正答が出るコードを書いて提出してから K≤10<sup>9</sup> に気づくチョンボ. 24分間 heapq のまま高速化の努力をした後に、これは駄目だとなった瞬間ににぶたんを閃いて、13分でコードを書いたらもう終了1分半前. 通った、助かった. しかし、 `abs(ng - ok) > 1` でよく通ったな.

```python
from math import ceil

N, K, *A = map(int, open(0).read().split())

def is_ok(n):
    t = 0
    for a in A:
        if a <= n:
            continue
        t += ceil(a / n) - 1
    return t <= K

ok = 1000000000
ng = 0.0000000001
while abs(ng - ok) > 1:
    m = (ok + ng) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m

print(ceil(ok))
```

## [ABC174F - Range Set Query](https://atcoder.jp/contests/abc174/tasks/abc174_f)

各色についてrより左側にある一番右側の玉の位置がl番目以上ならカウントできるので、SegmentTree なり BIT なりで管理しながら進めれば良い. っていうのは簡単だけどぱっとは思いつかないよなあ.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

type BIT []int

func newBIT(n int) BIT {
	return make([]int, n)
}

func (bit BIT) add(i, v int) {
	for i++; i <= len(bit); i += i & -i {
		bit[i-1] += v
	}
}

func (bit BIT) sum(i int) int {
	result := 0
	for i++; i > 0; i -= i & -i {
		result += bit[i-1]
	}
	return result
}

func (bit BIT) query(start int, stop int) int {
	return bit.sum(stop-1) - bit.sum(start-1)
}

func main() {
	defer flush()

	N := readInt()
	Q := readInt()

	c := make([]int, N)
	for i := 0; i < N; i++ {
		c[i] = readInt()
	}

	lr := make([][3]int, Q)
	for i := 0; i < Q; i++ {
		l := readInt() - 1
		r := readInt() - 1
		lr[i] = [3]int{i, l, r}
	}
	sort.Slice(lr, func(i, j int) bool { return lr[i][2] < lr[j][2] })

	result := make([]int, Q)
	bit := newBIT(N)
	mr := map[int]int{}
	k := 0
	for i := 0; i < N; i++ {
		if j, ok := mr[c[i]]; ok {
			bit.add(j, -1)
		}
		bit.add(i, 1)
		mr[c[i]] = i
		for k < Q && lr[k][2] == i {
			result[lr[k][0]] = bit.query(lr[k][1], lr[k][2]+1)
			k++
		}
	}

	for i := 0; i < Q; i++ {
		println(result[i])
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
