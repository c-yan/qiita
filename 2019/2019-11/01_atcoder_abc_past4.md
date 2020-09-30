# AtCoder Beginners Contest 過去問チャレンジ4

## [ABC126D - Even Relation](https://atcoder.jp/contests/abc126/tasks/abc126_d)

探索ですべての頂点について頂点1からの距離を求め、偶数距離のものは0を、奇数距離のものは1を出力すればいい.

```python
N = int(input())

link = [[] for _ in range(N)]
for _ in range(N - 1):
    u, v, w = map(int, input().split())
    link[u - 1].append((v - 1, w))
    link[v - 1].append((u - 1, w))

d = [-1] * N
d[0] = 0
q = [0]
while q:
    p = q.pop()
    for n, w in link[p]:
        if d[n] == -1:
            d[n] = d[p] + w
            q.append(n)

for i in d:
    print(i % 2)
```

## [ABC126E - 1 or 2](https://atcoder.jp/contests/abc126/tasks/abc126_e)

「A<sub>X<sub>i</sub></sub>+A<sub>Y<sub>i</sub></sub>+Z<sub>i</sub> は偶数である。」ということは、片方のAが分かればもう片方も分かるということである. もし「A<sub>X</sub>+A<sub>Y</sub>+Z<sub>i</sub>は偶数である。」で、「A<sub>X</sub>+A<sub>Z</sub>+Z<sub>j</sub>は偶数である。」場合は、3つのAのうち1つが分かれば残り2つも分かる.

そうするとお互いに関係性を持たないAのグループがいくつあるかという問題になり、Union Find でグループ数を数えるだけの問題に帰着できる.

```python
from sys import setrecursionlimit


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


setrecursionlimit(10 ** 5)

N, M = map(int, input().split())

parent = [-1] * N
for _ in range(M):
    X, Y, Z = map(int, input().split())
    unite(parent, X - 1, Y - 1)
print(len([x for x in parent if x < 0]))
```

## [ABC052D - Walk and Teleport](https://atcoder.jp/contests/abc052/tasks/arc067_b)

順番に徒歩とテレポート、疲労度の上昇値が少ない方で訪れるだけ. 簡単.

```python
N, A, B = map(int, input().split())
X = list(map(int, input().split()))

result = 0
for i in range(N - 1):
    result += min(A * (X[i + 1] - X[i]), B)
print(result)
```

## [ABC079D - Wall](https://atcoder.jp/contests/abc079/tasks/abc079_d)

-1 と 1 以外のすべての数字について、1に変えるのに必要な魔力を加算していくだけなのだが、直接1に変えるよりも、別の数字を経由したほうが必要な魔力が少ない場合があるので、各数字から1に変える最小魔力をワーシャルフロイド法で求めておく.

```python
def warshall_floyd(n, d):
    for i in range(n):
        for j in range(n):
            for k in range(n):
                d[j][k] = min(d[j][k], d[j][i] + d[i][k])


H, W = map(int, input().split())

N = 10
d = [None] * N
for i in range(N):
    d[i] = list(map(int, input().split()))
warshall_floyd(N, d)

m = [d[i][1] for i in range(N)]

result = 0
for _ in range(H):
    for i in map(int, input().split()):
        if i == -1:
            continue
        result += m[i]
print(result)
```

## [ABC085D - Katana Thrower](https://atcoder.jp/contests/abc085/tasks/abc085_d)

振るのは最大ダメージの刀のみ. この振るダメージより投げつけるダメージが低い刀は不要. 投げつけるダメージが高い方から順に投げていって、振るの最大ダメージに辿り着く前にダメージが H を超えた場合は振る必要なし. 足りなければその分だけ振る. 振る刀が投げつける刀に入っていた場合は、振った後投げつけたことにすれば結果として回数は同じなので問題なし.

```python
N, H = map(int, input().split())
a, b = zip(*(map(int, input().split()) for _ in range(N)))

a_max = max(a)                   # 振るの最大ダメージ
b = [x for x in b if x > a_max]  # 振るの最大ダメージより、投げつけるダメージが低い刀は投げつける意味がないので除去
b.sort(reverse=True)

result = 0

# 投げつけるダメージが高い順に刀を投げていき、刀が尽きる前に倒せればそれで終了
for x in b:
    result += 1
    H -= x
    if H <= 0:
        break

# もし投げつけるだけで倒せなかった場合は、振るの最大ダメージの刀を振る
# 振るの最大ダメージの刀が投げつける刀に入っていた場合は、必要なだけ振った後に投げつけたことになる
if H > 0:
    result += (H + (a_max - 1)) // a_max

print(result)
```
