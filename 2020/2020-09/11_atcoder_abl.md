# ACL Beginner Contest 参戦記

## [ABLA - Repeat ACL](https://atcoder.jp/contests/abl/tasks/abl_a)

1分で突破. 書くだけ.

```python
K = int(input())

print('ACL' * K)
```

## [ABLB - Integer Preference](https://atcoder.jp/contests/abl/tasks/abl_b)

2分半で突破、WA1 orz. A < B < C < D のパターンを見落とした.

```python
A, B, C, D = map(int, input().split())

if A <= C <= B or A <= D <= B or C <= A <= D or C <= B <= D:
    print('Yes')
else:
    print('No')
```

## [ABLC - Connect Cities](https://atcoder.jp/contests/abl/tasks/abl_c)

3分半で突破. Union Find して Union 数 - 1 が答え.

```python
from sys import setrecursionlimit, stdin


def find(parent, i):
    t = parent[i]
    if t < 0:
        return i
    t = find(parent, t)
    parent[i] = t
    return t


def unite(parent, i, j):
    i = find(parent, i)
    j = find(parent, j)
    if i == j:
        return
    parent[j] += parent[i]
    parent[i] = j


readline = stdin.readline
setrecursionlimit(10 ** 6)

N, M = map(int, readline().split())

parent = [-1] * N
for _ in range(M):
    A, B = map(lambda x: int(x) - 1, readline().split())
    unite(parent, A, B)

print(sum(1 for i in range(N) if parent[i] < 0) - 1)
```

## [ABLD - Flat Subsequence](https://atcoder.jp/contests/abl/tasks/abl_d)

32分で突破、WA1 orz. Go のコードを Python で提出した、アホすぎる. それまでの各値の最大経由数を記録しておけば SegmentTree で *O*(log<i>N</i>) で現値の最大経由数が求まるので *O*(<i>N</i>log<i>N</i>) になり解ける.

```go
package main

import (
	"bufio"
	"fmt"
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

type segmentTree struct {
	offset int
	data   []int
	op     func(x, y int) int
	e      int
}

func newSegmentTree(n int, op func(x, y int) int, e int) segmentTree {
	var result segmentTree
	t := 1
	for t < n {
		t *= 2
	}
	result.offset = t - 1
	result.data = make([]int, 2*t-1)
	for i := 0; i < len(result.data); i++ {
		result.data[i] = e
	}
	result.op = op
	result.e = e
	return result
}

func (st segmentTree) update(index, value int) {
	i := st.offset + index
	st.data[i] = value
	for i >= 1 {
		i = (i - 1) / 2
		st.data[i] = st.op(st.data[i*2+1], st.data[i*2+2])
	}
}

func (st segmentTree) query(start, stop int) int {
	result := st.e
	l := start + st.offset
	r := stop + st.offset
	for l < r {
		if l&1 == 0 {
			result = st.op(result, st.data[l])
		}
		if r&1 == 0 {
			result = st.op(result, st.data[r-1])
		}
		l = l / 2
		r = (r - 1) / 2
	}
	return result
}

const (
	maxA = 300000
)

func main() {
	defer flush()

	N := readInt()
	K := readInt()

	st := newSegmentTree(maxA+1, max, 0)
	for i := 0; i < N; i++ {
		A := readInt()
		st.update(A, st.query(max(A-K, 0), min(A+K+1, maxA+1))+1)
	}

	println(st.query(0, maxA+1))
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

追記: Python では TLE になってしまうが、PyPy なら AC した.

```python
class SegmentTree:
    def __init__(self, size, op, e):
        self._op = op
        self._e = e
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [e] * (t * 2 - 1)

    def update(self, index, value):
        op = self._op
        data = self._data
        i = self._offset + index
        data[i] = value
        while i >= 1:
            i = (i - 1) // 2
            data[i] = op(data[i * 2 + 1], data[i * 2 + 2])

    def query(self, start, stop):
        def iter_segments(data, l, r):
            while l < r:
                if l & 1 == 0:
                    yield data[l]
                if r & 1 == 0:
                    yield data[r - 1]
                l = l // 2
                r = (r - 1) // 2
        op = self._op
        it = iter_segments(self._data, start + self._offset,
                           stop + self._offset)
        result = self._e
        for v in it:
            result = op(result, v)
        return result


max_A = 300000

N, K, *A = map(int, open(0).read().split())

st = SegmentTree(max_A + 1, max, 0)
for a in A:
    st.update(a, st.query(max(a - K, 0), min(a + K + 1, max_A + 1)) + 1)

print(st.query(0, max_A + 1))
```

## [ABLE - Replace Digits](https://atcoder.jp/contests/abl/tasks/abl_e)

突破できず. Lazy Segtree なんだろうなとは思ったけど、ACL の lazy_segtree の mapping, composition, id が理解できなくて利用できなかった.
