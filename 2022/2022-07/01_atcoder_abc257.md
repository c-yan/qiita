# 日鉄ソリューションズプログラミングコンテスト2022(AtCoder Beginner Contest 257) 参戦記

集中できない理由があったので unrated で参加.

## [ABC257A - A to Z String 2](https://atcoder.jp/contests/abc257/tasks/abc257_a)

書くだけ.

```python
N, X = map(int, input().split())

print(chr(ord('A') + (X - 1) // N))
```

## [ABC257B - 1D Pawn](https://atcoder.jp/contests/abc257/tasks/abc257_b)

素直にシミュレーションすればいい.

```python
N, K, Q = map(int, input().split())
A = list(map(lambda x: int(x) - 1, input().split()))
L = list(map(lambda x: int(x) - 1, input().split()))

cells = [None] * N
for i in range(K):
    cells[A[i]] = i

for l in L:
    t = -1
    for j in range(N):
        if cells[j] is None:
            continue
        t += 1
        if t == l:
            break
    if j != N - 1 and cells[j + 1] is None:
        cells[j + 1] = cells[j]
        cells[j] = None

result = [-1] * K
for i in range(N):
    if cells[i] is None:
        continue
    result[cells[i]] = i + 1
print(*result)
```

## [ABC257C - Robot Takahashi](https://atcoder.jp/contests/abc257/tasks/abc257_c)

予め体重をソートしておけば、指定の体重未満、以上の人数をにぶたんにより $O(\log N)$ で求めれるようになるので、$O(N \log N)$ で解けた.

```python
from bisect import bisect_left

N = int(input())
S = input()
W = list(map(int, input().split()))

a = sorted(W[i] for i in range(N) if S[i] == '0')
b = sorted(W[i] for i in range(N) if S[i] == '1')
c = set()
for x in W:
    c.add(x - 1)
    c.add(x)
    c.add(x + 1)

result = 0
for x in c:
    result = max(result, bisect_left(a, x) + len(b) - bisect_left(b, x))
print(result)
```

座標圧縮+累積和でも解けた.

```python
from itertools import accumulate

N = int(input())
S = input()
W = list(map(int, input().split()))

a = sorted(W)
d = {}
for i in range(len(a)):
    d[a[i]] = i + 1
W = [d[x] for x in W]
m = max(W)

b = [0] * (m + 2)
c = [0] * (m + 2)
for i in range(N):
    if S[i] == '0':
        b[W[i]] += 1
    else:
        c[W[i]] += 1
b = list(accumulate(b))
c = list(accumulate(c))
d = S.count('1')

result = 0
for i in range(m + 2):
    result = max(result, b[i] + (d - c[i]))
print(result)
```
