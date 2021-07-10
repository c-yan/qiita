# AtCoder Beginner Contest 209 参戦記

連敗をようやくストップできたのに、結局その1回だけで、また連敗入り…….

## [ABC209A - Counting](https://atcoder.jp/contests/abc209/tasks/abc209_a)

1分半で突破. 書くだけ.

```python
A, B = map(int, input().split())

if A > B:
    print(0)
else:
    print(B - A + 1)
```

## [ABC209B - Can you buy them all?](https://atcoder.jp/contests/abc209/tasks/abc209_b)

2分で突破. 書くだけ.

```python
N, X, *A = map(int, open(0).read().split())

if sum(A) - N // 2 <= X:
    print('Yes')
else:
    print('No')
```

## [ABC209D - Collision](https://atcoder.jp/contests/abc209/tasks/abc209_d)

15分?で突破. 街cと街dの間の距離が奇数なら道路上で、偶数なら街で遭遇する. *N*≦10<sup>5</sup> なのでワーシャルフロイドで全街間の距離を計算することは出来ない. *N* 個の街と *N*−1 本の道路という条件と、どの街からどの街へもいくつかの道路を通ることで移動できるという条件から木構造(=ループがない)であることが分かる. であれば、ある特定の街aからその他の街まで最短の距離を全て求めれば任意の街cと街dの距離の偶奇が*O*(1)で分かる. 街iから街jまでの距離を距離<sub>ij</sub>とする. 街aから街cを通らないと街dに行けない場合、距離<sub>cd</sub>=距離<sub>ad</sub>-距離<sub>ac</sub>となる. 街aから街cを通らずに街dに行け、なおかつ街aから街dを通らずに街cに行ける場合、街aから街cや街dに行くときは途中の街bまで同じ道を通ることになるが(a=bのこともある)、その場合距離<sub>cd</sub>=距離<sub>ac</sub>+距離<sub>ad</sub>-2×距離<sub>ab</sub>となる. 結論的に距離<sub>ac</sub>+距離<sub>ad</sub>は常に距離<sub>cd</sub>の偶奇と一致する.

```python
from collections import deque
from sys import stdin

readline = stdin.readline

INF = 10 ** 15

N, Q = map(int, readline().split())

links = [[] for _ in range(N)]
for _ in range(N - 1):
    a, b = map(lambda x: int(x) - 1, readline().split())
    links[a].append(b)
    links[b].append(a)

dp = [INF] * N
dp[0] = 0
q = deque([0])
while q:
    a = q.popleft()
    for b in links[a]:
        if dp[b] <= dp[a] + 1:
            continue
        dp[b] = dp[a] + 1
        q.append(b)

result = []
for _ in range(Q):
    c, d = map(lambda x: int(x) - 1, readline().split())
    if (dp[c] + dp[d]) % 2 == 0:
        result.append('Town')
    else:
        result.append('Road')
print(*result, sep='\n')
```

## [ABC209C - Not Equal](https://atcoder.jp/contests/abc209/tasks/abc209_c)

38分半?で突破. ものすごい勢いで解かれているのに全然分からなくて焦ったし、これのせいでパフォを大きく落とした. 分かってしまえば簡単で、Cは順序を変えても答えが変わらないので、昇順に並び替えて良い. 並び替えれば C<sub>i</sub>-(i-1) を掛け続けるだけというのはすぐ分かる. i番目を処理するときには、既にそこまでに i-1 個が使われていて選択肢にならないからである. 昇順に並んでいれば瞬殺じゃんと分かった瞬間に、これだけ解かれているから並び替えていいに決まっているという腐った読みで突破. 後で考えてみれば A<sub>i</sub>≠A<sub>j</sub> の条件で i, j の前後は問われていない=順序に依存性がないので、順序を変えていいことは分かった.

```python
m = 1000000007

N, *C = map(int, open(0).read().split())

C.sort()
result = 1
for i in range(N):
    result *= (C[i] - i)
    result %= m
print(result)
```

## [ABC209E - Shiritori](https://atcoder.jp/contests/abc209/tasks/abc209_e)

突破できず. ループ検出+メモ化再帰で解けたかなあと思ったら WA. そうすると端点から順に確定していくしか無いのかなあと考え始めた辺りでタイムアップ. 後退解析という言葉を知らなかった時点で敗退が決定づけられていた気はする.
