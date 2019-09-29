# AtCoder Beginners Contest 過去問チャレンジ3

## ABC120D - Decayed Bridges

木のサイズを持つ Union Find を実装したのに、サイズを使ってないのが残念なので類題調査で見つけてやった. Union Find には unite はあっても separate はないので、逆順にして、くっつけた時に不便さがどれだけ減るかを計算することによって解いた.

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
AB = [[int(c) - 1 for c in input().split()] for _ in range(M)]

parent = [-1] * N
inconvenience = N * (N - 1) // 2
result = []
for a, b in AB[::-1]:
    result.append(inconvenience)
    pa, pb = find(parent, a), find(parent, b)
    if pa != pb:
        inconvenience -= parent[pa] * parent[pb]
    unite(parent, a, b)
print(*result[::-1])
```

## ABC125D - Flipping Signs

負の数が偶数だったら全消しできるので絶対値の総和になる. 負の数が奇数の場合には、絶対値が一番小さい値を負にすればいいので、絶対値の総和から一番絶対値が小さい値の絶対値×2を引けばいい.

```python
N = int(input())
A = list(map(int, input().split()))

negatives = len([a for a in A if a < 0])
if negatives % 2 == 0:
    m = 0
else:
    m = min(abs(a) for a in A)
print(sum(abs(a) for a in A) - 2 * m)
```

## ABC124D - Handstand

「逆立ちした人を連続で最大何人」なので、直立した人と逆立ちした人を同時にひっくり返すことには意味がなく、直立した人のブロックK個をひっくり返した 2K + 1 ブロックの最大長になる. 最初にブロックを連長カウントして、あとはしゃくとり法で最大長を求めることによって解いた.

```python
N, K = map(int, input().split())
S = list(map(int, input()))

j = 1
r = 0
t = []
for i in range(N):
    if S[i] == j:
        r += 1
    else:
        t.append(r)
        r = 1
        j ^= 1
t.append(r)
if j == 0:
    t.append(0)

if K >= (len(t) - 1) // 2:
    print(sum(t))
else:
    result = sum(t[:2 * K + 1])
    j = result
    i = 2
    while i + 2 * K < len(t):
        j += t[i + 2 * K - 1] + t[i + 2 * K] - (t[i - 2] + t[i - 1])
        result = max(result, j)
        i += 2
    print(result)
```

## ABC119D - Lazy Faith

ひたすら西に行くパターン、西に行って東に行くパターン、東に行って西に行くパターン、ひたすら東に行くパターンを、最初に神社に行ってから寺に行くパターン、最初に寺に行ってから神社に行くパターンについて、全パターン(つまり4×2で8パターン)計算し、最小値を答えとすることによって解いた. 位置は二分探索で見つける.

```python
from bisect import bisect_left

INF = float('inf')

def f(x, s, t):
    result = INF
    i = bisect_left(s, x)
    if i > 0:
        j = bisect_left(t, s[i - 1])
        if j > 0:
            result = min(result, (x - s[i - 1]) + (s[i - 1] - t[j - 1]))
        if j < len(t):
            result = min(result, (x - s[i - 1]) + (t[j] - s[i - 1]))
    if i < len(s):
        j = bisect_left(t, s[i])
        if j > 0:
            result = min(result, (s[i] - x) + (s[i] - t[j - 1]))
        if j < len(t):
            result = min(result, (s[i] - x) + (t[j] - s[i]))
    return result


A, B, Q = map(int, input().split())
s = [int(input()) for _ in range(A)]
t = [int(input()) for _ in range(B)]

for i in range(Q):
    x = int(input())
    print(min(f(x, s, t), f(x, t, s)))
```

## ABC123D - Cake 123

1, 2, 3 の美味しさをそれぞれソートして、まずそれぞれの一番美味しいのを取ったのを出力し、1だけ次に美味しいのに差し替えたもの、2だけ次に美味しいのに差し替えたもの、3だけ次に美味しいのに差し替えたもの3パターンを作って優先度付きキューに突っ込み、その3パターンで1番美味しいものを出力し、出力したものをベースにまた3パターンを作って優先度付きキューに突っ込みを繰り返すことによって解いた.

```python
from heapq import heappush, heappop

X, Y, Z, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))
C = list(map(int, input().split()))

for l in [A, B, C]:
    l.sort()

exists = set()
q = [(-(A[X - 1] + B[Y - 1] + C[Z - 1]), (X - 1, Y - 1, Z - 1))]
for _ in range(K):
    v, n = heappop(q)
    print(-v)
    for i in range(3):
        if n[i] > 0:
            t = [n[j] for j in range(3)]
            t[i] -= 1
            nn = tuple(t)
            if nn not in exists:
                exists.add(nn)
                nv = A[nn[0]] + B[nn[1]] + C[nn[2]]
                heappush(q, (-nv, nn))
```
