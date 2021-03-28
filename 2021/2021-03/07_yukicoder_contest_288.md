# yukicoder contest 288 参戦記

## [A 1438 Broken Drawers](https://yukicoder.me/problems/no/1438)

昇順に並んでいない組はどちらかしか開けれないのでその分だけ差し引けばいい.

```python
N, *A = map(int, open(0).read().split())

c = 0
i = 0
while i < N - 1:
    if A[i] > A[i + 1]:
        c += 1
        i += 1
    i += 1
print(N - c)
```

## [B 1439 Let's Compare!!!!](https://yukicoder.me/problems/no/1439)

不一致の最左点だけ比較すれば大小が分かるので、その位置を押さえる. その位置より右で変更があった場合は大小は変わらない. その位置より左で変更があった場合はそこの位置を見るように変える. 現在見ている場所が一地になった場合はそこから次に不一致の点に移動する. これ文字列の一番左と右を行ったり来たりさせられるテストケースで TLE すると思うけど AC できたのでヨシ!

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
S = list(readline()[:-1])
T = list(readline()[:-1])
Q = int(readline())

p = 0
while p < N and S[p] == T[p]:
    p += 1

result = []
for _ in range(Q):
    c, x, y = readline().split()
    x = int(x) - 1
    if  c == 'S':
        S[x] = y
    elif c == 'T':
        T[x] = y
    if x < p and S[x] != T[x]:
        p = x
    elif x == p and S[x] == T[x]:
        while p < N and S[p] == T[p]:
            p += 1

    if p == N:
        result.append('=')
    elif S[p] > T[p]:
        result.append('>')
    elif S[p] < T[p]:
        result.append('<')
print(*result, sep='\n')
```

heapq と set を組み合わせて、TLE しないコードを書いた.

```python
from sys import stdin
from heapq import heapify, heappop, heappush

readline = stdin.readline

N = int(readline())
S = list(readline()[:-1])
T = list(readline()[:-1])
Q = int(readline())

s = set()
q = []
for i in range(N):
    if S[i] == T[i]:
        continue
    q.append(i)
    s.add(i)
heapify(q)

result = []
for _ in range(Q):
    c, x, y = readline().split()
    x = int(x) - 1

    is_same = S[x] == T[x]
    if c == 'S':
        S[x] = y
    elif c == 'T':
        T[x] = y
    if (S[x] == T[x]) != is_same:
        if is_same:
            heappush(q, x)
            s.add(x)
        else:
            s.remove(x)

    while len(q) != 0 and q[0] not in s:
        heappop(q)

    if len(q) == 0:
        result.append('=')
        continue

    p = q[0]
    if S[p] > T[p]:
        result.append('>')
    elif S[p] < T[p]:
        result.append('<')
print(*result, sep='\n')
```

## [D 1441 MErGe](https://yukicoder.me/problems/no/1441)

範囲加算ができる SegmentTree を使って、クエリで提示されたインデックスを最初の数列 A のインデックスに変換できれば、累積和をすることによって範囲合計のクエリは *O*(1) で計算できる. 言うのは簡単ですが、実装は大変でした…….

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

type rangeAddSegmentTree struct {
	size   int
	offset int
	data   []int
}

func newRangeAddSegmentTree(n int) rangeAddSegmentTree {
	var result rangeAddSegmentTree
	result.size = n
	t := 1
	for t < n {
		t *= 2
	}
	result.offset = t - 1
	result.data = make([]int, 2*t-1)
	return result
}

func (st rangeAddSegmentTree) rangeAdd(start, stop, value int) {
	l := start + st.offset
	r := stop + st.offset
	for l < r {
		if l&1 == 0 {
			st.data[l] += value
		}
		if r&1 == 0 {
			st.data[r-1] += value
		}
		l /= 2
		r = (r - 1) / 2
	}
}

func (st rangeAddSegmentTree) get(index int) int {
	i := index + st.offset
	result := st.data[i]
	for i > 0 {
		i = (i - 1) / 2
		result += st.data[i]
	}
	return result
}

func (st rangeAddSegmentTree) set(index, value int) {
	st.rangeAdd(index, index+1, value-st.get(index))
}

func (st rangeAddSegmentTree) bisectLeft(value int) int {
	return sort.Search(st.size, func(i int) bool { return st.get(i) >= value })
}

func main() {
	defer flush()

	N := readInt()
	Q := readInt()

	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
		if i != 0 {
			A[i] += A[i-1]
		}
	}

	st := newRangeAddSegmentTree(N + 1)
	for i := 0; i < N+1; i++ {
		st.rangeAdd(i, i+1, i)
	}

	for i := 0; i < Q; i++ {
		T := readInt()
		l := readInt() - 1
		r := readInt() - 1

		x := st.bisectLeft(l)
		y := st.bisectLeft(r+1) - 1
		if T == 1 {
			for j := l + 1; j < r+1; j++ {
				st.rangeAdd(st.bisectLeft(j), st.bisectLeft(j+1), l-j)
			}
			st.rangeAdd(y+1, N+1, l-r)
		} else if T == 2 {
			if x != 0 {
				println(A[y] - A[x-1])
			} else {
				println(A[y])
			}
		}
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
