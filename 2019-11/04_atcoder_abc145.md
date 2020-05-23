# AtCoder Beginner Contest 145 参戦記

## [ABC145A - Circle](https://atcoder.jp/contests/abc145/tasks/abc145_a)

1分で突破. 書くだけ.

```python
r = int(input())

print(r * r)
```

## [ABC145B - Echo](https://atcoder.jp/contests/abc145/tasks/abc145_b)

3分半で突破. 長さが偶数であることを確認することを忘れなければいいだけ.

```python
from sys import exit

N = int(input())
S = input()

if N % 2 == 1:
    print('No')
    exit()

if S[:N // 2] == S[N // 2:]:
    print('Yes')
else:
    print('No')
```

## [ABC145C - Average Length](https://atcoder.jp/contests/abc145/tasks/abc145_c)

8分で突破. すべての組み合わせの距離の合計を平均を取ればいいのかなって思って書いたら入力例が通ったので提出したら AC 出たけど、解説を読むとまぐれで通っただけかってなった.

```python
from math import sqrt

N = int(input())
xy = [list(map(int, input().split())) for _ in range(N)]

result = 0
for i in range(N):
    for j in range(N):
        if i != j:
            result += sqrt((xy[i][0] - xy[j][0]) * (xy[i][0] - xy[j][0]) + (xy[i][1] - xy[j][1]) * (xy[i][1] - xy[j][1]))
print(result / N)
```

## [ABC145D - Knight](https://atcoder.jp/contests/abc145/tasks/abc145_d)

85分で突破. RE4 WA1 だけど、終了1分半前にギリギリ AC. 最初 DP しようとしてメモリ溢れ. 配列を循環使用に変えたら計算時間溢れ. こりゃ方針が駄目だなと考慮モードに. X, Y が小さい時の DP テーブルを眺めたら、値が入っているマスがあまりに少なく、X, Y 到着時の2つの動きのそれぞれの回数は一パターンしか無いことに気づく. ちょこちょこ連立方程式を解いて `a = (2*Y - X) / 3, b = (2*X - Y) / 3` であることを求め、求める解が <sub>a+b</sub>C<sub>min(a, b)</sub> であるところまで辿り着く. ABC132D を解いた時の解説動画で、すぬけさんが「今回は値が小さいのでパスカルの三角形が使える」みたいな事を言っていたので、フェルマーの小定理がいるじゃんとなってググったら C++ の実装が出てきたので Python に移植. 試したら速度的に全然駄目だったので、Go に再移植して提出したら RE 2つで後ちょっと. インデックスがはみ出てるのかと思ってスライスを大きくして提出で追加で2回 RE をくらい、冷静になったところで 99999 3 を試して、b がマイナスになっていることに気づき、マイナスの場合は 0 を出力するように直して提出……たら、デバッグ出力が残ってて WA orz. 消して出し直して AC. なので、変数名がいつもとぜんぜん違うところは、移植コードを自分流に直す時間がなかったせいです…….

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

const (
	M = 1000000007
)

var (
	fac  []int
	ifac []int
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func mpow(x int, n int) int {
	ans := 1
	for n != 0 {
		if n&1 == 1 {
			ans = ans * x % M
		}
		x = x * x % M
		n = n >> 1
	}
	return ans
}

func comb(a int, b int) int {
	if a == 0 && b == 0 {
		return 1
	}
	if a < b || a < 0 {
		return 0
	}
	tmp := ifac[a-b] * ifac[b] % M
	return tmp * fac[a] % M
}

func main() {
	X := readInt()
	Y := readInt()

	if (X+Y)%3 != 0 {
		fmt.Println(0)
		return
	}

	fac = make([]int, 666667)
	ifac = make([]int, 666667)

	a := (2*Y - X) / 3
	b := (2*X - Y) / 3

	if a < 0 || b < 0 {
		fmt.Println(0)
		return
	}

	fac[0] = 1
	ifac[0] = 1
	for i := 0; i < 666666; i++ {
		fac[i+1] = fac[i] * (i + 1) % M
		ifac[i+1] = ifac[i] * mpow(i+1, M-2) % M
	}

	fmt.Println(comb(a+b, min(a, b)) % M)
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

追記: Python で書き直した.

```python
p = 1000000007

X, Y = map(int, input().split())

if (X + Y) % 3 != 0:
    print(0)
    exit()

a = (2 * Y - X) // 3
b = (2 * X - Y) // 3

if a < 0 or b < 0:
    print(0)
    exit()

n = a + b
fac = [0] * (n + 1)
fac[0] = 1
for i in range(n):
    fac[i + 1] = fac[i] * (i + 1) % p


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], p - 2, p) * pow(fac[k], p - 2, p) % p


print(mcomb(n, a))
```

## [ABC145E - All-you-can-eat](https://atcoder.jp/contests/abc145/tasks/abc145_e)

着手すらできず. ナップサックではあるんだけど、一発だけはみ出すことが出来るという. ルールを守った組み合わせは、一番時間がかかるやつを最後のものと入れ替えてもルール通りのままだが、逆は必ずしもそうではないので、食べるのに時間がかかるのを後に回したほうがたくさん食べれる. よって食べるのにかかる時間順にソートしてDPすればよい. 個人的にはD問題より簡単な気がする.

```python
def main():
    N, T = map(int, input().split())
    AB = [list(map(int, input().split())) for _ in range(N)]

    dp = [-1] * (T + 3000)
    dp[0] = 0
    for a, b in sorted(AB):
        for i in range(T - 1, -1, -1):
            if dp[i] == -1:
                continue
            if dp[i + a] < dp[i] + b:
                dp[i + a] = dp[i] + b
    print(max(dp))


main()
```
