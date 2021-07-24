# AtCoder Beginner Contest 211 参戦記

C問題、さっぱり分からないのにものすごい勢いで解かれていて、競プロ典型90問かなと思ったら、本当にそうだった. やってないことが完全にディスアドバンテージになっている.

## [ABC211A - Blood Pressure](https://atcoder.jp/contests/abc211/tasks/abc211_a)

2分で突破. 書くだけ.

```python
A, B = map(int, input().split())

print((A - B) / 3 + B)
```

## [ABC211B - Cycle Hit](https://atcoder.jp/contests/abc211/tasks/abc211_b)

2分半で突破. 書くだけ.

```python
S = [input() for _ in range(4)]

if sorted(S) == ['2B', '3B', 'H', 'HR']:
    print('Yes')
else:
    print('No')
```

## [ABC211D - Number of Shortest paths](https://atcoder.jp/contests/abc211/tasks/abc211_d)

20分?で突破. 最短経路探索 BFS で経路数も記録するだけだよなあとササッと書いたらやっぱりそれで良かった.

```python
from sys import stdin
from collections import deque

readline = stdin.readline

m = 1000000007
INF = 10 ** 18

N, M = map(int, readline().split())

links = [[] for _ in range(N)]
for _ in range(M):
    A, B = map(int, readline().split())
    links[A - 1].append(B - 1)
    links[B - 1].append(A - 1)

costs = [INF] * N
counts = [0] * N

costs[0] = 0
counts[0] = 1
q = deque([0])
while q:
    i = q.popleft()
    c = costs[i]
    for j in links[i]:
        if c + 1 > costs[j]:
            continue
        elif c + 1 == costs[j]:
            counts[j] += counts[i]
            counts[j] %= m
        elif c + 1 < costs[j]:
            costs[j] = c + 1
            counts[j] = counts[i]
            q.append(j)
print(counts[N - 1])
```

## [ABC211C - chokudai](https://atcoder.jp/contests/abc211/tasks/abc211_c)

89分?で突破、TLE3. メモ化再帰で書いて、Go言語で書き直してなどして3回 TLE を食らってから真面目に考えて *O*(*N*<sup>2</sup>) になっとるやんと気づく体たらく. ある位置のある文字の通り数は、その位置までにある一つ前の文字の通り数の合計なので、1文字づつ求めて累積和をすれば *O*(*N*) になった.

```python
from itertools import accumulate

m = 1000000007

S = input()

n = len(S)
a = [1] * n
for c in 'chokudai':
    b = [0] * n
    for i in range(n):
        if S[i] != c:
            continue
        b[i] = a[i] % m
    a = list(accumulate(b))
print(a[-1] % m)
```
