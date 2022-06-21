# 東京海上日動プログラミングコンテスト2022(AtCoder Beginner Contest 256) 参戦記

## [ABC256A - 2^N](https://atcoder.jp/contests/abc256/tasks/abc256_a)

1分で突破. 書くだけ.

```python
N = int(input())

print(2 ** N)
```

## [ABC256B - Batters](https://atcoder.jp/contests/abc256/tasks/abc256_b)

4分で突破. 素直にシミュレーションすればいい.

```python
N, *A = map(int, open(0).read().split())

P = 0
cells = [0] * 4
for a in A:
    cells[0] = 1
    for i in range(3, -1, -1):
        if cells[i] == 0:
            continue
        if i + a >= 4:
            P += 1
            cells[i] = 0
        else:
            cells[i + a] = 1
            cells[i] = 0
print(P)
```

ちょっと考えると最後の方のホームに戻れない人数だけ数えればいいと分かる.

```python
N, *A = map(int, open(0).read().split())

result = N
t = 0
for a in reversed(A):
    t += A
    if t >= 4:
        break
    result -= 1
print(result)
```

## [ABC256C - Filling 3x3 array](https://atcoder.jp/contests/abc256/tasks/abc256_c)

12分で突破. 左上4つだけ総当たりをするのは $30^4 = 8.1 \times 10^5$ なので性能的に可能だし、左上4つが決まれば他のマス目は自動的に決まるので解ける.

```python
h1, h2, h3, w1, w2, w3 = map(int, input().split())

result = 0
for v11 in range(1, h1 - 1):
    for v21 in range(1, h1 - v11):
        v31 = h1 - v11 - v21
        for v12 in range(1, h2 - 1):
            for v22 in range(1, h2 - v12):
                v32 = h2 - v12 - v22
                if v32 < 1:
                    continue
                v13 = w1 - v11 - v12
                if v13 < 1:
                    continue
                v23 = w2 - v21 - v22
                if v23 < 1:
                    continue
                v33 = w3 - v31 - v32
                if v33 < 1:
                    continue
                if v13 + v23 + v33 != h3:
                    continue
                result += 1
print(result)
```

## [ABC256D - Distinct Trio](https://atcoder.jp/contests/abc256/tasks/abc256_d)

19分で突破. 範囲加算セグ木があれば正面からシミュレーションしても大丈夫!

```python
from sys import stdin

readline = stdin.readline


class RangeAddSegmentTree:
    def __init__(self, size):
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [0] * (t * 2 - 1)

    def range_add(self, start, stop, x):
        data = self._data
        l = start + self._offset
        r = stop + self._offset
        while l < r:
            if l & 1 == 0:
                data[l] += x
            if r & 1 == 0:
                data[r - 1] += x
            l = l // 2
            r = (r - 1) // 2

    def __getitem__(self, key):
        data = self._data
        i = key + self._offset
        result = data[i]
        while i > 0:
            i = (i - 1) // 2
            result += data[i]
        return result

    def __iter__(self):
        for i in range(self._size):
            yield self[i]


N = int(readline())

st = RangeAddSegmentTree(2 * 10 ** 5 + 10)
for _ in range(N):
    L, R = map(int, readline().split())
    st.range_add(L, R, 1)
a = [x != 0 for x in st]

result = []
j = -1
while True:
    try:
        i = a.index(True, j + 1)
    except:
        break
    j = a.index(False, i + 1)
    result.append((i, j))
print(*("%d %d" % (x, y) for x, y in result), sep='\n')
```

どう考えても imos 法案件ですよね. 頭が終わっとる.

```python
from sys import stdin
from itertools import accumulate

readline = stdin.readline

N = int(readline())

t = [0] * (2 * 10 ** 5 + 1)
for _ in range(N):
    L, R = map(int, readline().split())
    t[L] += 1
    t[R] -= 1
a = [x != 0 for x in accumulate(t)]

result = []
j = -1
while True:
    try:
        i = a.index(True, j + 1)
    except:
        break
    j = a.index(False, i + 1)
    result.append((i, j))
print(*("%d %d" % (x, y) for x, y in result), sep='\n')
```

## [ABC256E - Takahashi's Anguish](https://atcoder.jp/contests/abc256/tasks/abc256_e)

56分で突破. 枝が一つだけなので、各グラフに閉路はたかだか一つしか存在せず、閉路で一番低いコストを選択すればいいのはなんとなく想像がついた. が、閉路のメンバーか、そうでないかをどう判断すればいいのかが分からなかった. 結構考えて、閉路検出されたノードは必ず閉路のメンバーなので、そこからもう一周すれば閉路のメンバーでないノードはスルーできると気づいた.

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


setrecursionlimit(10 ** 6)

N = int(input())
X = list(map(lambda x: int(x) - 1, input().split()))
C = list(map(int, input().split()))

parent = [-1] * N
for i in range(N):
    unite(parent, i, X[i])
roots = [i for i in range(N) if parent[i] < 0]

result = 0
for r in roots:
    used = {r}
    i = X[r]
    while i not in used:
        used.add(i)
        i = X[i]

    t = [C[i]]
    used = {i}
    i = X[i]
    while i not in used:
        t.append(C[i])
        used.add(i)
        i = X[i]
    result += min(t)
print(result)
```

コンテスト終了後にコストが大きい方から繋いでいって、閉路が発生したタイミングのコストを選択すればいいんだよって言われてなるほどになった.

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


setrecursionlimit(10 ** 6)

N = int(input())
X = list(map(lambda x: int(x) - 1, input().split()))
C = list(map(int, input().split()))

parent = [-1] * N
result = 0
for c, i in sorted(zip(C, range(N)), reverse=True):
    if find(parent, i) != find(parent, X[i]):
        unite(parent, i, X[i])
    else:
        result += c
print(result)
```
