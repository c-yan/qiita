# yukicoder contest 254 参戦記

## [A 1095 Smallest Kadomatsu Subsequence](https://yukicoder.me/problems/no/1095)

ナイーブに書くと *O*(*N*<sup>2</sup>) になってしまう. 最初はセグ木かなあと思ったけど、指定値以上の最小値の検索が出来る気がしなかったので、平衡二分探索木となりました. 中心の門松より左側と右側の平衡二分探索木をメンテしつつ、凸の場合は最小値、凹の場合は真ん中の大きさ以上の最小値を求めて、門松列の大きさの最小値を求めればいいだけ. *O*(<i>N</i>log<i>N</i>).

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
	"strings"
)

var (
	y uint = 88172645463325252
)

func xorshift() uint {
	y ^= y << 7
	y ^= y >> 9
	return y
}

type treapNode struct {
	value    int
	priority uint
	count    int
	left     *treapNode
	right    *treapNode
}

func newTreapNode(v int) *treapNode {
	return &treapNode{v, xorshift(), 1, nil, nil}
}

func treapRotateRight(n *treapNode) *treapNode {
	l := n.left
	n.left = l.right
	l.right = n
	return l
}

func treapRotateLeft(n *treapNode) *treapNode {
	r := n.right
	n.right = r.left
	r.left = n
	return r
}

func treapInsert(n *treapNode, v int) *treapNode {
	if n == nil {
		return newTreapNode(v)
	}
	if n.value == v {
		n.count++
		return n
	}
	if n.value > v {
		n.left = treapInsert(n.left, v)
		if n.priority > n.left.priority {
			n = treapRotateRight(n)
		}
	} else {
		n.right = treapInsert(n.right, v)
		if n.priority > n.right.priority {
			n = treapRotateLeft(n)
		}
	}
	return n
}

func treapDelete(n *treapNode, v int) *treapNode {
	if n == nil {
		panic("node is not found!")
	}
	if n.value > v {
		n.left = treapDelete(n.left, v)
		return n
	}
	if n.value < v {
		n.right = treapDelete(n.right, v)
		return n
	}

	// n.value == v
	if n.count > 1 {
		n.count--
		return n
	}

	if n.left == nil && n.right == nil {
		return nil
	}

	if n.left == nil {
		n = treapRotateLeft(n)
	} else if n.right == nil {
		n = treapRotateRight(n)
	} else {
		// n.left != nil && n.right != nil
		if n.left.priority < n.right.priority {
			n = treapRotateRight(n)
		} else {
			n = treapRotateLeft(n)
		}
	}
	return treapDelete(n, v)
}

func treapCount(n *treapNode) int {
	if n == nil {
		return 0
	}
	return n.count + treapCount(n.left) + treapCount(n.right)
}

func treapString(n *treapNode) string {
	if n == nil {
		return ""
	}
	result := make([]string, 0)
	if n.left != nil {
		result = append(result, treapString(n.left))
	}
	result = append(result, fmt.Sprintf("%d:%d", n.value, n.count))
	if n.right != nil {
		result = append(result, treapString(n.right))
	}
	return strings.Join(result, " ")
}

func treapMin(n *treapNode) int {
	if n.left != nil {
		return treapMin(n.left)
	}
	return n.value
}

func treapGEMin(n *treapNode, v int) int {
	if n.value == v {
		return v
	}
	if n.value > v {
		if n.left != nil {
			return treapGEMin(n.left, v)
		}
		return n.value
	}
	// n.value < v
	if n.right != nil {
		return treapGEMin(n.right, v)
	}
	return math.MaxInt64
}

func treapMax(n *treapNode) int {
	if n.right != nil {
		return treapMax(n.right)
	}
	return n.value
}

type treap struct {
	root *treapNode
	size int
}

func (t *treap) Insert(v int) {
	t.root = treapInsert(t.root, v)
	t.size++
}

func (t *treap) Delete(v int) {
	t.root = treapDelete(t.root, v)
	t.size--
}

