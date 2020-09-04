# AtCoder Beginner Contest 177 参戦記

レーティングが5回連続下がっていたので、久々に上がって安心した.

## [ABC177A - Don't be late](https://atcoder.jp/contests/abc177/tasks/abc177_a)

2分で突破. 浮動小数点数は誤差が怖いので、T分間で歩ける距離がDメートル以上かでジャッジするようにした.

```python
D, T, S = map(int, input().split())

if D > S * T:
    print('No')
else:
    print('Yes')
```

## [ABC177B - Substring](https://atcoder.jp/contests/abc177/tasks/abc177_b)

5分で突破、WA1. + 1 を忘れて死亡. ずらしながら違う文字が何文字があるかを調べて、最小値を取れば OK. Sの長さが1000なので、計算量は最大でも2.5×10<sup>4</sup>.

```python
S = input()
T = input()

result = float('inf')
for i in range(len(S) - len(T) + 1):
    t = 0
    for j in range(len(T)):
        if S[i + j] != T[j]:
            t += 1
    result = min(result, t)
print(result)
```

## [ABC177C - Sum of product of pairs](https://atcoder.jp/contests/abc177/tasks/abc177_c)

4分で突破. ナイーブに2重ループで計算すると、計算量が2×10<sup>10</sup>で TLE してしまうので累積和した. 右から順番にやっていけば累積和いらないけど、まあいいでしょ.

```python
from itertools import accumulate

m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
b = list(accumulate(A))
for i in range(N):
    result += A[i] * (b[N - 1] - b[i])
    result %= m
print(result)
```

追記: 累積和を使わずに解いてみた.

```python
m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
c = 0
for i in range(1, N + 1):
    result += A[N - i] * c
    result %= m
    c += A[N - i]
    c %= m
print(result)
```

追々記: 右からやる必要もないな.

```python
m = 1000000007

N, *A = map(int, open(0).read().split())

result = 0
c = 0
for a in A:
    result += a * c
    result %= m
    c += a
    c %= m
print(result)
```

## [ABC177D - Friends](https://atcoder.jp/contests/abc177/tasks/abc177_d)

4分で突破. 見るからに Union Find. 一番大きいユニオンに属する人数の数だけグループを作ればいい.

```python
from sys import setrecursionlimit, stdin


def find(parent, i):
    t = parent[i]
    if t < 0:
        return i
    t = find(parent, t)
    parent[i] = t
    return t


def unite(parent, i, j):
    i = find(parent, i)
    j = find(parent, j)
    if i == j:
        return
    parent[j] += parent[i]
    parent[i] = j


readline = stdin.readline
setrecursionlimit(10 ** 6)

N, M = map(int, readline().split())

parent = [-1] * N
for _ in range(M):
    A, B = map(lambda x: int(x) - 1, readline().split())
    unite(parent, A, B)

print(-min(parent))
```

## [ABC177E - Coprime](https://atcoder.jp/contests/abc177/tasks/abc177_e)

23分で突破. 右から素因数分解していけば行けるなと思った. setwise coprime かどうかも一緒に計算できそうな気がしたけど、時間をかけるのはパフォが落ちるので素直に別に計算した.

```python
from math import gcd
from functools import reduce


def make_prime_table(n):
    sieve = list(range(n + 1))
    sieve[0] = -1
    sieve[1] = -1
    for i in range(4, n + 1, 2):
        sieve[i] = 2
    for i in range(3, int(n ** 0.5) + 1, 2):
        if sieve[i] != i:
            continue
        for j in range(i * i, n + 1, i * 2):
            if sieve[j] == j:
                sieve[j] = i
    return sieve


def prime_factorize(n):
    result = []
    while n != 1:
        p = prime_table[n]
        e = 0
        while n % p == 0:
            n //= p
            e += 1
        result.append((p, e))
    return result


def is_pairwise_coprime():
    s = set()
    for i in range(N - 1, -1, -1):
        t = prime_factorize(A[i])
        for p, _ in t:
            if p in s:
                return False
            s.add(p)
    return True


N, *A = map(int, open(0).read().split())

prime_table = make_prime_table(10 ** 6)
if is_pairwise_coprime():
    print('pairwise coprime')
elif reduce(gcd, A) == 1:
    print('setwise coprime')
else:
    print('not coprime')
```

追記: 公式解説動画を見たら素因数分解せずに解いてた. なるほどー.

```python
from math import gcd
from functools import reduce

N, *A = map(int, open(0).read().split())

def is_pairwise_coprime():
    t = [0] * (10 ** 6 + 1)
    for a in A:
        t[a] += 1

    c = 0
    for j in range(2, 10 ** 6 + 1, 2):
        c += t[j]
    if c > 1:
        return False

    for i in range(3, 10 ** 6 + 1, 2):
        c = 0
        for j in range(i, 10 ** 6 + 1, i):
            c += t[j]
        if c > 1:
            return False

    return True

if is_pairwise_coprime():
    print('pairwise coprime')
elif reduce(gcd, A) == 1:
    print('setwise coprime')
else:
    print('not coprime')
```
