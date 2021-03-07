# AtCoder Beginner Contest 194 参戦記

## [ABC194A - I Scream](https://atcoder.jp/contests/abc194/tasks/abc194_a)

4分で突破. 国語力を要求され、珍しく難しいA問題だった. そして、typo で WA してしまった orz.

```python
A, B = map(int, input().split())

if A + B >= 15 and B >= 8:
    print(1)
elif A + B >= 10 and B >= 3:
    print(2)
elif A + B >= 3:
    print(3)
else:
    print(4)
```

## [ABC194B - Job Assignment](https://atcoder.jp/contests/abc194/tasks/abc194_b)

7分で突破. B問題にしては意外と実装がめんどくさかった.

```python
N = int(input())

a = []
b = []
for _ in range(N):
    A, B = map(int, input().split())
    a.append(A)
    b.append(B)

result = 10 ** 18
for i in range(N):
    for j in range(N):
        if i == j:
            result = min(result, a[i] + b[i])
        else:
            result = min(result, max(a[i], b[j]))
print(result)
```

## [ABC194C - Squared Error](https://atcoder.jp/contests/abc194/tasks/abc194_c)

14分で突破、TLE1. *O*(*N*<sup>2</sup>) の解法しか思いつかず、何この難しいC問題と思ったが、|A<sub>i</sub>|≤200 をみて、「あ」っとなった.

```python
from collections import Counter

N, *A = map(int, open(0).read().split())

c = Counter(A)
result = 0
for a in c:
    for b in c:
        result += (a - b) * (a - b) * c[a] * c[b]
print(result // 2)
```

## [ABC194 	D - Journey](https://atcoder.jp/contests/abc194/tasks/abc194_d)

7分で突破. コンプリートガチャが揃うのにかかる試行回数の期待値をググって解いた.

```python
N = int(input())

t = 0
for i in range(1, N + 1):
    t += 1 / i
print(N * t - 1)
```

## [ABC194E - Mex Min](https://atcoder.jp/contests/abc194/tasks/abc194_e)

26分半で突破、RE1. mex でググったらすぐ解き方がわかったが、オフバイワンエラーをやらかした.

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

const (
	X = 2000000
)

func main() {
	defer flush()

	N := readInt()
	M := readInt()

	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	c := make([]int, X)
	st := newSegmentTree(X, min, math.MaxInt64)
	for i := 0; i <= X; i++ {
		st.update(i, i)
	}

	for i := 0; i < M; i++ {
		a := A[i]
		if c[a] == 0 {
			st.update(a, math.MaxInt64)
		}
		c[a]++
	}
	result := st.query(0, X)
	for i := 0; i < N-M; i++ {
		a := A[i]
		if c[a] == 1 {
			st.update(a, a)
		}
		c[a]--
		b := A[i+M]
		if c[b] == 0 {
			st.update(b, math.MaxInt64)
		}
		c[b]++
		result = min(result, st.query(0, X))
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
