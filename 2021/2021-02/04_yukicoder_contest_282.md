# yukicoder contest 282 参戦記

## [A 1389 Clumsy Calculation](https://yukicoder.me/problems/no/1389)

chinerist 君が間違わなかったとすると、S<sub>i</sub> の合計は X * N - (A<sub>1</sub> +  A<sub>2</sub> + ... + A<sub>N</sub>) = X * N - X = X * (N - 1) となる. 間違っているのが S<sub>j</sub> だとすると、S<sub>j</sub> = 2 * (X - A<sub>j</sub>) となり、実際の S<sub>i</sub> の合計は X - A<sub>j</sub> 増えた X * (N - 1) + (X - A<sub>j</sub>) となる. 求めるのは間違えた計算の正しい答え、つまり S<sub>j</sub> の正しい値 X - A<sub>j</sub> なので、実際の S<sub>i</sub> の合計から、正しい S<sub>i</sub> の合計 X * (N - 1) を引けば答えになる.

```python
N, X, *S = map(int, open(0).read().split())

print(sum(S) - X * (N - 1))
```

## [B 1390 Get together](https://yukicoder.me/problems/no/1390)

Union Find を使えば簡単. まず同じ箱にいるスライムを同じ集合に入れる. そして各色ごとに違う箱にいるスライムを統合していって、統合した回数が答え.

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

box = {}
color = {}
parent = [-1] * N
for i in range(N):
    b, c = map(int, readline().split())
    box.setdefault(b, [])
    box[b].append(i)
    color.setdefault(c, [])
    color[c].append(i)

for b in box.values():
    i = b[0]
    for j in b[1:]:
        unite(parent, i, j)

result = 0
for c in color.values():
    i = c[0]
    for j in c[1:]:
        if find(parent, i) == find(parent, j):
            continue
        result += 1
        unite(parent, i, j)
print(result)
```
