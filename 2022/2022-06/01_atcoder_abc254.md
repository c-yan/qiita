# AtCoder Beginner Contest 254 参戦記

## [ABC254A - Last Two Digits](https://atcoder.jp/contests/abc254/tasks/abc254_a)

1分くらいで解いたけど遅刻したせいで2分半で突破. 書くだけ.

```python
N = input()

print(N[-2:])
```

## [ABC254B - Practical Computing](https://atcoder.jp/contests/abc254/tasks/abc254_b)

11分半で突破、TLE1. 見た瞬間にパスカルの三角形だと分かったのに、なぜこんなに時間がかかって TLE までしてしまうのか orz.

```python
from functools import lru_cache

N = int(input())


def print_pascal(d):
    for i in range(d):
        t = []
        for j in range(i + 1):
            t.append(choice(i, j))
        print(*t)


@lru_cache(maxsize=None)
def choice(n, r):
    if r < 0 or r > n:
        return 0
    elif r == 0:
        return 1
    else:
        return choice(n - 1, r - 1) + choice(n - 1, r)


print_pascal(N)
```

## [ABC254C - K Swap](https://atcoder.jp/contests/abc254/tasks/abc254_c)

14分半で突破. 実際にK個おきにソートして、比較すればいいだけ. なんでこんなに時間をかけてしまったんだ…….

```python
N, K, *a = map(int, open(0).read().split())

b = a[:]
for x in range(K):
    b[x::K] = sorted(b[x::K])

if sorted(a) == b:
    print('Yes')
else:
    print('No')
```
