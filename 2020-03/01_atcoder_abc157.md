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

20分で突破. WA4. やー、これは酷い. N = 1 のときは先頭桁が0を取れることを見落としてボッコボコになった. それを除けば解くのは難しくなく、配列で決まった桁を管理し、決まらなかった桁には最小になる数字をり付ければいいだけである.

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

友達の友達クラスタの大きさはサイズ付き UnonFind で簡単に求まる.

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
