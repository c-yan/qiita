# AtCoder Beginner Contest 180 参戦記

## [ABC180A - box](https://atcoder.jp/contests/abc180/tasks/abc180_a)

1分で突破. 書くだけ.

```python
N, A, B = map(int, input().split())

print(N - A + B)
```

## [ABC180B - Various distances](https://atcoder.jp/contests/abc180/tasks/abc180_b)

3分で突破. 書くだけ.

```python
N, *x = map(int, open(0).read().split())

print(sum(abs(a) for a in x))
print(sum(a * a for a in x) ** 0.5)
print(max(abs(a) for a in x))
```

## [ABC180C - Cream puff](https://atcoder.jp/contests/abc180/tasks/abc180_c)

3分で突破. 書くだけ. Nの二乗根まで回すと分かっていれば難しいことはないんじゃないかな.

```python
N = int(input())

result = set()
for i in range(1, int(N ** 0.5) + 1):
    if N % i == 0:
        result.add(i)
        result.add(N // i)
print(*sorted(result), sep='\n')
```

## [ABC180D - Takahashi Unevolved](https://atcoder.jp/contests/abc180/tasks/abc180_d)

14分で突破. 時間かかりすぎ. 素直にシミュレートするナイーブなコードだと B が小さい場合に TLE になることは火を見るよりも明らか. となると、`+ B` より `* A` のほうが増分が小さいうちは `* A` をして、残りはまとめて足せば良いんじゃないのという結論に落ち着く. A は少なくとも2なので、素直にシミュレートしても *O*(log<i>N</i>) なので問題ない.

```python
X, Y, A, B = map(int, input().split())

result = 0
while X * A < Y and X * A < X + B:
    X *= A
    result += 1
result += ((Y - 1) - X) // B
print(result)
```

追記: どうにもD問題でペナ食らった人が多いなあと思っていたが、工夫せずに書くと `X * A` が int64 でもオーバーフローするせいなのね. なるほど.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	defer flush()

	X := readInt()
	Y := readInt()
	A := readInt()
	B := readInt()

	result := 0
	for X <= (Y-1)/A && X*A < X+B {
		X *= A
		result++
	}
	result += ((Y - 1) - X) / B
	println(result)
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

## [ABC180E - Traveling Salesman among Aerial Cities](https://atcoder.jp/contests/abc180/tasks/abc180_e)

39分半で突破. 蟻本のコードを写経して解いた. 写経してあれば黄色パフォとか取れたかな、くそー.

```c++
#include <bits/stdc++.h>
#define rep(i, a) for (int i = (int)0; i < (int)a; ++i)
using ll = long long;
using namespace std;

#define MAX_N 17
#define INF 2147483647

ll N;
ll dp[1 << MAX_N][MAX_N];
ll d[MAX_N][MAX_N];

void solve() {
    rep(i, 1 << N) rep(j, N) dp[i][j] = INF;
    dp[(1 << N) - 1][0] = 0;

    for (int S = (1 << N) - 2; S >= 0; S--) {
        for (int v = 0; v < N; v++) {
            for (int u = 0; u < N; u++) {
                dp[S][v] = min(dp[S][v], dp[S | 1 << u][u] + d[v][u]);
            }
        }
    }
    cout << dp[0][0] << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    cin >> N;

    vector<ll> X(N), Y(N), Z(N);
    rep(i, N) {
        cin >> X[i] >> Y[i] >> Z[i];
    }

    rep(i, N) {
        rep(j, N) {
            d[i][j] = abs(X[i] - X[j]) + abs(Y[i] - Y[j]) + max(0ll, Z[i] - Z[j]);
        }
    }

    solve();

    return 0;
}
```
