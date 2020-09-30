# AtCoder Beginner Contest 177 参戦記

レーティングが5回連続下がっていたので、久々に上がって安心した.

## [ABC177A - Don't be late](https://atcoder.jp/contests/abc177/tasks/abc177_a)

2分で突破. 浮動小数点数は誤差が怖いので、T分間で歩ける距離がDメートル以上かでジャッジするようにした.

```python
D, T, S = map(int, input().split())

if D > S * T:
    print('No')
else:
    print('Yes')
```

## [ABC177B - Substring](https://atcoder.jp/contests/abc177/tasks/abc177_b)

5分で突破、WA1. + 1 を忘れて死亡. ずらしながら違う文字が何文字があるかを調べて、最小値を取れば OK. Sの長さが1000なので、計算量は最大でも2.5×10<sup>4</sup>.

```python
S = input()
T = input()

result = float('inf')
for i in range(len(S) - len(T) + 1):
    t = 0
    for j in range(len(T)):
        if S[i + j] != T[j]:
            t += 1
    result = min(result, t)
print(result)
```

## [ABC177C - Sum of product of pairs](https://atcoder.jp/contests/abc177/tasks/abc177_c)

4分で突破. ナイーブに2重ループで計算すると、計算量が2×10<sup>10</sup>で TLE してしまうので累積和した. 右から順番にやっていけば累積和いらないけど、まあいいでしょ.

```python
from itertools import accumulate

m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
b = list(accumulate(A))
for i in range(N):
    result += A[i] * (b[N - 1] - b[i])
    result %= m
print(result)
```

追記: 累積和を使わずに解いてみた.

```python
m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
c = 0
for i in range(1, N + 1):
    result += A[N - i] * c
    result %= m
    c += A[N - i]
    c %= m
print(result)
```

追々記: 右からやる必要もないな.

```python
m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
c = 0
for a in A:
    result += a * c
    result %= m
    c += a
    c %= m
print(result)
```

## [ABC177D - Friends](https://atcoder.jp/contests/abc177/tasks/abc177_d)

4分で突破. 見るからに Union Find. 一番大きいユニオンに属する人数の数だけグループを作ればいい.

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

print(-min(parent))
```

## [ABC177E - Coprime](https://atcoder.jp/contests/abc177/tasks/abc177_e)

23分で突破. 右から素因数分解していけば行けるなと思った. setwise coprime かどうかも一緒に計算できそうな気がしたけど、時間をかけるのはパフォが落ちるので素直に別に計算した.

```python
from math import gcd
from functools import reduce


def make_prime_table(n):
    sieve = list(range(n + 1))
    sieve[0] = -1
    sieve[1] = -1
    for i in range(4, n + 1, 2):
        sieve[i] = 2
    for i in range(3, int(n ** 0.5) + 1, 2):
        if sieve[i] != i:
            continue
        for j in range(i * i, n + 1, i * 2):
            if sieve[j] == j:
                sieve[j] = i
    return sieve


def prime_factorize(n):
    result = []
    while n != 1:
        p = prime_table[n]
        e = 0
        while n % p == 0:
            n //= p
            e += 1
        result.append((p, e))
    return result


def is_pairwise_coprime():
    s = set()
    for i in range(N - 1, -1, -1):
        t = prime_factorize(A[i])
        for p, _ in t:
            if p in s:
                return False
            s.add(p)
    return True


N, *A = map(int, open(0).read().split())

prime_table = make_prime_table(10 ** 6)
if is_pairwise_coprime():
    print('pairwise coprime')
elif reduce(gcd, A) == 1:
    print('setwise coprime')
else:
    print('not coprime')
```

追記: 公式解説動画を見たら素因数分解せずに解いてた. なるほどー.

```python
from math import gcd
from functools import reduce

N, *A = map(int, open(0).read().split())

def is_pairwise_coprime():
    t = [0] * (10 ** 6 + 1)
    for a in A:
        t[a] += 1

    c = 0
    for j in range(2, 10 ** 6 + 1, 2):
        c += t[j]
    if c > 1:
        return False

    for i in range(3, 10 ** 6 + 1, 2):
        c = 0
        for j in range(i, 10 ** 6 + 1, i):
            c += t[j]
        if c > 1:
            return False

    return True

if is_pairwise_coprime():
    print('pairwise coprime')
elif reduce(gcd, A) == 1:
    print('setwise coprime')
else:
    print('not coprime')
