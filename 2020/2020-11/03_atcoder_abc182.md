# AtCoder Beginner Contest 182 参戦記

## [ABC182A - twiblr](https://atcoder.jp/contests/abc182/tasks/abc182_a)

1分で突破. 書くだけ.

```python
A, B = map(int, input().split())

print(2 * A + 100 - B)
```

## [ABC182B - Almost GCD](https://atcoder.jp/contests/abc182/tasks/abc182_b)

4分で突破. 少し悩んだが全部試せばいいことに気づいた.

```python
N, *A = map(int, open(0).read().split())

result = 0
for i in range(2, 1000):
    t = 0
    for a in A:
        if a % i == 0:
            t += 1
    result = max(result, t)
print(result)
```

## [ABC182C - To 3](https://atcoder.jp/contests/abc182/tasks/abc182_c)

6分で突破. 言うまでもないが、各桁の数の合計が3の倍数なら3の倍数. なので各桁の数の合計を3で割ったあまりを考えることになる. 余りが0であれば最初から3の倍数なので話はおしまい. 余りが1, 2の場合には1桁ないし、2桁消せば必ず3の倍数になるが、消せる桁数が足りているかをチェックする必要がある.

```python
N = input()

t = [int(c % 3) for c in N]
x = sum(t)

if x % 3 == 0:
    print(0)
elif x % 3 == 1:
    if 1 in t:
        print(1)
    else:
        print(2)
elif x % 3 == 2:
    if 2 in t:
        print(1)
    else:
        print(2)
```

## [ABC182D - Wandering](https://atcoder.jp/contests/abc182/tasks/abc182_d)

17分で突破. 各ラウンドの開始時点の座標はそれまでの累積和の合計であることはすぐ分かる. 各ラウンド毎に座標が最大となるのは、ラウンドの開始時点の座標にそれまでの累積和の最大値を足したものとなるので、それの最大を取れば良い.

```python
from itertools import accumulate

N, *A = map(int, open(0).read().split())

a = list(accumulate(A))

result = 0
c = 0
m = 0
for i in range(N):
    m = max(a[i], m)
    result = max(result, c + m)
    c += a[i]
print(result)
```

## [ABC182E - Akari](https://atcoder.jp/contests/abc182/tasks/abc182_e)

60分で突破. 難しく考えすぎた. 何も考えずに色塗りすればよかった…….

```python
from sys import stdin

readline = stdin.readline

H, W, N, M = map(int, readline().split())
AB = [tuple(map(lambda x: int(x) - 1, readline().split())) for _ in range(N)]
CD = [tuple(map(lambda x: int(x) - 1, readline().split())) for _ in range(M)]

t = [[0] * W for _ in range(H)]

ly = [[] for _ in range(H)]
lt = [[] for _ in range(W)]
for a, b in AB:
    ly[a].append(b)
    lt[b].append(a)
for i in range(H):
    ly[i].sort()
for i in range(W):
    lt[i].sort()

by = [[] for _ in range(H)]
bt = [[] for _ in range(W)]
for c, d in CD:
    by[c].append(d)
    bt[d].append(c)
for i in range(H):
    by[i].append(W)
    by[i].sort()
for i in range(W):
    bt[i].append(H)
    bt[i].sort()

result = 0
for h in range(H):
    lyh = ly[h]
    i = 0
    pd = -1
    for d in by[h]:
        j = i
        while j < len(lyh) and lyh[j] < d:
            j += 1
        if i != j:
            for w in range(pd + 1, d):
                t[h][w] = 1
            i = j
        pd = d
for w in range(W):
    ltw = lt[w]
    i = 0
    pc = -1
    for c in bt[w]:
        j = i
        while j < len(ltw) and ltw[j] < c:
            j += 1
        if i != j:
            for h in range(pc + 1, c):
                t[h][w] = 1
            i = j
        pc = c
print(sum(sum(x) for x in t))
```
