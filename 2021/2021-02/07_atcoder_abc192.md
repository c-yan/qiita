# SOMPO HD プログラミングコンテスト2021(AtCoder Beginner Contest 192) 参戦記

## [ABC192A - Star](https://atcoder.jp/contests/abc192/tasks/abc192_a)

1分で突破. 書くだけ.

```python
X = int(input())

print(100 - X % 100)
```

## [ABC192B - uNrEaDaBlE sTrInG](https://atcoder.jp/contests/abc192/tasks/abc192_b)

3分で突破. 書くだけだけど、0-indexed と 1-indexed で偶奇が入れ替わるのを間違えて無駄に時間を取った.

```python
S = input()

for i in range(0, len(S), 2):
    if S[i] != S[i].lower():
        print('No')
        exit()

for i in range(1, len(S), 2):
    if S[i] != S[i].upper():
        print('No')
        exit()

print('Yes')
```

## [ABC192C - Kaprekar Number](https://atcoder.jp/contests/abc192/tasks/abc192_c)

6分で突破. 書くだけだったけど、計算量が計算できなくて、大丈夫かなってなった.

```python
def g1(x):
    return int(''.join(sorted(str(x), reverse=True)))


def g2(x):
    return int(''.join(sorted(str(x))))


def f(x):
    return g1(x) - g2(x)


N, K = map(int, input().split())

a = N
for i in range(K):
    a = f(a)
print(a)
```

## [ABC192E - Train](https://atcoder.jp/contests/abc192/tasks/abc192_e)

24分で突破. WA1. 最初、特定の都市間には一つしか路線がないように書いて WA した、まぬけ. まあ、ダイクストラすればいいだけですね.

```python
from sys import stdin
from heapq import heappop, heappush

readline = stdin.readline
INF = 10 ** 18

N, M, X, Y = map(int, readline().split())

links = [{} for _ in range(N)]
for _ in range(M):
    A, B, T, K = map(int, readline().split())
    links[A - 1].setdefault(B - 1, [])
    links[B - 1].setdefault(A - 1, [])
    links[A - 1][B - 1].append((T, K))
    links[B - 1][A - 1].append((T, K))

dp = [INF] * N
dp[X - 1] = 0
q = [(0, X - 1)]
while len(q) != 0:
    n, i = heappop(q)
    link = links[i]
    for j in link:
        for t, k in link[j]:
            d = (n + k - 1) // k * k
            if d + t >= dp[j]:
                continue
            dp[j] = d + t
            heappush(q, (d + t, j))

if dp[Y - 1] == INF:
    print(-1)
else:
    print(dp[Y - 1])
```


## [ABC192D - Base n](https://atcoder.jp/contests/abc192/tasks/abc192_d)

48分半で突破. WA5. Xが1桁のときの特別性に気づくのに時間がかかったし、気づいた後も求められているのが*n*進法表記の数とみなして得られる値の**種類数**であることになかなか気づかなかった. やらかしたなあ.

```python
def f(s, n):
    result = 0
    b = 1
    for c in s[::-1]:
        result += int(c) * b
        b *= n
    return result


INF = 10 ** 19

X = input()
M = int(input())

d = int(max(X))

ok = d
ng = INF + 1
while ng - ok > 1:
    m = ok + (ng - ok) // 2
    if f(X, m) <= M:
        ok = m
    else:
        ng = m

if ok == INF:
    print(1)
else:
    print(ok - d)
```
