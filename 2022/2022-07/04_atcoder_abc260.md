# AtCoder Beginner Contest 260 不参戦記

## [ABC260A - A Unique Letter](https://atcoder.jp/contests/abc260/tasks/abc260_a)

書くだけ.

```python
S = input()

d = {}
for c in S:
    d.setdefault(c, 0)
    d[c] += 1

for c in d:
    if d[c] == 1:
        print(c)
        exit()
else:
    print(-1)
```

## [ABC260B - Better Students Are Needed!](https://atcoder.jp/contests/abc260/tasks/abc260_b)

ソートして言われた順に取るだけではあるけど、ソートの応用編を知らないと大変そうではある.

```python
N, X, Y, Z = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

result = []

a = list(range(1, N + 1))

a.sort(key=lambda x: (A[x - 1], -x), reverse=True)
result.extend(a[:X])
a = a[X:]

a.sort(key=lambda x: (B[x - 1], -x), reverse=True)
result.extend(a[:Y])
a = a[Y:]

a.sort(key=lambda x: (A[x - 1] + B[x - 1], -x), reverse=True)
result.extend(a[:Z])

print(*sorted(result), sep='\n')
```

## [ABC260C - Changing Jewels](https://atcoder.jp/contests/abc260/tasks/abc260_c)

何も考えずにメモ化再帰するのが楽そう.

```python
from sys import setrecursionlimit
from functools import lru_cache

setrecursionlimit(10 ** 6)

N, X, Y = map(int, input().split())

@lru_cache(maxsize=None)
def red(level, count):
    if level == 1:
        return 0
    return (red(level - 1, 1) + blue(level, X)) * count

@lru_cache(maxsize=None)
def blue(level, count):
    if level == 1:
        return count
    return (red(level - 1, 1) + blue(level - 1, Y)) * count

print(red(N, 1))
```