```

## [ABC177F - I hate Shortest Path Problem](https://atcoder.jp/contests/abc177/tasks/abc177_f)

1時間以上残っていたので初の全完チャンス!?と思ったが突破できず. 開始のX座標と現在のX座標が管理されていれば、移動回数は現在のX座標-開始のX座標+現在のY座標となる. 現在のX座標-開始のX座標の最小値を高速に求めるために二分探索木を用いる. 候補が壁にぶち当たった場合、壁の右端のX座標-開始のX座標が一番小さいの、つまり開始のX座標が最大のものだけ生き残って、他の候補は全部破棄となる. 壁の範囲の候補を高速に列挙するために二分探索木を用いる.

```go
package main

import (
	"bufio"
	"fmt"
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
	key      int
	value    int
	priority uint
	left     *treapNode
	right    *treapNode
}

func newTreapNode(k int, v int) *treapNode {
	return &treapNode{k, v, xorshift(), nil, nil}
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

func treapFind(n *treapNode, k int) *treapNode {
	if n == nil {
		return nil
	}
	if n.key == k {
		return n
	}
	if n.key > k {
		return treapFind(n.left, k)
	}
	return treapFind(n.right, k)
}

func treapLowerBound(n *treapNode, k int) *treapNode {
	if n == nil {
		return nil
	}
	if n.key < k {
		return treapLowerBound(n.right, k)
	}
	if t := treapLowerBound(n.left, k); t != nil {
		return t
	}
	return n
}

func treapInsert(n *treapNode, k int, v int) *treapNode {
	if n == nil {
		return newTreapNode(k, v)
	}
	if n.key == k {
		panic("node key is duplicated!")
	}
	if n.key > k {
		n.left = treapInsert(n.left, k, v)
		if n.priority > n.left.priority {
			n = treapRotateRight(n)
		}
	} else {
		n.right = treapInsert(n.right, k, v)
		if n.priority > n.right.priority {
			n = treapRotateLeft(n)
		}
	}
	return n
}

func treapDelete(n *treapNode, k int) *treapNode {
	if n == nil {
		panic("node is not found!")
	}
	if n.key > k {
		n.left = treapDelete(n.left, k)
		return n
	}
	if n.key < k {
		n.right = treapDelete(n.right, k)
		return n
	}

	// n.key == k
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
	return treapDelete(n, k)
}

func treapCount(n *treapNode) int {
	if n == nil {
		return 0
	}
	return 1 + treapCount(n.left) + treapCount(n.right)
}

func treapString(n *treapNode) string {
	if n == nil {
		return ""
	}
	result := make([]string, 0)
	if n.left != nil {
		result = append(result, treapString(n.left))
	}
	result = append(result, fmt.Sprintf("%d:%d", n.key, n.value))
	if n.right != nil {
		result = append(result, treapString(n.right))
	}
	return strings.Join(result, " ")
}

func treapMin(n *treapNode) int {
	if n == nil {
		panic("node is not found!")
	}
	if n.left != nil {
		return treapMin(n.left)
	}
	return n.key
}

func treapMax(n *treapNode) int {
	if n == nil {
		panic("node is not found!")
	}
	if n.right != nil {
		return treapMax(n.right)
	}
	return n.key
}

type treap struct {
	root *treapNode
	size int
}

func (t *treap) Find(k int) *treapNode {
	return treapFind(t.root, k)
}

func (t *treap) LowerBound(k int) *treapNode {
	return treapLowerBound(t.root, k)
}

func (t *treap) Insert(k int, v int) {
	t.root = treapInsert(t.root, k, v)
	t.size++
}

func (t *treap) Delete(k int) {
	t.root = treapDelete(t.root, k)
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

	candidates := &treap{}
	moves := &treap{}

	for i := 0; i < W; i++ {
		candidates.Insert(i, i)
	}
	moves.Insert(0, W)

	for i := 0; i < H; i++ {
		A := readInt() - 1
		B := readInt()

		r := -1
		for {
			n := candidates.LowerBound(A)
			if n == nil || n.key > B {
				break
			}
			r = max(r, n.value)
			t := moves.Find(n.key - n.value)
			t.value--
			if t.value == 0 {
				moves.Delete(t.key)
			}
			candidates.Delete(n.key)
		}

		if r != -1 && B != W {
			t := B - r
			n := moves.Find(t)
			if n == nil {
				moves.Insert(t, 1)
			} else {
				n.value++
			}
			candidates.Insert(B, r)
		}

		if moves.Count() == 0 {
			println(-1)
		} else {
			println(moves.Min() + i + 1)
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
