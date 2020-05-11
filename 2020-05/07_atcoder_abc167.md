# AtCoder Beginner Contest 167 参戦記

700番台とか初めてなのでイヤッッホォォォオオォオウ!!

## [ABC167A - Registration](https://atcoder.jp/contests/abc167/tasks/abc167_a)

2分で突破. 書くだけ.

```python
S = input()
T = input()

if S == T[:-1]:
    print('Yes')
else:
    print('No')
```

## [ABC167B - Easy Linear Programming](https://atcoder.jp/contests/abc167/tasks/abc167_b)

3分半で突破. `sum(([0] * A + [1] * B + [-1] * C)[:K])` って書いている途中で K≤2×10<sup>9</sup> に気づいた. なんという時間の無駄.

```python
A, B, C, K = map(int, input().split())

result = 0
t = min(A, K)
result += t
K -= t
t = min(B, K)
K -= t
t = min(C, K)
result -= t
print(result)
```

## [ABC167C - Skill Up](https://atcoder.jp/contests/abc167/tasks/abc167_c)

8分半で突破. C問題でビット全探索!?ってなった. やっぱりC問題にしては難しいせいか、半分くらい虐殺されていますね(順位表を見て).

```python
N, M, X = map(int, input().split())

C = []
A = []
for i in range(N):
    t = list(map(int, input().split()))
    C.append(t[0])
    A.append(t[1:])

result = -1
for i in range(1 << N):
    t = [0] * M
    c = 0
    for j in range(N):
        if (i >> j) & 1 == 0:
            continue
        c += C[j]
        for k in range(M):
            t[k] += A[j][k]
    if all(x >= X for x in t):
        if result == -1:
            result = c
        else:
            result = min(result, c)
print(result)
```

## [ABC167D - Teleporter](https://atcoder.jp/contests/abc167/tasks/abc167_d)

13分で突破. K≤10<sup>18</sup> なので K 回 for を回すことはできないけど、N≤2×10<sup>5</sup> なので N 回 for を回せる. 町の数は N 個なので、N 回以内に必ずループするので、1ループの長さがわかる. それさえ分かれば K をその長さで割った余りだけ回せば答えになる.

```python
N, K = map(int, input().split())
A = [int(a) - 1 for a in input().split()]

if K <= N:
    p = 0
    for i in range(K):
        p = A[p]
    print(p + 1)
    exit()

p = 0
t = [-1] * N
t[0] = 0
for i in range(1, N):
    p = A[p]
    if t[p] != -1:
        break
    t[p] = i

d = i - t[p]
K -= i
K %= d

for i in range(K):
    p = A[p]
print(p + 1)
```

## [ABC167E - Colorful Blocks](https://atcoder.jp/contests/abc167/tasks/abc167_e)

25分で突破. Python で TLE1. 一つも同じ色が隣り合わない塗り方は M×(M - 1)<sup>N - 1</sup> 通り. 一箇所だけ隣り合う塗り方は M×1×(M - 1)<sup>N - 2</sup>×<sub>N-1</sub>C<sub>1</sub> と考えていくと n 箇所色が隣り合う塗り方は M×(M - 1)<sup>N - 1 - n</sup>×<sub>N-1</sub>C<sub>n</sub> となる. 後はループして積算するだけ. スパッと解けたのは yukicoder の [No.1035 Color Box](https://yukicoder.me/problems/no/1035) で5時間唸ったおかげだと思う.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

const (
	p = 998244353
)

var (
	fac []int
)

func mpow(x int, n int) int {
	result := 1
	for n != 0 {
		if n&1 == 1 {
			result = result * x % p
		}
		x = x * x % p
		n >>= 1
	}
	return result
}

func mcomb(n int, k int) int {
	if n == 0 && k == 0 {
		return 1
	}
	if n < k || k < 0 {
		return 0
	}
	return (fac[n] * mpow(fac[n-k], p-2) % p) * mpow(fac[k], p-2) % p
}

func main() {
	defer flush()

	N := readInt()
	M := readInt()
	K := readInt()

	fac = make([]int, N+1)
	fac[0] = 1
	for i := 0; i < N; i++ {
		fac[i+1] = fac[i] * (i + 1) % p
	}

	result := 0
	for i := 0; i < K+1; i++ {
		t := M * mcomb(N-1, i)
		t %= p
		t *= mpow(M-1, N-1-i)
		t %= p
		result += t
		result %= p
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

func printf(f string, args ...interface{}) (int, error) {
	return fmt.Fprintf(stdoutWriter, f, args...)
}

func println(args ...interface{}) (int, error) {
	return fmt.Fprintln(stdoutWriter, args...)
}
```

追記: Python でも通しました.

```python
N, M, K = map(int, input().split())

result = 0
n = 1
k = 1
for i in range(K + 1):
    result += n * pow(k, 998244353 - 2, 998244353) * pow(M - 1, N - 1 - i, 998244353)
    result %= 998244353
    n *= N - 1 - i
    n %= 998244353
    k *= i + 1
    k %= 998244353
result *= M
result %= 998244353
print(result)
```

## [ABC167F - Bracket Sequencing](https://atcoder.jp/contests/abc167/tasks/abc167_f)

突破できず.

追記: 概ね解説動画の通り解いてみましたが、正と負でリストを分けるのが面倒だったので、ソートのキー関数で処理してしまいました.

```python
def scan(s):
    m = 0
    a = 0
    for c in s:
        if c == '(':
            a += 1
        elif c == ')':
            a -= 1
        m = min(m, a)
    return m, a


def custom_key(v):
    m, a = v
    if a >= 0:
        return 1, m, a
    else:
        return -1, a - m, a


N = int(input())
S = [input() for _ in range(N)]

c = 0
for m, a in sorted([scan(s) for s in S], reverse=True, key=custom_key):
    if c + m < 0:
        c += m
        break
    c += a

if c == 0:
    print('Yes')
else:
    print('No')
```
