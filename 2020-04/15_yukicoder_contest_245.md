# yukicoder contest 245 参戦記

## [A 1033 乱数サイ](https://yukicoder.me/problems/no/1033)

問題を見た瞬間に、K ってなんのためにあるんだろうと思ったが、やっぱり要らなかった(笑). 0～Nの平均値は0～Nの合計値をN+1で割ったものなので (0 + N) * (N + 1) ÷ 2 ÷ (N + 1) なので、N ÷ 2 となる.

```python
N, K = map(int, input().split())

print(N / 2)
```

## [D 1036 Make One With GCD 2](https://yukicoder.me/problems/no/1036)

セグ木の GCD 版を持っていたので瞬殺だった(笑). 基本は尺取法. GCD が1になったらそこから先はどれだけ進めても1なので、GCD(A<sub>i</sub>, A<sub>i+1</sub>, ..., A<sub>j</sub>) で初めて1になったのであれば、A<sub>i</sub>を左端とする部分列で GCD が1になる部分列の数は N - j + 1 となる. 次は A<sub>i+1</sub> を左端とする部分列を考えるのだが、当然右端は j 以上の数となる. ここで GCD(A<sub>i+1</sub>, ..., A<sub>j</sub>) を高速に計算するためにセグ木を使った. なお、j が N になっても GCD が1にならなかったら、それより右側の部分列では GCD は1にならないので、そこで計算を打ち切って良い.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func gcd(a, b int) int {
	if a < b {
		a, b = b, a
	}
	for b > 0 {
		a, b = b, a%b
	}
	return a
}

type segmentTree struct {
	data   []int
	offset int
}

func newSegmentTree(n int) segmentTree {
	var result segmentTree
	t := 1
	for t < n {
		t *= 2
	}
	result.offset = t - 1
	result.data = make([]int, 2*t-1)
	return result
}

func (st segmentTree) updateAll(a []int) {
	for i, v := range a {
		st.data[st.offset+i] = v
	}
	for i := st.offset - 1; i > -1; i-- {
		st.data[i] = gcd(st.data[i*2+1], st.data[i*2+2])
	}
}

func (st segmentTree) update(index, value int) {
	i := st.offset + index
	st.data[i] = value
	for i >= 1 {
		i = (i - 1) / 2
		st.data[i] = gcd(st.data[i*2+1], st.data[i*2+2])
	}
}

func (st segmentTree) query(start, stop int) int {
	result := 0
	l := start + st.offset
	r := stop + st.offset
	for l < r {
		if l&1 == 0 {
			result = gcd(result, st.data[l])
		}
		if r&1 == 0 {
			result = gcd(result, st.data[r-1])
		}
		l = l / 2
		r = (r - 1) / 2
	}
	return result
}

func main() {
	defer flush()

	N := readInt()
	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	st := newSegmentTree(N)
	st.updateAll(A)

	result := 0
	j := 1
	for i := 0; i < N; i++ {
		v := st.query(i, j)
		for v != 1 && j < N {
			v = gcd(v, A[j])
			j++
		}
		if v != 1 {
			break
		}
		result += N - j + 1
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

## [B 1034 テスターのふっぴーさん](https://yukicoder.me/problems/no/1034)

時計回りに渦を巻きながら進んでいくわけだが、(I, J) が何巻目かをまず考える. I, J, N - 1 - I, N - 1 - J の最小値だと分かる. この最小値を l すると、l 巻目に入るまでに移動した回数は、全移動回数 N * N から、l 巻以降の移動回数 (N - 2 * l) * (N - 2 * l) を引いたものだと分かる. 後は一巻きだけ進めてかかった移動を確認し、l巻き目に入るまで移動した回数と合算すれば回答できる.

```python
Q = int(input())

for i in range(Q):
    N, I, J = map(int, input().split())
    l = min(I, J, N - 1 - I, N - 1 - J)
    result = N * N - (N - 2 * l) * (N - 2 * l)
    N, I, J = N - 2 * l, I - l, J - l
    if I == 0:
        result += J
    elif J == N - 1:
        result += N - 1 + I
    elif I == N - 1:
        result += 2 * (N - 1) + (N - 1) - J
    elif J == 0:
        result += 3 * (N - 1) + (N - 1) - I
    print(result)
```

## [E 1037 exhausted ](https://yukicoder.me/problems/no/1037)

`println(result)` を `print(result)` と書いてしまって、コンテスト中に回答できなかった orz. 全く同じアルゴリズムで Python で書いたら、WA は出なかったものの TLE したので Go 言語で書き直している.

ガソリンスタンドに着くたびに、燃料を入れるパターンと入れないパターンの DP をするだけなので、特に難しいことはないんじゃなかろうか. 同じ燃料の量でそこまでの費用が高い場合はそのパターンを捨てる.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	defer flush()

	N := readInt()
	V := readInt()
	L := readInt()

	cx := 0
	d := map[int]int{}
	d[V] = 0
	for i := 0; i < N; i++ {
		x := readInt()
		v := readInt()
		w := readInt()
		nd := map[int]int{}

		for k := range d {
			t := k - (x - cx)
			if t < 0 {
				continue
			}
			_, ok := nd[t]
			if !ok || nd[t] > d[k] {
				nd[t] = d[k]
			}
			t = min(t+v, V)
			_, ok = nd[t]
			if !ok || nd[t] > d[k]+w {
				nd[t] = d[k] + w
			}
		}

		if len(nd) == 0 {
			println(-1)
			return
		}
		d = nd
		cx = x
	}

	result := math.MaxInt64
	for k := range d {
		if k-(L-cx) >= 0 {
			result = min(result, d[k])
		}
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

## [C 1035 Color Box](https://yukicoder.me/problems/no/1035)

i種類のペンキを使って色を塗る塗り方の総数を f(i) とすると、f(i) = <sub>M</sub>C<sub>i</sub> * i<sup>N</sup> - f(i - 1) となる. また、f(0) = 0 である.

と簡潔に書いていますが、これにたどり着くのに5時間以上かかりました orz. ★2個とか嘘やろ.

```python
from sys import setrecursionlimit

setrecursionlimit(1000000)

N, M = map(int, input().split())

fac = [0] * (M + 1)
fac[0] = 1
for i in range(M):
    fac[i + 1] = fac[i] * (i + 1) % 1000000007


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], 1000000005, 1000000007) * pow(fac[k], 1000000005, 1000000007) % 1000000007


def f(i):
    if i == 0:
        return 0
    return (mcomb(M, i) * pow(i, N, 1000000007) - f(i - 1)) % 1000000007


print(f(M))
```
