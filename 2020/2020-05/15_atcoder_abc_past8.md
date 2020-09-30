# AtCoder Beginners Contest 過去問チャレンジ8

## [ABC134E - Sequence Decomposing](https://atcoder.jp/contests/abc134/tasks/abc134_e)

左から順に色を塗っていく. 同じ色で塗られているならば A<sub>i</sub>&lt;A<sub>j</sub> を満たすために、色数を増やさないならば現在塗ろうとしているとこより左に A<sub>j</sub> より小さい A<sub>i</sub> がある必要がある. なければ新色を足すしか無い. また、A<sub>j</sub> より小さい A<sub>i</sub> が複数ある場合にはその最大のものを使う必要がある.

ぱっと考えると平衡二分探索木があれば一発である. 新しく色を塗ろうとしている A の数より小さい最大の値を検索し、存在すればそれを削除して A を挿入する. 存在しなければそのまま A を挿入する. 最後まで処理した後に、平衡二分木の要素の数が答えとなっている. *O*(<i>N</i>log<i>N</i>) で処理できる. 残念ながら Python には組み込みの平衡二分探索木がないので Treap を実装した.

```python
from random import random
from sys import setrecursionlimit

setrecursionlimit(10 ** 5)


class TreapNode:
    _value = None
    _priority = None
    _count = None
    _left = None
    _right = None

    def __init__(self, value):
        self._value = value
        self._priority = random()
        self._count = 1


def treap_rotate_right(n):
    l = n._left
    n._left = l._right
    l._right = n
    return l


def treap_rotate_left(n):
    r = n._right
    n._right = r._left
    r._left = n
    return r


def treap_insert(n, v):
    if n is None:
        return TreapNode(v)
    if n._value == v:
        n._count += 1
        return n
    if n._value > v:
        n._left = treap_insert(n._left, v)
        if n._priority > n._left._priority:
            n = treap_rotate_right(n)
    else:
        n._right = treap_insert(n._right, v)
        if n._priority > n._right._priority:
            n = treap_rotate_left(n)
    return n


def treap_delete(n, v):
    if n is None:
        return None
    if n._value > v:
        n._left = treap_delete(n._left, v)
        return n
    if n._value < v:
        n._right = treap_delete(n._right, v)
        return n

    # n._value == v
    if n._count > 1:
        n._count -= 1
        return n

    if n._left is None and n._right is None:
        return None

    if n._left is None:
        n = treap_rotate_left(n)
    elif n._right is None:
        n = treap_rotate_right(n)
    else:
        # n._left is not None and n._right is not None
        if n._left._priority < n._right._priority:
            n = treap_rotate_right(n)
        else:
            n = treap_rotate_left(n)
    return treap_delete(n, v)


def treap_size(n):
    if n is None:
        return 0
    return n._count + treap_size(n._left) + treap_size(n._right)


def treap_str(n):
    if n is None:
        return ""
    result = []
    if n._left is not None:
        result.append(treap_str(n._left))
    result.append("%d:%d" % (n._value, n._count))
    if n._right is not None:
        result.append(treap_str(n._right))
    return ' '.join(result)


def treap_search(n, v):
    # v 未満で最大のノードを検索する. v 未満のノードがなければ None を返す
    if n is None:
        return None
    if n._value >= v:
        if n._left is None:
            return None
        return treap_search(n._left, v)
    # n._value < v
    if n._right is None:
        return n
    r = treap_search(n._right, v)
    if r is None:
        return n
    return r


class Treap:
    _root = None

    def insert(self, v):
        self._root = treap_insert(self._root, v)

    def delete(self, v):
        self._root = treap_delete(self._root, v)

    def __len__(self):
        return treap_size(self._root)

    def __str__(self):
        return treap_str(self._root)

    def search(self, v):
        return treap_search(self._root, v)


N = int(input())
A = [int(input()) for _ in range(N)]

t = Treap()
for a in A:
    n = t.search(a)
    if n is not None:
        t.delete(n._value)
    t.insert(a)
print(len(t))
```

実は平衡二分探索木がなくても解ける. 過去の A をソート済リストとして、A 以下の最大の数を二分探索で検索し、存在したらそれを A で上書きしたとしても、右の数と同じになることはあっても越えることはないのでソート状態は保たれる. 見つからなかった場合は左端に追加することになるので deque が必要となる.

