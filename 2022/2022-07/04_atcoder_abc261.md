# AtCoder Beginner Contest 261 不参戦記

## [ABC261A - Intersection](https://atcoder.jp/contests/abc261/tasks/abc261_a)

WA2. 2つの線の位置関係とか考えるより素直に色塗りするべきだった.

```python
L1, R1, L2, R2 = map(int, input().split())

a = [0] * 100
for x in range(L1, R1):
    a[x] += 1
for x in range(L2, R2):
    a[x] += 1
print(a.count(2))
```

## [ABC261B - Counterclockwise Rotation](https://atcoder.jp/contests/abc261/tasks/abc261_b)

書くだけ.

```python
N = int(input())
A = [input() for _ in range(N)]

for y in range(N):
    for x in range(N):
        if x == y:
            continue
        if A[y][x] == 'W' and A[x][y] != 'L':
            print('incorrect')
            exit()
        if A[y][x] == 'L' and A[x][y] != 'W':
            print('incorrect')
            exit()
        if A[y][x] == 'D' and A[x][y] != 'D':
            print('incorrect')
            exit()
print('correct')
```

## [ABC261C - NewFolder(1)](https://atcoder.jp/contests/abc261/tasks/abc261_c)

こういう現実っぽい問題良いですね.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())

result = []
d = {}
for _ in range(N):
    S = readline()[:-1]
    if S not in d:
        result.append(S)
        d[S] = 1
    else:
        result.append('%s(%d)' % (S, d[S]))
        d[S] += 1
print(*result, sep='\n')
```

## [ABC261D - Circumferences](https://atcoder.jp/contests/abc261/tasks/abc261_d)

どストレートな DP で特に難しいところはなかった.

```python
from sys import stdin

readline = stdin.readline

N, M = map(int, readline().split())
X = list(map(int, readline().split()))

a = [0] * (N + 1)
for _ in range(M):
    C, Y = map(int, readline().split())
    a[C] = Y

dp = [0] * (N + 1)
for i in range(N):
    t = [0] * (N + 1)
    for j in range(i + 1):
        t[j + 1] = max(t[j + 1], dp[j] + X[i] + a[j + 1])
        t[0] = max(t[0], dp[j])
    dp = t
print(max(dp))
```
