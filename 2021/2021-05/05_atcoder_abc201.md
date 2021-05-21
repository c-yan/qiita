# マイナビプログラミングコンテスト2021(AtCoder Beginner Contest 201) 参戦記

2週連続でコケてしまった…….

## [ABC201A - Tiny Arithmetic Sequence](https://atcoder.jp/contests/abc201/tasks/abc201_a)

2分で突破. 書くだけ.

```python
A = list(map(int, input().split()))

A.sort()

if A[2] - A[1] == A[1] - A[0]:
    print('Yes')
else:
    print('No')
```

## [ABC201B - Do you know the second highest mountain?](https://atcoder.jp/contests/abc201/tasks/abc201_b)

4分で突破. 高さがそれぞれ相異なることが保証されているので、何も考えずソートすればいい.

```python
N = int(input())
ST = []
for _ in range(N):
    S, T = input().split()
    ST.append((int(T), S))

ST.sort(reverse=True)
print(ST[1][1])
```

## [ABC201C - Secret Number](https://atcoder.jp/contests/abc201/tasks/abc201_c)

14分で突破. 最大でも10<sup>4</sup>通りしかないので、全部生成してしまうことにした. Python の itertools がパワフルなので、全生成してよければ簡単.

```python
from itertools import product, permutations

S = input()

if S.count('o') > 4 or S.count('x') == 10:
    print(0)
    exit()

base = []
option = []
for i in range(10):
    if S[i] == 'o':
        base.append(str(i))
    if S[i] != 'x':
        option.append(str(i))

result = set()
for p1 in product(option, repeat=4 - len(base)):
    for p2 in permutations(base + list(p1)):
        result.add(''.join(list(p2)))
print(len(result))
```

## [ABC201D - Game in Momotetsu World](https://atcoder.jp/contests/abc201/tasks/abc201_d)

73分半で突破. RE3、TLE6. メモ化再帰で素直にシミュレートすればいいかと思いきや、Python / PyPy では TLE となった. Go で書き直しても 457ms だったので、そりゃダメだなあと思った.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

var (
	cache []int
	H, W  int
	A     [][]int
)

const (
	INF = math.MaxInt64
)

func f(y, x int) int {
	key := (y << 12) + x
	if y == H-1 && x == W-1 {
		return 0
	}
	if cache[key] != INF {
		return cache[key]
	}
	result := -INF
	if y < H-1 {
		result = max(result, A[y+1][x]-f(y+1, x))
	}
	if x < W-1 {
		result = max(result, A[y][x+1]-f(y, x+1))
	}
	cache[key] = result
	return result
}

func main() {
	defer flush()

	H = readInt()
	W = readInt()
	A = make([][]int, H)
	for i := 0; i < H; i++ {
		A[i] = make([]int, W)
		s := readString()
		for j := 0; j < W; j++ {
			if s[j] == '-' {
				A[i][j] = -1
			} else if s[j] == '+' {
				A[i][j] = 1
			}
		}
	}

	cache = make([]int, 1<<24)
	for i := 0; i < len(cache); i++ {
		cache[i] = INF
	}

	result := f(0, 0)
	if result == 0 {
		println("Draw")
	} else if result > 0 {
		println("Takahashi")
	} else if result < 0 {
		println("Aoki")
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