```python
from collections import deque
from bisect import bisect_left

N = int(input())
A = [int(input()) for _ in range(N)]

t = deque([A[0]])
for a in A[1:]:
    if a <= t[0]:
        t.appendleft(a)
    else:
        t[bisect_left(t, a) - 1] = a
print(len(t))
```

降順にソートしていれば、追加が右端になるので deque が不要になる. その代わり bisect_left が使えなくなるが、まあ特に問題はないでしょう.

```python
N = int(input())
A = [int(input()) for _ in range(N)]

t = [A[0]]
for a in A[1:]:
    if t[-1] >= a:
        t.append(a)
        continue
    ok = len(t) - 1
    ng = -1
    while ok - ng > 1:
        m = (ok + ng) // 2
        if t[m] < a:
            ok = m
        else:
            ng = m
    t[ok] = a
print(len(t))
```

deque は使わず、bisect は使いたいという場合は、heapq を使うときによくやるマイナスにして値を突っ込む作戦で上手くいきます.

```python
from bisect import bisect_right

N = int(input())
A = [int(input()) for _ in range(N)]

t = [-A[0]]
for a in A[1:]:
    if a <= -t[-1]:
        t.append(-a)
    else:
        t[bisect_right(t, -a)] = -a
print(len(t))
```

## [ABC089D - Practical Skill Test](https://atcoder.jp/contests/abc089/tasks/abc089_d)

