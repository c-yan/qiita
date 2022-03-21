# AtCoder Beginner Contest 244 参戦記

## [ABC244A - Last Letter](https://atcoder.jp/contests/abc244/tasks/abc244_a)

1分半で突破. 書くだけ.

```python
N = int(input())
S = input()

print(S[-1])
```

## [ABC244B - Go Straight and Turn Right](https://atcoder.jp/contests/abc244/tasks/abc244_b)

4分で突破. 書くだけ.

```python
N = int(input())
T = input()

d = 0
t = [(1, 0), (0, -1), (-1, 0), (0, 1)]
x, y = 0, 0
for c in T:
    if c == 'S':
        dx, dy = t[d]
        x += dx
        y += dy
    elif c == 'R':
        d += 1
        d %= 4
print(x, y)
```

## [ABC244C - Yamanote Line Game](https://atcoder.jp/contests/abc244/tasks/abc244_c)

10分で突破. リストでやると TLE するかなーと思っていたが、後で試したらそんなこともなかった. インタラクティブは珍しい.

```python
N = int(input())

s = set(range(1, 2 * N + 2))
while True:
    print(s.pop())
    x = int(input())
    if x == 0:
        break
    s.remove(x)
```

## [ABC244D - Swap Hats](https://atcoder.jp/contests/abc244/tasks/abc244_d)

9分で突破、WA1. 出した瞬間に No を Np と typo してることに気づいてグエッとなった. 揃っている状態から1回交換している状態と同等だと駄目で、それは2つ違っている状態.

```python
S = input().split()
T = input().split()

c = 0
for i in range(3):
    if S[i] != T[i]:
        c += 1

if c == 2:
    print('No')
else:
    print('Yes')
```

## [ABC244E - King Bombee](https://atcoder.jp/contests/abc244/tasks/abc244_e)

27分半で突破. 偶奇分けする必要こそあるものの、DPするだけじゃんってなった. どこがキングボンビーなのかが分からない.

```python
from sys import stdin

readline = stdin.readline
m = 998244353

N, M, K, S, T, X = map(int, readline().split())
S, T, X = S - 1, T - 1, X - 1
UV = [tuple(map(lambda x: int(x) - 1, readline().split())) for _ in range(M)]

dp = [[0] * N for _ in range(2)]
dp[0][S] = 1
for _ in range(K):
    t = [[0] * N for _ in range(2)]
    for u, v in UV:
        if v == X:
            t[1][v] += dp[0][u]
            t[1][v] %= m
            t[0][v] += dp[1][u]
            t[0][v] %= m
        else:
            t[0][v] += dp[0][u]
            t[0][v] %= m
            t[1][v] += dp[1][u]
            t[1][v] %= m
        if u == X:
            t[1][u] += dp[0][v]
            t[1][u] %= m
            t[0][u] += dp[1][v]
            t[0][u] %= m
        else:
            t[0][u] += dp[0][v]
            t[0][u] %= m
            t[1][u] += dp[1][v]
            t[1][u] %= m
    dp = t
print(dp[0][T])
```
