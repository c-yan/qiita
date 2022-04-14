# AtCoder Beginner Contest 245 参戦記

## [ABC245A - Good morning](https://atcoder.jp/contests/abc245/tasks/abc245_a)

3分で突破. 書くだけ.

```python
A, B, C, D = map(int, input().split())

if A * 3600 + B * 60 < C * 3600 + D * 60 + 1:
    print('Takahashi')
else:
    print('Aoki')
```

## [ABC245B - Mex](https://atcoder.jp/contests/abc245/tasks/abc245_b)

3分で突破. 書くだけ.

```python
N = int(input())
A = list(map(int, input().split()))

print(min(set(range(2002)) - set(A)))
```

## [ABC245C - Choose Elements](https://atcoder.jp/contests/abc245/tasks/abc245_c)

10分で突破、TLE1. メモ化せずに死んだ.

```python
from functools import lru_cache

from sys import setrecursionlimit

setrecursionlimit(10 ** 6)

N, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

@lru_cache(maxsize=None)
def f(n, i):
    if i == N:
        return True
    a = A[i]
    if abs(a - n) <= K:
        if f(a, i + 1):
            return True
    b = B[i]
    if abs(b - n) <= K:
        if f(b, i + 1):
            return True
    return False

if f(A[0], 1) or f(B[0], 1):
    print('Yes')
else:
    print('No')
```

が、こんな回りくどいことをする必要なかったね.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

a = True
b = True
for i in range(1, N):
    c, d = a, b
    a = (c and abs(A[i - 1] - A[i]) <= K) or (d and abs(B[i - 1] - A[i]) <= K)
    b = (c and abs(A[i - 1] - B[i]) <= K) or (d and abs(B[i - 1] - B[i]) <= K)

if a or b:
    print('Yes')
else:
    print('No')
```

## [ABC245D - Polynomial division](https://atcoder.jp/contests/abc245/tasks/abc245_d)

77分半で突破、RE4. A<sub>i</sub>=0 のパターンを考えていなくて0の除算を出しまくって死んだ.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))
C = list(map(int, input().split()))

B = [None] * (M + 1)


def f(b, c):
    x = C[c]
    if A[c - b] == 0:
        return None
    for i in range(min(c, N) + 1):
        if A[i] == 0:
            continue
        if c - i == b:
            continue
        if B[c - i] is None:
            return None
        x -= A[i] * B[c - i]
    return x // A[c - b]


for i in range(M + 1):
    for j in range(M + 1):
        if B[j] is not None:
            continue
        for k in range(j, N + M + 1):
            x = f(j, k)
            if x is not None:
                B[j] = x
                break
        if B[j] is not None:
            break
print(*B)
```
