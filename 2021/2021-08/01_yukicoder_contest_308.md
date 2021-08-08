# yukicoder contest 308 参戦記

## [A 1636 森](https://yukicoder.me/problems/no/1636)

N頂点の木の辺の数は*N*-1なので、答えは(*N*-1)<sup>2</sup>です.

```python
N = int(input())

print((N - 1) * (N - 1))
```

## [B 1637 Easy Tree Query](https://yukicoder.me/problems/no/1637)

素直に部分木の頂点にコストを足してまわると *O*(*NQ*) で TLE です. 増えるコストは部分木の頂点数に*x*を掛けたものなので、最初に各部分木の頂点数を DFS で求めておけば *O*(1) でクエリに回答できます.

[ABC138D - Ki](https://atcoder.jp/contests/abc138/tasks/abc138_d)を思い出しましたが、こっちのほうがやや簡単かな.

```python
from sys import setrecursionlimit, stdin

readline = stdin.readline
setrecursionlimit(10 ** 6)

N, Q = map(int, readline().split())

links = [[] for _ in range(N)]
for _ in range(N - 1):
    a, b = map(int, readline().split())
    links[a - 1].append(b - 1)
    links[b - 1].append(a - 1)

visited = [False] * N
subnodes = [0] * N


def f(i):
    visited[i] = True
    result = 1
    for j in links[i]:
        if visited[j]:
            continue
        result += f(j)
    subnodes[i] = result
    return result


f(0)

result = []
t = 0
for _ in range(Q):
    p, x = map(int, readline().split())
    t += subnodes[p - 1] * x
    result.append(t)
print(*result, sep='\n')
```
