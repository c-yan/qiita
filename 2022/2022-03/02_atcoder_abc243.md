# AtCoder Beginner Contest 243 参戦記

終了1分前にE問題が通って、1回で水色に復帰.

## [ABC243A - Shampoo](https://atcoder.jp/contests/abc243/tasks/abc243_a)

4分で突破. 書くだけではあるけど、A問題にしてはむずい.

```python
V, A, B, C = map(int, input().split())

while True:
    V -= A
    if V < 0:
        print('F')
        break
    V -= B
    if V < 0:
        print('M')
        break
    V -= C
    if V < 0:
        print('T')
        break
```

## [ABC243B - Hit and Blow](https://atcoder.jp/contests/abc243/tasks/abc243_b)

4分で突破. 書くだけではあるけど、B問題にしてはむずい.

```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

c = 0
for i in range(N):
    if A[i] == B[i]:
        c += 1
print(c)

c = 0
for i in range(N):
    for j in range(N):
        if i == j:
            continue
        if A[i] == B[j]:
            c += 1
print(c)
```

## [ABC243C - Collision 2](https://atcoder.jp/contests/abc243/tasks/abc243_c)

11分で突破. あるYにおいてLの左側にRがいたらアウト.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
XY = [tuple(map(int, readline().split())) for _ in range(N)]
S = readline()[:-1]

L = {}
R = {}
for i in range(N):
    X, Y = XY[i]
    if S[i] == 'L':
        if Y in L:
            L[Y] = max(L[Y], X)
        else:
            L[Y] = X
    elif S[i] == 'R':
        if Y in R:
            R[Y] = min(R[Y], X)
        else:
            R[Y] = X

for k in L:
    if k not in R:
        continue
    if L[k] > R[k]:
        print('Yes')
        exit()
print('No')
```

## [ABC243D - Moves on Binary Tree](https://atcoder.jp/contests/abc243/tasks/abc243_d)

13分半で突破、TLE1. 上に行くと半分(端数切捨て)になって、左下に行くと倍になって、右下に行くと 2*n*+1 になることを確認した瞬間に Python はオーバーフロー無いしと出して無事死亡. 下行って上はキャンセルなので、それを再帰的に消せれば OK かなと思ってスタックで消したら通った.

```python
N, X = map(int, input().split())
S = input()

q = []
for c in S:
    if c == 'U' and len(q) != 0 and q[-1] != 'U':
        q.pop()
    else:
        q.append(c)

for c in q:
    if c == 'U':
        X >>= 1
    elif c == 'L':
        X <<= 1
    elif c == 'R':
        X <<= 1
        X += 1
print(X)
```

追記: 2進数の文字の列としてやると消さなくても行ける.

```python
N, X = map(int, input().split())
S = input()

a = list(bin(X))[2:]
for c in S:
    if c == 'U':
        a.pop()
    elif c == 'L':
        a.append('0')
    elif c == 'R':
        a.append('1')
print(int(''.join(a), 2))
```

## [ABC243E - Edge Deletion](https://atcoder.jp/contests/abc243/tasks/abc243_e)

65分半で突破、WA1. 他を経由するルートが直通以下の距離であればその辺は不要というポリシーで数えたら合っていたらしく通った.

```python
from sys import stdin

readline = stdin.readline

def warshall_floyd(n, d):
    for i in range(n):
        for j in range(n):
            for k in range(n):
                d[j][k] = min(d[j][k], d[j][i] + d[i][k])

INF = 10 ** 15

N, M = map(int, readline().split())
ABC = [tuple(map(int, readline().split())) for _ in range(M)]

d = [[INF] * (N + 1) for _ in range(N + 1)]
for k in range(N):
    d[k][k] = 0
for a, b, c in ABC:
    d[a][b] = c
    d[b][a] = c
warshall_floyd(N + 1, d)

result = 0
for a, b, c in ABC:
    for x in range(1, N + 1):
        if x in [a, b]:
            continue
        if c >= d[a][x] + d[x][b]:
            result += 1
            break
print(result)
```
