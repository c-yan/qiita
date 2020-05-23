# AtCoder Beginner Contest 141 参戦記

## [ABC141A - Weather Prediction](https://atcoder.jp/contests/abc141/tasks/abc141_a)

3分半で突破. ボーッとしてて開始に気づかなく、40秒くらい損している. もったいない. 書くだけ.

```python
S = input()
if S == 'Sunny':
  print('Cloudy')
elif S == 'Cloudy':
  print('Rainy')
else:
  print('Sunny')
```

## [ABC141B - Tap Dance](https://atcoder.jp/contests/abc141/tasks/abc141_b)

3分で突破. 0-indexed と 1-indexed で偶奇がひっくり返ることだけ気にした.

```python
S = input()
if all(c in 'RUD' for c in S[::2]) and all(c in 'LUD' for c in S[1::2]):
  print('Yes')
else:
  print('No')
```

## [ABC141C - Attack Survival](https://atcoder.jp/contests/abc141/tasks/abc141_c)

10分半で突破. 制約を見るに、素直に N - 1 人のスコアを -1 して回ると TLE になるのは明々白々なので、正解者を +1 点する方向性で書いたらあっさり通りました.

```python
N, K, Q = map(int, input().split())
score = [K - Q] * (N + 1)
for _ in range(Q):
  score[int(input())] += 1
for i in range(1, N + 1):
  if score[i] > 0:
    print('Yes')
  else:
    print('No')
```

## [ABC141D - Powerful Discount Tickets](https://atcoder.jp/contests/abc141/tasks/abc141_d)

13分半で突破. [ABC137D - Summer Vacation](https://atcoder.jp/contests/abc137/tasks/abc137_d) からそんなに経ってないのにまた優先度付きキューですかーと思いながら実装. D問題だから素直にM回一番大きいやつを割るのではなく、まとめ割がいるかなあと思ったけどそんなことはなかった.

```python
from heapq import heapify, heappop, heappush
N, M = map(int, input().split())
a = [-int(a) for a in input().split()]
heapify(a)
for _ in range(M):
  heappush(a, heappop(a) / 2)
print(-sum(int(x) for x in a))
```

## [ABC141E - Who Says a Pun?](https://atcoder.jp/contests/abc141/tasks/abc141_e)

TLE だらけで敗退.

解法の1つ目は [Twitter の TL 上に流れてた](https://twitter.com/elephantarium/status/1173240697069027330)シンプルかつ高速な天才的解法. 部分文字列と部分文字列の距離1でスキャンし、距離2でスキャンし、……で、*O*(*N*<sup>2</sup>)でサクッと求まるもの. こんなシンプルな解法でも Python は TLE で絶望した. というわけで、Go 言語版. オリジナルそのままではなく、ループを逆向きで回し、距離が現在の最大長を割り込んだら完了するというやり方でちょっとだけ処理を端折っている.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

func main() {
	N := readInt()
	S := readString()

	result := 0
	for i := N - 1; i > 0; i-- {
		if i <= result {
			break
		}
		t := 0
		for j := 0; j < N-i; j++ {
			if S[j] == S[j+i] {
				t++
				result = max(result, t)
				if result == i {
					break
				}
			} else {
				t = 0
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

もう一つの解法は解説動画で説明されていた DP 版. これも Python は TLE なので Go 言語で. この DP の漸化式は発想できないし、重なっている最大長を求めてそこから解を求めるという発想も自分には出来ないなと思った.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

func main() {
	N := readInt()
	S := readString()

	dp := make([][]int, N+1)
	for i := 0; i < N+1; i++ {
		dp[i] = make([]int, N+1)
	}

	result := 0
	for i := N - 1; i > -1; i-- {
		for j := N - 1; j > -1; j-- {
			if S[i] == S[j] {
				dp[i][j] = dp[i+1][j+1] + 1
				t := min(dp[i][j], j-i)
				result = max(result, t)
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
