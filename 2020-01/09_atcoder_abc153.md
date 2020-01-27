# AtCoder Beginner Contest 153 参戦記

## ABC153A - Serval vs Monster

1分半で突破. 書くだけ.

```python
H, A = map(int, input().split())

print((H + (A - 1)) // A)
```

## ABC153B - Common Raccoon vs Monster

2分半で突破. 書くだけ. 必殺技の合計ダメージがモンスターの体力を上回っているか、それだけ.

```python
H, N = map(int, input().split())
A = list(map(int, input().split()))

if sum(A) >= H:
    print('Yes')
else:
    print('No')
```

## ABC153C - Fennec vs Monster

2分半で突破. 書くだけ. できるだけ体力が多いやつを必殺技で倒したいので、ソートして先頭K匹以降を攻撃で倒すものとして集計すればいいだけ.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

A.sort(reverse=True)
print(sum(A[K:]))
```

## ABC153D - Caracal vs Monster

7分半で突破. log なので TLE にはならないので、再帰関数で定義通りカウントしていくだけで OK.

```python
from sys import setrecursionlimit

setrecursionlimit(1000000)

H = int(input())


def f(n):
    if n == 1:
        return 1
    else:
        return 1 + f(n // 2) * 2


print(f(H))
```

## ABC153E - Crested Ibis vs Monster

55分半で突破. DP で総当り.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func main() {
	H := readInt()
	N := readInt()
	AB := make([]struct{ A, B int }, N)
	for i := 0; i < N; i++ {
		AB[i].A = readInt()
		AB[i].B = readInt()
	}

	dp := make([]int, 100000)
	for i := 0; i < 100000; i++ {
		dp[i] = math.MaxInt64
	}

	dp[0] = 0
	for i := 0; i < H; i++ {
		if dp[i] == math.MaxInt64 {
			continue
		}
		for j := 0; j < N; j++ {
			a := AB[j].A
			b := AB[j].B
			if dp[i]+b < dp[i+a] {
				dp[i+a] = dp[i] + b
			}
		}
	}
	result := math.MaxInt64
	for i := H; i < 100000; i++ {
		if dp[i] < result {
			result = dp[i]
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

## ABC153F - Silver Fox vs Monster

敗退. AtCoder の Go のバージョンがもっと新しくて、スライスのソートができたら突破していたと思う. そうでなくてももう少し時間が残っていたら C# で書き直したのだが…….
