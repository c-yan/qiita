# AtCoder Beginner Contest 205 参戦記

## [ABC205A - kcal](https://atcoder.jp/contests/abc205/tasks/abc205_a)

2分で突破. 書くだけ.

```python
A, B = map(int, input().split())

print(A * B / 100)
```

## [ABC205B - Permutation Check](https://atcoder.jp/contests/abc205/tasks/abc205_b)

2分半で突破. 個数カウントしても良かったけど、まあいいかと一つづつチェックした.

```python
N, *A = map(int, open(0).read().split())

A.sort()
for i in range(N):
    if i == A[i] - 1:
        continue
    print('No')
    break
else:
    print('Yes')
```

set に押し込んで個数が減らないかチェックするのが一番楽そうだと今気づいた.

```python
N, *A = map(int, open(0).read().split())

if len(set(A)) == N:
    print('Yes')
else:
    print('No')
```

## [ABC205C - POW](https://atcoder.jp/contests/abc205/tasks/abc205_c)

14分で突破、WA1. やらかした. どちらも乗数は同じなので偶奇が変わらないように減らしてあげれば OK.

```python
A, B, C = map(int, input().split())

C = (C + 1) % 2 + 1

if pow(A, C) < pow(B, C):
    print('<')
elif pow(A, C) > pow(B, C):
    print('>')
elif pow(A, C) == pow(B, C):
    print('=')
```

## [ABC205D - Kth Excluded](https://atcoder.jp/contests/abc205/tasks/abc205_d)

30分で突破. A<sub>i</sub> が一番右端だと思うと、A<sub>i</sub>+1 は A<sub>i</sub>-i+1 番目の数になる. 実際には右端とは限らないので、Kを超えない A<sub>i</sub> を二分探索で探してそこをベースに計算すれば良い.

```python
from sys import stdin

readline = stdin.readline

N, Q = map(int, readline().split())
A = list(map(int, readline().split()))

result = []
for _ in range(Q):
    K = int(readline())
    ok = 0
    ng = N + 1
    while ng - ok > 1:
        m = ok + (ng - ok) // 2
        if A[m - 1] - m < K:
            ok = m
        else:
            ng = m
    result.append(K + ok)
print(*result, sep='\n')
```
