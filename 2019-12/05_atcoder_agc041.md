# AtCoder Grand Contest 041 参戦記

## A - Table Tennis Training

28分半で突破. WA5. 何だ簡単じゃんと思って提出したらやっぱり簡単ではなかったw

以下のうち一番早いものが解答

1. まっすぐ近寄る(ただし距離が偶数でないとすれ違うので偶数の場合のみ)
2. 1 で待ち合わせをする.
3. N で待ち合わせをする.
4. 2人で1に向かい、先に着くAにいた選手が1ターン1に留まり(これで距離が偶数になる)、折り返してBを迎えに行く
5. 2人でNに向かい、先に着くBにいた選手が1ターンNに留まり(これで距離が偶数になる)、折り返してAを迎えに行く

```python
N, A, B = map(int, input().split())

t = float('inf')
if abs(A - B) % 2 == 0:
    t = min(abs(A - B) // 2, t)
t = min(B - 1, t)
t = min(N - A, t)
t = min((A - 1 + 1) + (B - A - 1) // 2, t)
t = min((N - B + 1) + (N - (A + (N - B + 1))) // 2, t)
print(t)
```

## B - Voting Judges

113分で突破. WA5. Python で TLE 食らって、PyPy にしても TLE 食らって、Go で書き直しても TLE 食らって、一箇所無駄なスライスのコピーを消したら AC と、一つだけ重たいテストケースが入っててボコボコにされた.

とはいえ、1900番台でレーティングフルボッコになりそうで涙目だった状態から、華麗な900番台への転生で歓喜の舞.

一つの問題が受け取れる投票はM個がマックスなので、現在スコア+MがP番目のスコア以上になれば基本的には候補になる.
が、投票数が多すぎる場合はP番目のスコアにも投票をまぶさざるえなく、その処理がとても大変だった.
というか、適当に書いたのによく通ったなあというのが正直なところ.
厳密にはどっかに穴がありそう…….

```Go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func main() {
	N := readInt()
	M := readInt()
	V := readInt()
	P := readInt()

	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}
	sort.Sort(sort.Reverse(sort.IntSlice(A)))
	p := A[P-1]
	V -= P - 1
	b := A[P-1:]
	for {
		mv := M * V
		for i := 0; i < len(b); i++ {
			t := min(p-b[i], M)
			mv -= t
		}
		if mv > 0 {
			p += (mv + len(b) - 1) / len(b)
		} else {
			break
		}
	}

	result := 0
	for i := 0; i < N; i++ {
		if A[i]+M >= p {
			result++
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
