# yukicoder contest 292 参戦記

## [A 1485 運賃](https://yukicoder.me/problems/no/1485)

電車とバスの運賃を足すだけです.

```python
n, m = map(int, input().split())

print(n + m)
```

## [B 1486 ロボット](https://yukicoder.me/problems/no/1486)

*E*≦10<sup>9</sup>なので*O*(*E*)は無理だが、*F*=gcd(*A*+*B*,*C*+*D*)としたときに、*F*≦10<sup>6</sup>なので*O*(*F*)は可能. なので*F*秒間で2台のロボットが同時に動いている秒数をループで求める. あとは*E*を*F*で割ったあまりの秒数についても同様にループを回して同時に動いている秒数をを求めれば、最終的な答えが求まる.

```python
from math import gcd

A, B, C, D, E = map(int, input().split())

x = (A + B) * (C + D) // gcd(A + B, C + D)
y = ([True] * A + [False] * B) * (x // (A + B))
z = ([True] * C + [False] * D) * (x // (C + D))

c = 0
for i in range(x):
    if not y[i] or not z[i]:
        continue
    c += 1

result = E // x * c
for i in range(E % x):
    if not y[i] or not z[i]:
        continue
    result += 1
print(result)
```

## [C 1487 ぺんぎんさんかっけー](https://yukicoder.me/problems/no/1487)

3辺の長さから三角形の面積を求める公式はググればすぐ見つかる. 直感的に各辺が半分になって2次元なので2乗されて1/4となる. サンプルで答えが合ったので、直感通りのようだ.

```python
from math import sqrt

def f(a, b, c):
    s = (a + b + c) / 2
    return sqrt(s * (s - a) * (s - b) * (s - c))

a, b, c = map(int, input().split())

print(f(a, b, c) / 4)
```

## [D 1488 Max Score of the Tree](https://yukicoder.me/problems/no/1488)

ある辺の長さを2倍にするとどれだけスコアが伸びるかというと、その辺の下にぶら下がっている葉の数×辺の長さだけ増える. DFS でそれぞれの辺の下にある葉の数と最初のスコアを求め、DP で増やせるスコアの最大値を求めれば解ける.

```python
from sys import stdin

readline = stdin.readline

N, K = map(int, readline().split())
links = [{} for _ in range(N)]
for _ in range(N - 1):
    a, b, c = map(int, readline().split())
    a, b = a - 1, b - 1
    links[a][b] = c
    links[b][a] = c


def dfs(parent):
    l = links[parent]
    if len(l) == 0:
        return (0, 1)

    total_score = 0
    total_leaves = 0
    for child in l:
        del links[child][parent]
        score, leaves = dfs(child)
        total_score += score
        total_leaves += leaves
        t.append((l[child], leaves))
        total_score += l[child] * leaves
    return total_score, total_leaves


t = []
result, _ = dfs(0)

dp = [-1] * (K + 1)
dp[0] = 0
for i in range(len(t)):
    for j in range(K - 1,  -1, -1):
        if dp[j] == -1:
            continue
        a = j + t[i][0]
        if a > K:
            continue
        dp[a] = max(dp[a], dp[j] + t[i][0] * t[i][1])
result += max(dp)
print(result)
```
