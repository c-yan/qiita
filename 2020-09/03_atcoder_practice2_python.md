# AtCoder Library Practice Contest 参戦記 (Python)

ACL Contest は 1200 から rated なので rated で出場できるかどうしようと考えつつ、AtCoder Library Practice Contest を解いてみるテスト.

## [PRACTICE2A - Disjoint Set Union](https://atcoder.jp/contests/practice2/tasks/practice2_a)

ACL に Union Find 入ってないような、見落としてる???

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

N, Q = map(int, readline().split())

parent = [-1] * (N + 1)
result = []
for _ in range(Q):
    t, u, v = map(int, readline().split())
    if t == 0:
        unite(parent, u, v)
    elif t == 1:
        if find(parent, u) == find(parent, v):
            result.append(1)
        else:
            result.append(0)

print(*result, sep='\n')
```

## [PRACTICE2B - Fenwick Tree](https://atcoder.jp/contests/practice2/tasks/practice2_b)

結構前に書いたのにまだ2度目の登板.

```python
from sys import setrecursionlimit, stdin


def bit_add(bit, i, x):
    i += 1
    n = len(bit)
    while i <= n:
        bit[i - 1] += x
        i += i & -i


def bit_sum(bit, i):
    result = 0
    i += 1
    while i > 0:
        result += bit[i - 1]
        i -= i & -i
    return result


def query(bit, start, stop):
    if start == 0:
        return bit_sum(bit, stop - 1)
    else:
        return bit_sum(bit, stop - 1) - bit_sum(bit, start - 1)


readline = stdin.readline
setrecursionlimit(10 ** 6)

N, Q = map(int, readline().split())
a = list(map(int, readline().split()))

bit = [0] * N
for i in range(N):
    bit_add(bit, i, a[i])

result = []
for _ in range(Q):
    Query = readline()
    if Query[0] == '0':
        _, p, x = map(int, Query.split())
        bit_add(bit, p, x)
    elif Query[0] == '1':
        _, l, r = map(int, Query.split())
        result.append(query(bit, l, r))

print(*result, sep='\n')
```
