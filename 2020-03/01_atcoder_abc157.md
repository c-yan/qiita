# AtCoder Beginner Contest 157 参戦記

## ABC157A - Duplex Printing

1分で突破. 書くだけ.

```python
N = int(input())

print((N + 1) // 2)
```

## ABC157B - Bingo

7分半で突破. 書くだけ……ではあるけど、エレガントに書こうと思えばどう書けばいいんですかね……. 特にビンゴ判定のところ.

```python
A = [list(map(int, input().split())) for _ in range(3)]
N = int(input())

for _ in range(N):
    b = int(input())
    for y in range(3):
        for x in range(3):
            if A[y][x] == b:
                A[y][x] = -1

for y in range(3):
    f = True
    for x in range(3):
        if A[y][x] != -1:
            f = False
    if f:
        print('Yes')
        exit()

for x in range(3):
    f = True
    for y in range(3):
        if A[y][x] != -1:
            f = False
    if f:
        print('Yes')
        exit()

f = True
for x in range(3):
    if A[x][x] != -1:
        f = False
if f:
    print('Yes')
    exit()

f = True
for x in range(3):
    if A[2 - x][x] != -1:
        f = False
if f:
    print('Yes')
    exit()

print('No')
```

## ABC157C - Guess The Number

20分で突破. WA4. やー、これは酷い. N = 1 のときは先頭桁が0を取れることを見落としてボッコボコになった. それを除けば解くのは難しくなく、配列で決まった桁を管理し、決まらなかった桁には最小になる数字を割り付ければいいだけである.

```python
N, M = map(int, input().split())

t = [-1] * N
for _ in range(M):
    s, c = map(int, input().split())
    s -= 1
    if t[s] != -1 and t[s] != c:
        print(-1)
        exit()
    t[s] = c

if N != 1:
    if t[0] == 0:
        print(-1)
        exit()

    if t[0] == -1:
        t[0] = 1

    for i in range(1, N):
        if t[i] == -1:
            t[i] = 0
else:
    if t[0] == -1:
        t[0] = 0

print(''.join(map(str, t)))
```

## ABC157D - Friend Suggestions

25分くらいで突破? Eに寄り道したので正確には分からず. 友達候補は友達の友達クラスタの大きさから以下を引いたもの.

- 自分の数(= 1)
- 友達の数
- 友達の友達クラスタにいるブロックした人の数

友達の友達クラスタの大きさはサイズ付き UnionFind で簡単に求まる.

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


N, M, K = map(int, input().split())
AB = [list(map(int, input().split())) for _ in range(M)]
CD = [list(map(int, input().split())) for _ in range(K)]

parent = [-1] * N
friends = [[] for _ in range(N)]
for A, B in AB:
    unite(parent, A - 1, B - 1)
    friends[A-1].append(B-1)
    friends[B-1].append(A-1)
blocks = [[] for _ in range(N)]
for C, D in CD:
    blocks[C-1].append(D-1)
    blocks[D-1].append(C-1)

result = []
for i in range(N):
    p = find(parent, i)
    t = -parent[p] - 1
    t -= len(friends[i])
    for b in blocks[i]:
        if p == find(parent, b):
            t -= 1
    result.append(t)
print(*result)
```

## ABC157E - Simple String Queries

敗退. セグ木とsetで行けるかなあと思ったけど TLE. 遅延セグ木で行けるかなあと思って、ちゃんと調べてる暇がなかったので Dirty 管理すればいいのかなと適当に書いたけど、性能改善しても TLE 消えず. PyPy は更に性能改善してくれたけどやっぱり足りず.

追記: set をやめて bit で表現したらあっさり通った. 文字種が小文字だけなので 26bit あれば表現できる. なんでコンテスト中に閃かないかなあ(嘆息).

```python
class SegmentTree():
    _data = []
    _offset = 0
    _size = 0

    def __init__(self, size):
        _size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [0 for _ in range(t * 2 - 1)]

    def update_all(self, iterable):
        self._data[self._offset:self._offset+self._size] = [1 << (ord(c) - ord('a')) for c in iterable]
        for i in range(self._offset - 1, -1, -1):
            self._data[i] = self._data[i * 2 + 1] | self._data[i * 2 + 2]

    def update(self, index, value):
        i = self._offset + index
        self._data[i] = 1 << (ord(value) - ord('a'))
        while i >= 1:
            i = (i - 1) // 2
            self._data[i] = self._data[i * 2 + 1] | self._data[i * 2 + 2]

    def query(self, start, stop):
        result = 0
        l = start + self._offset
        r = stop + self._offset
        while l < r:
            if l & 1 == 0:
                result = result | self._data[l]
            if r & 1 == 0:
                result = result | self._data[r - 1]
            l = l // 2
            r = (r - 1) // 2
        return result


N = int(input())
S = input()

st = SegmentTree(N)
st.update_all(S)

Q = int(input())
for _ in range(Q):
    q = input().split()
    if q[0] == '1':
        i, c = q[1:]
        i = int(i) - 1
        st.update(i, c)
    elif q[0] == '2':
        l, r = map(int, q[1:])
        print(bin(st.query(l - 1, r)).count('1'))
```
