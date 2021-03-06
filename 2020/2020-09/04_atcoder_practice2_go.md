# AtCoder Library Practice Contest 参戦記 (Go)

ACL Contest は 1200 から rated なので rated で出場できるかどうしようと考えつつ、AtCoder Library Practice Contest を解いてみるテスト.

## [PRACTICE2A - Disjoint Set Union](https://atcoder.jp/contests/practice2/tasks/practice2_a)

ACL に Union Find 入ってないような、見落としてる??? あ、DSU = Disjoint Set Union = Union Find か.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func find(parent []int, i int) int {
	if parent[i] < 0 {
		return i
	}
	parent[i] = find(parent, parent[i])
	return parent[i]
}

func unite(parent []int, i, j int) {
	i = find(parent, i)
	j = find(parent, j)
	if i == j {
		return
	}
	parent[j] += parent[i]
	parent[i] = j
}

func main() {
	defer flush()

	N := readInt()
	Q := readInt()

	parent := make([]int, N)
	for i := 0; i < N; i++ {
		parent[i] = -1
	}

	for i := 0; i < Q; i++ {
		t := readInt()
		u := readInt()
		v := readInt()
		if t == 0 {
			unite(parent, u, v)
		} else if t == 1 {
			if find(parent, u) == find(parent, v) {
				println(1)
			} else {
				println(0)
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

## [PRACTICE2B - Fenwick Tree](https://atcoder.jp/contests/practice2/tasks/practice2_b)

3度目の登板.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

// BIT stands for binary indexed tree.
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

	a := make([]int, N)
	for i := 0; i < N; i++ {
		a[i] = readInt()
	}

	bit := newBIT(N)
	for i := 0; i < N; i++ {
		bit.add(i, a[i])
	}

	for i := 0; i < Q; i++ {
		t := readInt()
		if t == 0 {
			p := readInt()
			x := readInt()
			bit.add(p, x)
		} else if t == 1 {
			l := readInt()
			r := readInt()
			println(bit.query(l, r))
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

## [PRACTICE2C - Floor Sum](https://atcoder.jp/contests/practice2/tasks/practice2_c)

これの応用が想像できない.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func floorSum(n, m, a, b int) int {
	result := 0
	if a >= m {
		result += (n - 1) * n * (a / m) / 2
		a %= m
	}
	if b >= m {
		result += n * (b / m)
		b %= m
	}

	yMax := (a*n + b) / m
	xMax := yMax*m - b
	if yMax == 0 {
		return result
	}
	result += (n - (xMax+a-1)/a) * yMax
	result += floorSum(yMax, a, m, (a-xMax%a)%a)
	return result
}

func main() {
	defer flush()

	T := readInt()
	for i := 0; i < T; i++ {
		N := readInt()
		M := readInt()
		A := readInt()
		B := readInt()
		println(floorSum(N, M, A, B))
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

## [PRACTICE2I - Number of Substrings](https://atcoder.jp/contests/practice2/tasks/practice2_i)

Go 言語のソートは癖があるなあと改めて思った.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func makeSuffixArray(s string) []int {
	n := len(s)
	sa := make([]int, n)
	for i := 0; i < n; i++ {
		sa[i] = i
	}
	rnk := make([]int, n)
	for i, b := range []byte(s) {
		rnk[i] = int(b)
	}
	tmp := make([]int, n)

	for k := 1; k < n; k *= 2 {
		cmp := func(x, y int) bool {
			if rnk[sa[x]] != rnk[sa[y]] {
				return rnk[sa[x]] < rnk[sa[y]]
			}
			rx, ry := -1, -1
			if sa[x]+k < n {
				rx = rnk[sa[x]+k]
			}
			if sa[y]+k < n {
				ry = rnk[sa[y]+k]
			}
			return rx < ry
		}
		sort.Slice(sa, cmp)
		tmp[sa[0]] = 0
		for i := 1; i < n; i++ {
			tmp[sa[i]] = tmp[sa[i-1]]
			if cmp(i-1, i) {
				tmp[sa[i]]++
			}
		}
		tmp, rnk = rnk, tmp
	}
	return sa
}

func makeLcpArray(s string, sa []int) []int {
	b := []byte(s)
	n := len(s)
	rnk := make([]int, n)
	for i := 0; i < n; i++ {
		rnk[sa[i]] = i
	}
	lcp := make([]int, n-1)
	h := 0
	for i := 0; i < n; i++ {
		if h > 0 {
			h--
		}
		if rnk[i] == 0 {
			continue
		}
		j := sa[rnk[i]-1]
		for j+h < n && i+h < n {
			if b[j+h] != b[i+h] {
				break
			}
			h++
		}
		lcp[rnk[i]-1] = h
	}
	return lcp
}

func main() {
	defer flush()

	S := readString()
	sa := makeSuffixArray(S)

	result := len(S) * (len(S) + 1) / 2
	for _, x := range makeLcpArray(S, sa) {
		result -= x
	}
	println(result)
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

## [PRACTICE2J - Segment Tree](https://atcoder.jp/contests/practice2/tasks/practice2_j)

min_left もメソッドにするべきかな.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

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

func (st segmentTree) build(a []int) {
	for i, v := range a {
		st.data[st.offset+i] = v
	}
	for i := st.offset - 1; i > -1; i-- {
		st.data[i] = st.op(st.data[i*2+1], st.data[i*2+2])
	}
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

func main() {
	defer flush()

	N := readInt()
	Q := readInt()

	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	st := newSegmentTree(N, max, -1)
	st.build(A)

	for i := 0; i < Q; i++ {
		T := readInt()
		if T == 1 || T == 3 {
			X := readInt()
			V := readInt()
			if T == 1 {
				st.update(X-1, V)
			} else if T == 3 {
				ok := N + 1
				ng := X - 1
				for ok-ng > 1 {
					m := ng + (ok-ng)/2
					if st.query(X-1, m) >= V {
						ok = m
					} else {
						ng = m
					}
				}
				println(ok)
			}
		} else if T == 2 {
			L := readInt()
			R := readInt()
			println(st.query(L-1, R))
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
