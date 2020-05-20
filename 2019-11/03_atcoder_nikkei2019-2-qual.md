# AtCoder 第二回全国統一プログラミング王決定戦予選 参戦記

## [A - Sum of Two Integers](https://atcoder.jp/contests/nikkei2019-2-qual/tasks/nikkei2019_2_qual_a)

2分で突破. 書くだけ. ARC の A 問題ってこんなに簡単だっけかと混乱した.

```python
N = int(input())

print((N - 1) // 2)
```

## [B - Counting of Trees](https://atcoder.jp/contests/nikkei2019-2-qual/tasks/nikkei2019_2_qual_b)

68分半で突破. WA 6個. WA2個が中々消えずに WA を重ねてしまった. その2個は0が複数あるやつだった…….

```python
from sys import exit

N = int(input())
D = list(map(int, input().split()))

if D[0] != 0:
    print(0)
    exit()

c = {}
for i in D:
    if i in c:
        c[i] += 1
    else:
        c[i] = 1

if c[0] != 1:
    print(0)
    exit()

result = 1
for i in range(1, max(D) + 1):
    if i not in c:
        print(0)
        exit()
    result = result * (c[i - 1] ** c[i]) % 998244353
print(result)
```

Python は `result = result * (c[i - 1] ** c[i]) % 998244353` とサラッと書けるけど、多倍長整数じゃない言語だと大変だよなと思って、Go で書き直してみたのが以下. やっぱり大変だなー. 冪乗は *O*(log*N*) じゃないといけないかと思って苦労して書いたら、みんな *O*(*N*) で計算していた(笑).

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func pow(base int, exponent int) int {
	result := 1
	for exponent > 0 {
		if exponent%2 == 1 {
			result *= base
			result %= 998244353
		}
		exponent >>= 1
		base *= base
		base %= 998244353
	}
	return result
}

func main() {
	N := readInt()
	D := readInts(N)

	if D[0] != 0 {
		fmt.Println(0)
		return
	}

	c := map[int]int{}
	maxD := 0
	for _, i := range D {
		c[i]++
		if i > maxD {
			maxD = i
		}
	}

	if c[0] != 1 {
		fmt.Println(0)
		return
	}

	result := 1
	for i := 1; i <= maxD; i++ {
		if _, ok := c[i]; !ok {
			fmt.Println(0)
			return
		}
		result *= pow(c[i-1], c[i])
		result %= 998244353
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

func readInts(n int) []int {
	result := make([]int, n)
	for i := 0; i < n; i++ {
		result[i] = readInt()
	}
	return result
}
```

## [C - Swaps](https://atcoder.jp/contests/nikkei2019-2-qual/tasks/nikkei2019_2_qual_c)

突破できず. 実現可能であれば、N-1回操作できれば必ず実現できることは分かったが、N-1回が必要かどうかを求める方法がさっぱり分からなかった. 終了後、解説に巡回置換とかサイクルとか知らない単語を見て、これは無理だったと悟る. 巡回置換でググると大学の数学っぽく、そりゃ無理だ(まて).
