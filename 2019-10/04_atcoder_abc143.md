# AtCoder Beginner Contest 143 参戦記

## [ABC143A - Curtain](https://atcoder.jp/contests/abc143/tasks/abc143_a)

2分で突破. マイナスにならないようにするところを忘れないだけ.

```python
A, B = map(int, input().split())

print(max(A - B * 2, 0))
```

## [ABC143B - TAKOYAKI FESTIVAL 2019](https://atcoder.jp/contests/abc143/tasks/abc143_b)

2分半で突破. 書くだけ.

```python
N = int(input())
d = list(map(int, input().split()))

result = 0
for i in range(N - 1):
    for j in range(i + 1, N):
        result += d[i] * d[j]
print(result)
```

## [ABC143C - Slimes](https://atcoder.jp/contests/abc143/tasks/abc143_c)

2分半で突破. 書くだけ. 最近の C 問題が簡単とはいえ、簡単にもほどがある.

```python
N = int(input())
S = input()

p = ''
result = 0
for i in range(N):
    if p != S[i]:
        result += 1
        p = S[i]
print(result)
```

## [ABC143D - Triangles](https://atcoder.jp/contests/abc143/tasks/abc143_d)

82分で突破. TLE3つ. 手元で最大の 2×10<sup>3</sup> のデータを作ってテストして0.6秒だったので安心して出したら TLE を食らうという……. でも結局3重ループで突破.

```Go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func main() {
	N := readInt()
	L := make([]int, N)
	for i := 0; i < N; i++ {
		L[i] = readInt()
	}

	sort.Sort(sort.IntSlice(L))
	result := 0
	for i := 0; i < N-2; i++ {
		for j := i + 1; j < N-1; j++ {
			x := L[i] + L[j]
			for k := j + 1; k < N; k++ {
				if L[k] >= x {
					break
				}
				result++
			}
		}
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

いや、3ループ目を二分探索でやればいいことは途中から分かってたんだけど、その検討途中で条件式が減ったので、なら通るんじゃねって出したら通っちゃったんですよ. Go 言語にビルトインで二分探索がなかったのも悪い(責任転嫁)(※追記: sort.SearchInts があります)

というわけで、二分探索版です.

```python
from bisect import bisect_left

N = int(input())
L = list(map(int, input().split()))

L.sort()
result = 0
for i in range(N - 2):
    for j in range(i + 1, N - 1):
        result += bisect_left(L, L[i] + L[j]) - j - 1
print(result)
```
