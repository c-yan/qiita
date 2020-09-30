# yukicoder contest 250 参戦記

## [A 1063 ルートの計算 / Sqrt Calculation](https://yukicoder.me/problems/no/1063)

n を割ることが出来る最大の i<sup>2</sup> が見つかれば答えは、i と n÷i<sup>2</sup> となる.

```python
n = int(input())

for i in range(10 ** 5, 0, -1):
    if n % (i * i) == 0:
        print(i, n // (i * i))
        break
```

## [B 1064 ∪∩∩ / Cup Cap Cap](https://yukicoder.me/problems/no/1064)

f(x) = C<sub>1</sub> - C<sub>2</sub> を考える. f(x) = 2x<sup>2</sup>+(a-c)x+(b-d) となる. 当然 x = +∞ 及び x = -∞ の時は f(x) = +∞ である. 最も小さいのは f'(x) = 4x + (a-c) = 0 → x = (c - a) / 4 のときである. f((c - a) / 4) が正の場合は交点無し、0の場合は交点1つ、負の場合は交点が2つとなる. 2つの交点は -∞ .. (c - a) / 4 と (c - a) / 4 .. +∞ にあるので二分探索で f(x) = 0 となる x を探せば良い. 後は二点を通る直線の方程式をググって書いておしまい.

```python
a, b, c, d = map(int, input().split())


def f(x):
    return 2 * x * x + (a - c) * x + (b - d)


t = f((c - a) / 4)

if abs(t) < 1e-6:
    print('Yes')
    exit()

if t > 0:
    print('No')
    exit()

ok = 1e12
ng = (c - a) / 4
for _ in range(100):
    m = (ok + ng) / 2
    if f(m) > 0:
        ok = m
    else:
        ng = m
x1 = ok
y1 = x1 * x1 + a * x1 + b

ok = -1e12
ng = (c - a) / 4
for _ in range(100):
    m = (ok + ng) / 2
    if f(m) > 0:
        ok = m
    else:
        ng = m
x2 = ok
y2 = x2 * x2 + a * x2 + b

p = (y2 - y1) / (x2 - x1)
q = y1 - p * x1
print(p, q)
```

## [C 1065 電柱 / Pole (Easy)](https://yukicoder.me/problems/no/1065)

コストを座標から計算しないといけないことを除けば、重み付きグラフの最短路計算でしか無いので、普通に幅優先探索で解ける. といいつつ、以下のコードは PyPy でないと TLE になる(笑).

```python
# PyPy でのみ AC
from math import sqrt
from sys import stdin
from collections import deque

readline = stdin.readline

N, M = map(int, readline().split())
X, Y = map(lambda x: int(x) - 1, readline().split())
pq = [list(map(int, readline().split())) for _ in range(N)]
PQ = [list(map(int, readline().split())) for _ in range(M)]

links = [[] for _ in range(N)]
for P, Q in PQ:
    links[P - 1].append(Q - 1)
    links[Q - 1].append(P - 1)

q = deque([X])
t = [float('inf')] * N
t[X] = 0
while q:
    i = q.popleft()
    for j in links[i]:
        a, b = pq[i]
        c, d = pq[j]
        k = t[i] + sqrt((a - c) * (a -c) + (b - d) * (b - d))
        if k < t[j]:
            t[j] = k
            q.append(j)
print(t[Y])
```

## [D 1066 #いろいろな色 / Red and Blue and more various colors (Easy)](https://yukicoder.me/problems/no/1066)

敗退. ナイーブなのは書けたけど当然 TLE. というか、B が1個でも、N=6000、B1=3000で TLE するのに(笑).

追記: 解説の DP を1次元化したがそれでも Python では TLE だった.

```python
# PyPy でのみ AC
N, Q = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

dp = [0] * (N + 1)
dp[1] = 1
dp[0] = A[0] - 1
for i in range(1, N):
    for j in range(i, -1, -1):
        dp[j + 1] += dp[j]
        dp[j + 1] %= 998244353
        dp[j] *= A[i] - 1
        dp[j] %= 998244353

print('\n'.join(str(dp[b]) for b in B))
```
