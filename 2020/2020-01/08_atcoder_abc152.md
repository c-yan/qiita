# AtCoder Beginner Contest 152 参戦記

## [ABC152A - AC or WA](https://atcoder.jp/contests/abc152/tasks/abc152_a)

1分半で突破. 書くだけ.

```python
N, M = map(int, input().split())

if N == M:
    print('Yes')
else:
    print('No')
```

## [ABC152B - Comparing Strings](https://atcoder.jp/contests/abc152/tasks/abc152_b)

2分で突破. 書くだけ.

```python
a, b = map(int, input().split())

if a < b:
    print(str(a) * b)
else:
    print(str(b) * a)
```

## [ABC152C - Low Elements](https://atcoder.jp/contests/abc152/tasks/abc152_c)

5分で突破. 流石に二重ループは TLE なので、現時点の最小値を持ち回す必要あり. 題意を理解するのに少し時間を使った.

```python
N = int(input())
P = list(map(int, input().split()))

result = 0
m = P[0]
for i in range(N):
    if P[i] <= m:
        result += 1
        m = P[i]
print(result)
```

## [ABC152D - Handstand 2](https://atcoder.jp/contests/abc152/tasks/abc152_d)

26分で突破. N回ループ回しても大丈夫だと見切れればさして難しくはない. 先頭と末尾の組み合わせで個数を集計すれば一発.

```python
N = int(input())

t = [[0] * 10 for _ in range(10)]
for i in range(1, N + 1):
    s = str(i)
    t[int(s[0])][int(s[-1])] += 1

result = 0
for i in range(1, 10):
    for j in range(1, 10):
        result += t[i][j] * t[j][i]
print(result)
```

## [ABC152E - Flatten](https://atcoder.jp/contests/abc152/tasks/abc152_e)

敗退. 最小公倍数を求めて集計するナイーブな実装では TLE だった.

追記: 解説通り実装した.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func mpow(x int, n int) int {
	result := 1
	for n != 0 {
		if n&1 == 1 {
			result *= x
			result %= 1000000007
		}
		x *= x
		x %= 1000000007
		n >>= 1
	}
	return result
}

func main() {
	maxA := 1000000

	N := readInt()
	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	sieve := make([]int, maxA+1)
	sieve[0] = -1
	sieve[1] = -1
	for i := 2; i <= maxA; i++ {
		if sieve[i] != 0 {
			continue
		}
		sieve[i] = i
		for j := i * i; j <= maxA; j += i {
			if sieve[j] == 0 {
				sieve[j] = i
			}
		}
	}

	lcmFactors := map[int]int{}
	for i := 0; i < N; i++ {
		t := map[int]int{}
		a := A[i]
		for a != 1 {
			t[sieve[a]]++
			a /= sieve[a]
		}
		for k, v := range t {
			if lcmFactors[k] < v {
				lcmFactors[k] = v
			}
		}
	}

	lcm := 1
	for k, v := range lcmFactors {
		for i := 0; i < v; i++ {
			lcm *= k
			lcm %= 1000000007
		}
	}

	result := 0
	for i := 0; i < N; i++ {
		result += lcm * mpow(A[i], 1000000007-2)
		result %= 1000000007
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
```