func (t *treap) String() string {
	return treapString(t.root)
}

func (t *treap) Count() int {
	return t.size
}

func (t *treap) Min() int {
	return treapMin(t.root)
}

func (t *treap) Max() int {
	return treapMax(t.root)
}

func (t *treap) GEMin(v int) int {
	return treapGEMin(t.root, v)
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func main() {
	defer flush()

	N := readInt()
	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	lt := &treap{}
	rt := &treap{}
	lt.Insert(A[0])
	for i := 2; i < N; i++ {
		rt.Insert(A[i])
	}

	result := math.MaxInt64
	for i := 1; i < N-1; i++ {
		a := lt.Min()
		b := rt.Min()
		if a <= A[i] && b <= A[i] {
			// printf("凸 %d %d %d\n", a, A[i], b)
			result = min(result, a+A[i]+b)
		}

		c := lt.GEMin(A[i])
		d := rt.GEMin(A[i])
		if c != math.MaxInt64 && d != math.MaxInt64 && c >= A[i] && d >= A[i] {
			// printf("凹 %d %d %d\n", c, A[i], d)
			result = min(result, c+A[i]+d)
		}

		lt.Insert(A[i])
		rt.Delete(A[i+1])
	}
	if result == math.MaxInt64 {
		println(-1)
	} else {
		println(result)
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

func readInts(n int) []int {
	result := make([]int, n)
	for i := 0; i < n; i++ {
		result[i] = readInt()
	}
	return result
}

var stdoutWriter = bufio.NewWriter(os.Stdout)

func flush() {
	stdoutWriter.Flush()
}

func printf(f string, args ...interface{}) (int, error) {
	return fmt.Fprintf(stdoutWriter, f, args...)
}

func println(args ...interface{}) (int, error) {
	return fmt.Fprintln(stdoutWriter, args...)
}
```

追記: 以下でも通る. 左右からの累積 Min を使っている. 指定値以上の最小値の検索が出来ていないので A<sub>i</sub> を B としたときの 凹 タイプの門松列が作成できる場合でも、作成できないとしてしまうことがある. 嘘解法じゃないかと思いつつ、反例を考えているが思いつかない. *O*(*N*).

```python
from itertools import accumulate

INF = float('inf')

N, *A = map(int, open(0).read().split())

l = list(accumulate(A, min))
r = list(accumulate(A[::-1], min))[::-1]

result = INF

# 凸 タイプの門松列の場合
for i in range(1, N - 1):
    a = l[i - 1]
    b = A[i]
    c = r[i + 1]
    if a <= b and c <= b:
        result = min(result, a + b + c)

# 凹 タイプの門松列の場合
for i in range(1, N - 1):
    a = l[i - 1]
    b = A[i]
    c = r[i + 1]
    if b <= a and b <= c:
        result = min(result, a + b + c)

if result == INF:
    print(-1)
else:
    print(result)
```

## [B 1096 Range Sums](https://yukicoder.me/problems/no/1096)

ナイーブに書くと *O*(*N*<sup>3</sup>) になってしまう. 累積和しても *O*(*N*<sup>2</sup>). 更にセグ木を投入することにより、*O*(<i>N</i>log<i>N</i>) になって解けた.

```python
from operator import add
from itertools import accumulate


class SegmentTree:
    _f = None
    _size = None
    _offset = None
    _data = None

    def __init__(self, size, f):
        self._f = f
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [0] * (t * 2 - 1)

    def build(self, iterable):
        f = self._f
        data = self._data
        data[self._offset:self._offset + self._size] = iterable
        for i in range(self._offset - 1, -1, -1):
            data[i] = f(data[i * 2 + 1], data[i * 2 + 2])

    def query(self, start, stop):
        def iter_segments(data, l, r):
            while l < r:
                if l & 1 == 0:
                    yield data[l]
                if r & 1 == 0:
                    yield data[r - 1]
                l = l // 2
                r = (r - 1) // 2
        f = self._f
        it = iter_segments(self._data, start + self._offset,
                           stop + self._offset)
        result = next(it)
        for e in it:
            result = f(result, e)
        return result


N, *A = map(int, open(0).read().split())

a = list(accumulate(A))

st = SegmentTree(N, add)
st.build(a)

result = 0
result += st.query(0, N)
for i in range(1, N):
    result += st.query(i, N) - a[i - 1] * (N - i)
print(result)
```

追記: A<sub>i</sub> が答えに足し込まれるのは l = 1, .., i かつ r = i, ..., N のときなので、答えに足し込まれる回数は (i + 1) × (N - i) 回となると考えれば簡単だった. *O*(*N*).

```python
N, *A = map(int, open(0).read().split())

result = 0
for i in range(N):
    # l = 0 .. i, r = i .. N - 1
    result += A[i] * (i + 1) * (N - i)
print(result)
```

追々記: セグ木ではなく Sparse table で解いたと言う人がいたのだが、Disjoint sparse table の間違え? Sparse table は演算に冪等性が必要だが、Min とは違い Sum にはそれはないので. Disjoint sparse table の実装を持ってないので試せなかった.

追々々記: Disjoint sparse table を実装した.

```python
from itertools import accumulate
from operator import add


class DisjointSparseTable:
    _f = None
    _data = None
    _lookup = None

    def __init__(self, a, f):
        self._f = f
        b = 0
        while (1 << b) <= len(a):
            b += 1
        _data = [[0] * len(a) for _ in range(b)]
        _data[0] = a[:]
        for i in range(1, b):
            shift = 1 << i
            for j in range(0, len(a), shift << 1):
                t = min(j + shift, len(a))
                _data[i][t - 1] = a[t - 1]
                for k in range(t - 2, j - 1, -1):
                    _data[i][k] = f(a[k], _data[i][k + 1])
                if t >= len(a):
                    break
                _data[i][t] = a[t]
                r = min(t + shift, len(a))
                for k in range(t + 1, r):
                    _data[i][k] = f(_data[i][k - 1], a[k])
        self._data = _data
        _lookup = [0] * (1 << b)
        for i in range(2, len(_lookup)):
            _lookup[i] = _lookup[i >> 1] + 1
        self._lookup = _lookup

    def query(self, start, stop):
        stop -= 1
        if start >= stop:
            return self._data[0][start]
        p = self._lookup[start ^ stop]
        return self._f(self._data[p][start], self._data[p][stop])


N, *A = map(int, open(0).read().split())

a = list(accumulate(A))

st = DisjointSparseTable(a, add)

result = 0
result += st.query(0, N)
for i in range(1, N):
    result += st.query(i, N) - a[i - 1] * (N - i)
print(result)
```

## [C 1097 Remainder Operation](https://yukicoder.me/problems/no/1097)

終了2分前に解けて嬉しかった. 余りは N 回以内に同じ余りが出てきてループする. ループ検出して、1周期の長さと1ループ中の増分を求めればクエリに *O*(1) で回答できるようになるので *O*(*N* + *Q*) で解けた. [ABC167D - Teleporter](https://atcoder.jp/contests/abc167/tasks/abc167_d) を思い出した.

```python
from sys import stdin
readline = stdin.readline

N = int(readline())
A = list(map(int, readline().split()))

X = 0
b = [-1]
c = [X]
used = set()
while True:
    r = X % N
    X += A[r]
    if r in used:
        break
    used.add(r)
    b.append(r)
    c.append(X)
b.append(r)
c.append(X)
loop_start = b.index(b[-1])
loop_len = len(b) - loop_start - 1

Q = int(readline())
for _ in range(Q):
    K = int(readline())
    if K < len(b):
        print(c[K])
    else:
        K -= loop_start
        a = K // loop_len
        K %= loop_len
        K += loop_start
        print(c[K] + a * (c[-1] - c[loop_start]))
```
