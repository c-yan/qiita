# AtCoder Beginner Contest 160 参戦記

## [ABC160A - Coffee](https://atcoder.jp/contests/abc160/tasks/abc160_a)

1分半で突破. 書くだけ.

```python
S = input()

if S[2] == S[3] and S[4] == S[5]:
    print('Yes')
else:
    print('No')
```

## [ABC160B - Golden Coins](https://atcoder.jp/contests/abc160/tasks/abc160_b)

2分半で突破. 500円のほうがコスパがいいので、500円を可能なだけ、余りを5円で.

```python
X = int(input())

result = (X // 500) * 1000
X -= (X // 500) * 500
result += (X // 5) * 5
print(result)
```

## [ABC160C - Traveling Salesman around Lake](https://atcoder.jp/contests/abc160/tasks/abc160_c)

7分で突破. ジャッジが詰まっててコードテストがなかなか実行されなくて参った. N軒全て回るということは、移動開始地点と移動終了地点の間だけ歩かなくていいということなので、それが最大のところを探せばいい.

```python
K, N = map(int, input().split())
A = list(map(int, input().split()))

result = A[0] - A[N - 1] + K
for i in range(N - 1):
    result = max(result, A[i + 1] - A[i])
print(K - result)
```

## [ABC160D - Line++](https://atcoder.jp/contests/abc160/tasks/abc160_d)

解かずにEに行ってしまったがこっちを解くべきだったろうか.

追記: 明らかにこっちを解くべきだった. すごく簡単だった. やらかした. グラフの苦手意識なんとかしないとなあ. 直接行く場合と、X から Y にワープした場合の小さい方が最短距離なので、集計して表示するだけ.

```python
N, X, Y = map(int, input().split())

t = [0] * (N - 1)
for i in range(1, N):
    for j in range(i + 1, N + 1):
        t[min(j - i, abs(X - i) + 1 + abs(Y - j)) - 1] += 1
print('\n'.join(map(str, t)))
```

## [ABC160E - Red and Green Apples](https://atcoder.jp/contests/abc160/tasks/abc160_e)

敗退. ソートして、累積和して、にぶたんすればいいかと思ったが、WA 5個が消せず. 無念.

追記: Off-by-one エラーだった. `ok := max(0, Y-(C-(X-i))) - 1` の `-1` が抜けていた orz.

降順ソートして累積和をしても、X と Y の2重ループすると 10<sup>10</sup> の計算量になるので TLE は必至. そこで、赤色のリンゴを食べる数はループするけど、緑色のリンゴを食べる数は二分探索で決めることとする. 緑色のリンゴを食べる数をループすると、美味しさの総和は単調増加した後、どこかから単調減少になるはずである. その変化するポイントを二分探索で探す. これなら 1.7 * 10<sup>6</sup> 程度の計算量なので問題なし.

まあ、解説 PDF を見たとおりで、こんなめんどくさいことをしなくても解けるわけだけど、こういうふうにも解けたよという記録として.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	X := readInt()
	Y := readInt()
	A := readInt()
	B := readInt()
	C := readInt()

	p := make([]int, A+1)
	q := make([]int, B+1)
	r := make([]int, C+1)

	for i := 0; i < A; i++ {
		p[i+1] = readInt()
	}
	for i := 0; i < B; i++ {
		q[i+1] = readInt()
	}
	for i := 0; i < C; i++ {
		r[i+1] = readInt()
	}

	sort.Sort(sort.Reverse(sort.IntSlice(p[1:])))
	sort.Sort(sort.Reverse(sort.IntSlice(q[1:])))
	sort.Sort(sort.Reverse(sort.IntSlice(r[1:])))

	for i := 1; i < A; i++ {
		p[i+1] += p[i]
	}
	for i := 1; i < B; i++ {
		q[i+1] += q[i]
	}
	for i := 1; i < C; i++ {
		r[i+1] += r[i]
	}

	result := 0
	for i := max(0, X-C); i <= X; i++ {
		ok := max(0, Y-(C-(X-i))) - 1
		ng := Y
		for ng-ok != 1 {
			m := ok + (ng-ok)/2
			t0 := q[m] + r[(X-i)+(Y-m)]
			t1 := q[m+1] + r[(X-i)+(Y-(m+1))]
			if t0 < t1 {
				ok = m
			} else {
				ng = m
			}
		}
		j := ok + 1
		result = max(result, p[i]+q[j]+r[(X-i)+(Y-j)])
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
