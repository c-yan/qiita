# AtCoder Beginner Contest 147 参戦記

## A - Blackjack

2分半で突破. 書くだけ.

```python
A1, A2, A3 = map(int, input().split())

if sum([A1, A2, A3]) >= 22:
  print('bust')
else:
  print('win')
```

## B - Palindrome-philia

2分で突破. 書くだけ.

```python
S = input()

result = 0
for i in range(len(S) // 2):
  if S[i] != S[-(i + 1)]:
    result += 1
print(result)
```

## D - Xor Sum 4

46分くらい?で突破. Cを提出したら WA3 で諦めてこっちに取り掛かったのでよく分からず. ビットごとに累積 XOR をするだけ. だけって言ってもコーディングは中々大変. Python で書いたら TLE になったので、Go で書き直した. `% 1000000007` がある問題はできるだけ Python で書きたいのにね.

A<sub>i</sub> のあるビットが0の場合、A<sub>i+1</sub>～A<sub>N</sub>のあるビットが1の個数をかける、A<sub>i</sub> のあるビットが1の場合、A<sub>i+1</sub>～A<sub>N</sub>のあるビットが0の個数をかけることによって、個数の累積和が取ってあれば内側のシグマはループせずに計算できる. 制約より0≤A<sub>i</sub>&lt;2<sup>60</sup>なので、ビットごとに60回処理すれば良い.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	N := readInt()
	A := make([]int, N)
	for i := 0; i < N; i++ {
		A[i] = readInt()
	}

	result := 0
	for b := 0; b < 60; b++ {
		a := make([]int, N)
		for i := 0; i < N; i++ {
			a[i] = (A[i] >> uint(b)) & 1
		}

		bs := make([]int, N)
		bs[N-1] = a[N-1]
		for i := N - 2; i > -1; i-- {
			bs[i] = bs[i+1] + a[i]
		}

		for i := 0; i < N-1; i++ {
			t := 0
			if a[i] == 0 {
				t = bs[i+1]
			} else {
				t = (N - (i + 1)) - bs[i+1]
			}
			B := (1 << uint(b)) % 1000000007
			result += (t * B) % 1000000007
			result = result % 1000000007
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

## C - HonestOrUnkind2

35分くらい?で突破. N≤15 なので、全組み合わせやっても 2<sup>15</sup> でしかなく、やるだけ. だけって言ってもコーディングは中々大変. 最初 `for i in range(1, 2 ** N):` が `for i in range(1, 2 ** N - 1):` になってて WA3 だった. Dが終わった後見直していたら気づいた.

N人の「正直者」、「不親切な人」のすべての組み合わせを生成し、正直者の証言が組み合わせに矛盾していないか確認する. 矛盾していない組み合わせの正直者の最大数が解答.
組み合わせは bit で表現している. Nビット用意し、1が立っていると正直者、0だと不親切な人. この組み合わせ状況と正直者の証言が矛盾しないか確認する.

```python
N = int(input())

s = [[] for _ in range(N)]
for i in range(N):
  A = int(input())
  for _ in range(A):
    x, y = map(int, input().split())
    s[i].append((x - 1, y))

result = 0
for i in range(1, 2 ** N):
  t = [-1] * N
  for j in range(N):
    t[j] = (i >> j) & 1
  m = False
  for j in range(N):
    if (i >> j) & 1 == 0:
      continue
    for x, y in s[j]:
      if t[x] != y:
        m = True
        break
  if not m:
    result = max(result, bin(i)[2:].count('1'))
print(result)
```

## E - Balanced Path

突破できず. 貪欲法では駄目なのは分かるけど、じゃあどうすればいいというのが思いつかなかった. 3次元DPかー(解説を見て). 解説読んでもさっぱりわかりませんね.
