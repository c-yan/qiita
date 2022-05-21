# パナソニックグループプログラミングコンテスト2022 (AtCoder Beginner Contest 251) 参戦記

## [ABC251A - Six Characters](https://atcoder.jp/contests/abc251/tasks/abc251_a)

2分で突破. 書くだけ.

```python
S = input()

print((S * 6)[:6])
```

## [ABC251C - Poem Online Judge](https://atcoder.jp/contests/abc251/tasks/abc251_c)

10分?で突破. オリジナルでないものを弾いて、得点、提出順の2キーソートをして終わり. 2キーソートが無ければ、安定ソートで提出順、得点の順で2回ソートすればOK.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
a = []
s = set()
for i in range(N):
    S, T = readline().split()
    T = int(T)
    if S in s:
        continue
    s.add(S)
    a.append((T, -i, S))
a.sort(reverse=True)
print(-a[0][1] + 1)
```

## [ABC251B - At Most 3 (Judge ver.)](https://atcoder.jp/contests/abc251/tasks/abc251_b)

19分?で突破. B問題なのに分かんねーとなって、結局牛刀を用いた.

```python
from itertools import combinations

N, W, *A = map(int, open(0).read().split())

a = [x for x in A if x <= W]
s = set(a)

for x, y in combinations(A, 2):
    v = x + y
    if v > W:
        continue
    s.add(v)

for x, y, z in combinations(A, 3):
    v = x + y + z
    if v > W:
        continue
    s.add(v)

print(len(s))
```

そうか、3重ループで良かったのかー. 全探索はC問題だったはずなのに.

```python
N, W, *A = map(int, open(0).read().split())

t = A[:]

for x in range(N - 1):
    for y in range(x + 1, N):
        t.append(A[x] + A[y])

for x in range(N - 2):
    for y in range(x + 1, N - 1):
        for z in range(y + 1, N):
            t.append(A[x] + A[y] + A[z])

print(len(set(x for x in t if x <= W)))
```

## [ABC251E - Takahashi and Animals](https://atcoder.jp/contests/abc251/tasks/abc251_e)

30分?で突破. 2回連続で取らないということは許されないので、今回取るのが必須かどうかと、最後取るのが必須かどうかの4パターンについて最小値を DP すれば解ける.

```python
N, *A = map(int, open(0).read().split())

# last: must, this: must
a = 0
# last: optional, this: optional
b = A[0]
# last: must, this: optional
c = 10 ** 15
# last: optional, this: must
d = 10 ** 15
for i in range(1, N - 1):
    # from a
    nc = a + A[i]
    # from b
    nb = b + A[i]
    nd = b
    # from c
    na = c
    nc = min(nc, c + A[i])
    # from d
    nb = min(nb, d + A[i])
    a = na
    b = nb
    c = nc
    d = nd
a += A[N - 1]
c += A[N - 1]
d += A[N - 1]
print(min(a, b, c, d))
```

## [ABC251D - At Most 3 (Contestant ver.)](https://atcoder.jp/contests/abc251/tasks/abc251_d)

突破できず. 解説を読んで、だからおもりの個数が300個以下だったのかーってなった.

```python
W = int(input())

print(297)
print(*sum(([x, x * 100, x * 10000] for x in range(1, 100)), []))
```
