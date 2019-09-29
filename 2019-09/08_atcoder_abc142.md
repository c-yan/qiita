# AtCoder Beginner Contest 142 参戦記

## A - Odds of Oddness

2分半で突破. A問題なのにあってるのかなと考える時点でいつものA問題より難しい. これは書くだけとは言えない.

```python
N = int(input())
print(int(N / 2 + 0.5) / N)
```

## B - Roller Coaster

3分で突破. 書くだけ. 今回はある意味A問題よりB問題のほうが簡単だった.

```python
N, K = map(int, input().split())
h = list(map(int, input().split()))
print(sum(1 if t >= K else 0 for t in h))
```

## C - Go to School

7分半で突破. 問題そのものは簡単なんだけど、タプルの第二要素でソートするのどうやるんだっけみたいな部分を調べるのに時間を使った.

```python
from operator import itemgetter

N = int(input())
A = list(map(int, input().split()))

print(*[t[0] + 1 for t in sorted(enumerate(A), key=itemgetter(1))])
```

## D - Disjoint Set of Common Divisors

WA が消せず. 最大公約数を取り、その約数のうち素数のものを数え上げるという方針は合っていたのだけど、TLE 対策で `sqrt(gcd(A, B))` までしかループを回さないようにした時に、`sqrt(gcd(A, B))` より大きい素数が1つだけ存在しうることを見落としたせいで、すべてがパーに. この問題を飛ばして、次の問題をやったら解けた可能性が高かったので、今回の ABC は完全に事故った感じに.

```python
from fractions import gcd
from math import sqrt


def prime_factorize(n):
    result = []
    for i in range(2, int(sqrt(n)) + 1):
        if n % i != 0:
            continue
        t = 0
        while n % i == 0:
            n //= i
            t += 1
        result.append((i, t))
        if n == 1:
            break
    if n != 1:
        result.append((n, 1))
    return result


A, B = map(int, input().split())
print(len(prime_factorize(gcd(A, B))) + 1)
```

## E - Get Everything

手を付けれず. ナップサック問題が解ける人なら大体解けるのではないか.

```python
def read_key():
    a, b = map(int, input().split())
    m = 0
    for c in map(int, input().split()):
        m |= 1 << (c - 1)
    return (a, m)


INF = float('inf')

N, M = map(int, input().split())
keys = [read_key() for _ in range(M)]

dp = [INF] * (1 << N)

dp[0] = 0
for i in range(M):
    a, m = keys[i]
    for j in range((1 << N) - 1, -1, -1):
        if dp[j] == INF:
            continue
        if dp[j] + a < dp[j | m]:
            dp[j | m] = dp[j] + a

if dp[(1 << N) - 1] == INF:
    print(-1)
else:
    print(dp[(1 << N) - 1])
```
