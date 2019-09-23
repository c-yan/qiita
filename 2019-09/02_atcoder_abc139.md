# AtCoder Beginner Contest 139 参戦記

## A - Tenki

2分半で突破. 書くだけ.

```python
s = input()
t = input()
print(sum(1 for i in range(3) if s[i] == t[i]))
```

## B - Power Socket

9分半で突破. 時間かかりすぎな上に、WA1.
最初は2行で書いてたんだけど、入出力例3でコケて、日和って while 文が登場しました.
そして1個も買わないことは想定してなかったので WA を食らう.

```python
a, b = map(int, input().split())
n = 0
t = 1
while t < b:
  t += a - 1
  n += 1
print(n)
```

終わった後に書いた while 無し版

```python
a, b = map(int, input().split())
print(((b - 1) + (a - 2)) // (a - 1))
```

## C - Lower

4分半で突破. 書くだけ. 最近、C問題が簡単な気がする.

```python
n = int(input())
h = list(map(int, input().split()))
t = 0
result = 0
for i in range(n - 1):
  if h[i + 1] <= h[i]:
    t += 1
  else:
    result = max(t, result)
    t = 0
result = max(t, result)
print(result)
```

## D - ModSum

5分で突破. n = 3 と n = 4 を手でやって、あまりの簡単さに困惑した. 前回のDも簡単だったけど、今回のは輪をかけて簡単でおかしい.

```python
n = int(input())
print((n + 1) * n // 2 - n)
```

これ、どう考えても

```python
n = int(input())
print(n * (n - 1) // 2)
```

でいいのにそうなっていないのは、1～n の合計を `(n + 1) * n // 2` として覚えていて、1～n-1 の合計の式に自信がなかった日和である. (今回日和が多いですね)

## E - League

TLE 1個を消せずに敗退.

コンテスト後に通した Python 版のコード. 概ね Youtube 解説のコード通りだけど以下の違いがある.

1. ノードの接続向きは逆にしてない (というか、何のために逆にしてるんですかね)
2. visited, calculated は dp テーブルの値で表現しているので存在しない. (-1 が visited=false で、0 が calculated=true)
3. 全ノード回しはせず、各選手の初戦ノードだけを dfs の開始ポイントとしている)

後は細かく最適化が入っている(id 生成するところの if 文 + continue が無くなっていたり). 中々 TLE が無くならなくて難儀したけど、一番効いたのが max を if 文にすることだったのでげっそり.

```python
from sys import exit, setrecursionlimit


def get_node_id(i, j, node_id_db):
    if i < j:
        return node_id_db[i][j]
    else:
        return node_id_db[j][i]


def dfs(node_id, edges, dp):
    t = dp[node_id]
    if t >= 0:
        return t
    dp[node_id] = 0
    result = 0
    for n in edges[node_id]:
        t = dfs(n, edges, dp)
        if t == 0:  # looped
            print(-1)
            exit()
        if t > result:
            result = t
    result += 1
    dp[node_id] = result
    return result


def main():
    N = int(input())
    A = [[int(e) - 1 for e in input().split()] for _ in range(N)]

    node_id_db = [[0] * N for _ in range(N)]
    v = 0
    for i in range(N):
        for j in range(i + 1, N):
            node_id_db[i][j] = v
            v += 1

    start_nodes = []
    edges = [[] for _ in range(N * (N - 1) // 2)]
    for i in range(N):
        Ai = A[i]
        from_id = get_node_id(i, Ai[0], node_id_db)
        if i < Ai[0]:
            start_nodes.append(from_id)
        for j in range(1, N - 1):
            to_id = get_node_id(i, Ai[j], node_id_db)
            edges[from_id].append(to_id)
            from_id = to_id

    dp = [-1] * (N * (N - 1) // 2)
    print(max(dfs(n, edges, dp) for n in start_nodes))


setrecursionlimit(1000000)
main()
```

もう一つの解法版. こちらは前日に試合のあった人(とその対戦相手)だけ試合がありうるという知識でループの無駄を減らしている. Youtube でも説明されているけど、それを見た人の TL の書き込みで考え方を知って書いたので、すぬけさんのソースコードには影響されていない. コンテスト中に書いたナイーブなコードを元にしているので実装もすぐ終わった. この方式の Python 版も書いてみたけど、TLE が消えなかったので Go 言語版で掲載.

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
	a := make([][]int, N)
	for i := 0; i < N; i++ {
		a[i] = make([]int, N-1)
		for j := 0; j < N-1; j++ {
			a[i][j] = readInt() - 1
		}
	}

	nextIndex := make([]int, N)
	nextQueue := make([]int, 0, N)
	for i := 0; i < N; i++ {
		nextQueue = append(nextQueue, i)
	}
	result := 0
	finished := 0

	for len(nextQueue) > 0 {
		result++
		queue := nextQueue
		nextQueue = nil
		booked := make([]bool, N)
		for i := 0; i < len(queue); i++ {
			player := queue[i]
			if booked[player] {
				continue
			}
			opponent := a[player][nextIndex[player]]
			if booked[opponent] {
				continue
			}
			if a[opponent][nextIndex[opponent]] != player {
				continue
			}
			booked[player] = true
			booked[opponent] = true
			nextIndex[player]++
			nextIndex[opponent]++
			if nextIndex[player] == N-1 {
				finished++
			} else {
				nextQueue = append(nextQueue, player)
			}
			if nextIndex[opponent] == N-1 {
				finished++
			} else {
				nextQueue = append(nextQueue, opponent)
			}
		}
	}
	if finished == N {
		fmt.Println(result)
	} else {
		fmt.Println(-1)
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
```

## F - Engines

ナイーブに書いたら TLE 食らいまくって、計算量減らしたら後半半分くらい WA になって敗退.
