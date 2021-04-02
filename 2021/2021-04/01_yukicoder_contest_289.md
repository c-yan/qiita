# yukicoder contest 289 (Kanten4205’s Birthday Contest) 参戦記

yukicoder なのに ABC みたいな難易度.

## [A 1461 しあはせや歩みゆく？](https://yukicoder.me/problems/no/1461)

N≦10<sup>15</sup> なので素直にシミュレートすると TLE するのは明らか. 10歩前くらいまで一気に飛ばして、そこからシミュレートすることによって AC できる.

```python
N = int(input())

a = max(N - 10, 0)

result = a * 5
p = a
while p != N:
    if result % 5 in [0, 1, 2]:
        p += 1
    else:
        p -= 1
    result += 1
print(result)
```

## [B 1462 最弱WEAKER](https://yukicoder.me/problems/no/1462)

素直にメモ化再帰で解くコードを書いたら TLE や MLE になって散々な目にあったので、規則性を見出そうと前から100個解かせてみたら偶奇だけだとわかった.

```python
N = int(input())

if N % 2 == 0:
    print('YES')
else:
    print('NO')
```

## [C 1463 Hungry Kanten](https://yukicoder.me/problems/no/1463)

N≦18なので bit 全探索で一発.

```python
from itertools import product
from functools import reduce

N, K, *A = map(int, open(0).read().split())

s = set()
for p in product([True, False], repeat=N):
    if list(p).count(True) < K:
        continue
    s.add(sum(A[i] for i in range(N) if p[i]))
    s.add(reduce(lambda x, y: x * y, (A[i] for i in range(N) if p[i])))
print(len(s))
```

## [D 1464 Number Conversion](https://yukicoder.me/problems/no/1464)

decimal を使えば小数点以下を正確に扱えるので、小数点以下がなくなるまで分母と分子を10倍し、最後に最大公約数で割ればよい.

```python
from decimal import Decimal
from math import gcd

X = Decimal(input())

r = 1
while X - int(X) != 0:
    X *= 10
    r *= 10

X =int(X)
n = gcd(X, r)
print('%d/%d' % (X // n, r // n))
```

## [E 1465 Archaea](https://yukicoder.me/problems/no/1465)

これはどこかで見たような問題. ゴールから逆向きにメモ化再帰をすれば解けます.

```python
from functools import lru_cache
from sys import setrecursionlimit

setrecursionlimit(10 ** 6)

N, K = map(int, input().split())


@lru_cache(maxsize=None)
def f(n, k):
    if k > K:
        return False
    if n == 1:
        return True
    if n % 2 == 0:
        result = f(n // 2, k + 1)
        if result:
            return True
    if n > 3:
        return f(n - 3, k + 1)
    return False


if f(N, 0):
    print('YES')
else:
    print('NO')
```
