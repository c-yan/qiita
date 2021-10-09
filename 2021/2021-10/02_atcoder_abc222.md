# エクサウィザーズプログラミングコンテスト2021(AtCoder Beginner Contest 222) 参戦記

## [ABC222A - Four Digits](https://atcoder.jp/contests/abc222/tasks/abc222_a)

1分で突破. 書くだけ.

```python
N = int(input())

print('%04d' % N)
```

## [ABC222B - Failing Grade](https://atcoder.jp/contests/abc222/tasks/abc222_b)

2分で突破. 書くだけ.

```python
N, P, *a = map(int, open(0).read().split())

print(len([a for x in a if x < P]))
```

## [ABC222C - Swiss-System Tournament](https://atcoder.jp/contests/abc222/tasks/abc222_c)

16分半で突破. 素直にシミュレーションすればいいのだけど、それがそれなりに大変だという. 勝数は大きいほど上位で、番号は小さいほど上位と逆なので、勝数をマイナスにして処理した.

```python
N, M = map(int, input().split())
A = [input() for _ in range(2 * N)]

a = [(0, i) for i in range(2 * N)]
for i in range(M):
    t = []
    for j in range(N):
        c, d = a[j * 2]
        e, f = a[j * 2 + 1]
        c, e = -c, -e
        x = A[d][i]
        y = A[f][i]
        if x == y:
            t.append((-c, d))
            t.append((-e, f))
        elif (x == 'G' and y == 'C') or (x == 'C' and y == 'P') or (x == 'P' and y == 'G'):
            t.append((-(c + 1), d))
            t.append((-e, f))
        else:
            t.append((-c, d))
            t.append((-(e + 1), f))
    a = sorted(t)
print(*(i + 1 for _, i in a), sep='\n')
```

## [ABC222D - Online games](https://atcoder.jp/contests/abc222/tasks/abc222_d)

22分半で突破、TLE1. TLEするよねーと言いながら流してる辺りがダメ(笑). ナイーブに書くと *O*(*NM*<sup>2</sup>) なので見るからに駄目なのだが、毎回 imos すれば *O*(*NM*) ですんだ.

```python
from itertools import accumulate

m = 998244353

N = int(input())

a = list(map(int, input().split()))
b = list(map(int, input().split()))

dp = [0] * 3002
for i in range(a[0], b[0] + 1):
    dp[i] = 1

for i in range(1, N):
    t = [0] * 3002
    for j in range(a[i - 1], b[i - 1] + 1):
        t[max(j, a[i])] += dp[j]
        t[b[i] + 1] -= dp[j]
    dp = list(accumulate(x % m for x in t))
print(sum(dp) % m)
```