ナイーブに書くと *O*(*QHW*/*D*) となり、最大 *O*(9×10<sup>9</sup>) なので無理. D個累積和を作れば *O*(*Q*) になるので通る.

```python
H, W, D = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(H)]

d = {}
for y in range(H):
    for x in range(W):
        d[A[y][x]] = (y, x)

t = [[0] * (H * W // D + 1) for _ in range(D)]
for i in range(D):
    j = 0
    if i == 0:
        j = 1
    while i + (j + 1) * D <= H * W:
        y1, x1 = d[i + j * D]
        y2, x2 = d[i + (j + 1) * D]
        t[i][j + 1] = abs(y1 - y2) + abs(x1 - x2)
        j += 1
    for j in range(H * W // D):
        t[i][j + 1] += t[i][j]

Q = int(input())
result = []
for _ in range(Q):
    L, R = map(int, input().split())
    i = L % D
    result.append(t[i][(R - i) // D] - t[i][(L - i) // D])
print('\n'.join(str(i) for i in result))
```

## [ABC073D - joisino's travel](https://atcoder.jp/contests/abc073/tasks/abc073_d)

各町の最短移動距離をワーシャルフロイド法で求める. 後は R≦8 なので r の全順列の移動距離を求めれば最短距離が求まる. 問題は N≦200 のワーシャルフロイド法は Python では TLE することだけですね. PyPy では通る.

```python
from itertools import permutations


def warshall_floyd(n, d):
    for i in range(n):
        for j in range(n):
            for k in range(n):
                d[j][k] = min(d[j][k], d[j][i] + d[i][k])


N, M, R = map(int, input().split())
r = list(map(int, input().split()))

d = [[1000000000] * (N + 1) for _ in range(N + 1)]
for _ in range(M):
    A, B, C = map(int, input().split())
    d[A][B] = C
    d[B][A] = C
warshall_floyd(N + 1, d)

result = 1000000000
for p in permutations(r):
    t = 0
    for i in range(R - 1):
        t += d[p[i]][p[i + 1]]
    result = min(result, t)
print(result)
```

ワーシャルフロイド法を scipy に丸投げしたら Python でも通った.

```python
from scipy.sparse.csgraph import csgraph_from_dense, floyd_warshall
from itertools import permutations

N, M, R = map(int, input().split())
r = list(map(int, input().split()))

g = [[0] * (N + 1) for _ in range(N + 1)]
for _ in range(M):
    A, B, C = map(int, input().split())
    g[A][B] = C
    g[B][A] = C
g = floyd_warshall(csgraph_from_dense(g))

result = 1000000000
for p in permutations(r):
    t = 0
    for i in range(R - 1):
        t += g[p[i]][p[i + 1]]
    result = min(result, t)
print(int(result))
```

## [ABC106D - AtCoder Express 2](https://atcoder.jp/contests/abc106/tasks/abc106_d)

2次元のリストを t 用意して、都市 L から都市 R に走る電車をインデックス t[L][R] に記録することにすると、走る区間が都市 p から 都市 q までに完全に含まれる電車の数は t[p][p] を左上として、t[q][q] を右下とした四角形の中の電車の合計となる. そのままだと1クエリの最大計算量が *O*(500<sup>2</sup>) になってしまうが、累積和をすれば *O*(500) となり、更に2次元の累積和をすれば *O*(1) になる. クエリの数が 10<sup>5</sup> なので、Python だと2次元の累積和をしないと苦しい. PyPy なら1次元の累積和でも通る.

```python
N, M, Q = map(int, input().split())

t = [[0] * (N + 1) for _ in range(N + 1)]
for _ in range(M):
    L, R = map(int, input().split())
    t[L][R] += 1

for i in range(N + 1):
    for j in range(N):
        t[i][j + 1] += t[i][j]

for i in range(N):
    for j in range(N + 1):
        t[i + 1][j] += t[i][j]

result = []
for _ in range(Q):
    p, q = map(int, input().split())
    result.append(t[q][q] + t[p - 1][p - 1] - t[p - 1][q] - t[q][p - 1])
print('\n'.join(str(v) for v in result))
```

## [ABC070D - Transit Tree Path](https://atcoder.jp/contests/abc070/tasks/abc070_d)

K を通るので、K から最短路を求めておいて、始点と K の距離と、終点と K の距離を足し合わせれば質問クエリに *O*(1) で回答できる. 重みがある最短経路問題だとダイクストラ法かなあと思って、まだ一度もダイクストラ法が要る問題を解いたことがなかったので、Wikipedia を眺めながら書いてみた.

```python
from heapq import heappop, heappush

N = int(input())
links = [[] for _ in range(N + 1)]
for _ in range(N - 1):
    a, b, c = map(int, input().split())
    links[a].append((b, c))
    links[b].append((a, c))

Q, K = map(int, input().split())

d = [float('inf')] * (N + 1)
d[K] = 0
#prev = [None] * (N + 1)
q = [(0, K)]
while q:
    _, u = heappop(q)
    for v, c in links[u]:
        alt = d[u] + c
        if d[v] > alt:
            d[v] = alt
            #prev[v] = u
            heappush(q, (alt, v))

result = []
for _ in range(Q):
    x, y = map(int, input().split())
    result.append(d[x] + d[y])
print('\n'.join(str(v) for v in result))
```

ところが、この問題だと幅優先探索の方が高速だし、なんと深さ優先探索にも速度で負けてしまった orz. わざわざ書いた意味.

```python
from collections import deque

N = int(input())
links = [[] for _ in range(N + 1)]
for _ in range(N - 1):
    a, b, c = map(int, input().split())
    links[a].append((b, c))
    links[b].append((a, c))

Q, K = map(int, input().split())

d = [float('inf')] * (N + 1)
d[K] = 0
q = deque([K])
while q:
    i = q.popleft()
    for j, c in links[i]:
        if d[i] + c < d[j]:
            d[j] = d[i] + c
            q.append(j)

result = []
for _ in range(Q):
    x, y = map(int, input().split())
    result.append(d[x] + d[y])
print('\n'.join(str(v) for v in result))
```

## [ABC095D - Static Sushi](https://atcoder.jp/contests/abc095/tasks/arc096_b)

時計回りと反時計回りで最大値求めればいいのかなと思ったが、D問題がそんなに簡単なわけがなく. 入力例2で時計回りに進んだ後に反時計回りに進むのを見てなるほどと. まあ、それでも累積和で全組み合わせができる制限なので、やっぱりD問題にしては簡単かなって.

```python
N, C = map(int, input().split())
x = [None] * N
v = [None] * N
for i in range(N):
    x[i], v[i] = map(int, input().split())

a0 = [None] * N
a = v[0] - x[0]
a0[0] = max(0, a)
for i in range(1, N):
    a += v[i] - (x[i] - x[i - 1])
    a0[i] = max(a, a0[i - 1])

a1 = [None] * N
a = v[N - 1] - (C - x[N - 1])
a1[0] = max(0, a)
for i in range(1, N):
    a += v[N - 1 - i] - ((C - x[N - 1 - i]) - (C - x[N - 1 - (i - 1)]))
    a1[i] = max(a, a1[i - 1])

result = max(a0[N - 1], a1[N - 1])
for i in range(N - 1):
    result = max(result, a0[i] - x[i] + a1[N - 1 - (i + 1)])
for i in range(N - 1):
    result = max(result, a1[i] - (C - x[N - 1 - i]) + a0[N - 1 - (i + 1)])
print(result)
```
