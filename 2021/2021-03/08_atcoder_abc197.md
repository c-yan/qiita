# AtCoder Beginner Contest 197 参戦記

## [ABC197A - Rotate](https://atcoder.jp/contests/abc197/tasks/abc197_a)

1分で突破. 書くだけ.

```python
S = input()

print(S[1:] + S[0])
```

## [ABC197B - Visibility](https://atcoder.jp/contests/abc197/tasks/abc197_b)

8分で突破. 書くだけなんだけど、X が y 座標なせいで無駄に時間を食った.

```python
H, W, X, Y = map(int, input().split())
S = [input() for _ in range(H)]

X, Y = X - 1, Y - 1

result = 0
x, y = Y, X
while y >= 0 and S[y][x] == '.':
    result += 1
    y -= 1
x, y = Y, X
while y < H and S[y][x] == '.':
    result += 1
    y += 1
x, y = Y, X
while x >= 0 and S[y][x] == '.':
    result += 1
    x -= 1
x, y = Y, X
while x < W and S[y][x] == '.':
    result += 1
    x += 1
result -= 3
print(result)
```

## [ABC197C - ORXOR](https://atcoder.jp/contests/abc197/tasks/abc197_c)

6分半で突破、TLE1. A<sub>i</sub> と A<sub>i+1</sub> の間に仕切りを入れるか、入れないかと考えるとビット全探索で解ける. 不用意に Python で提出して、提出した瞬間にヤバイかと思ったけど、実際 TLE になった. PyPy で提出し直し.

```python
from itertools import product
from functools import reduce

N, *A = map(int, open(0).read().split())

result = 10 ** 18
for p in product([True, False], N - 1)
    t = []
    x = 0
    for i in range(N):
        x |= A[i]
        if i == N - 1 or p[i]:
            t.append(x)
            x = 0
    result = min(result, reduce(lambda x, y: x ^y, t))
print(result)
```

## [ABC197D - Opposite](https://atcoder.jp/contests/abc197/tasks/abc197_d)

40分で突破. 数学なんて嫌いだ. まず入力の2点を使って中心の座標を求める. 中心と p0 を使って、中心から p0 への角度と距離を求める. 角度を 360°/ N 進めれば p1 の座標が求めれる. Python の atan が何故か -pi/2 ～ pi/2 が返ってくるという仕様でハマった. atan2 に差し替えて AC.

```python
from math import sqrt, pi, atan2, cos, sin

N = int(input())
x0, y0 = map(int, input().split())
xN2, yN2 = map(int, input().split())

x, y = (x0 + xN2) / 2, (y0 + yN2) / 2
r = sqrt((x0 - x) * (x0 - x) + (y0 - y) * (y0 - y))

t = atan2(y0 - y, x0 - x) + (2 * pi) / N
x1 = cos(t) * r + x
y1 = sin(t) * r + y
print(x1, y1)
```

## [ABC197E - Traveler](https://atcoder.jp/contests/abc197/tasks/abc197_e)

23分で突破. ボールの色が小さい順に回収していく. 現在座標がある色のボールの左端よりも左であれば右端のボールに行く、現在座標がある色のボールの右端よりも右であれば左端のボールに行く、中間にいれば左端に行った後右端に行くと、右端に行った後左端に行くの療法をやる. 各ラウンドごとの結果は常に1か2なので、色の種類数に対して *O*(*N*) で処理できる. 全体の計算量としてはソートしないといけないので *O*(log<i>N</i>). 最後に座標0に戻らないといけないことに注意. 負の座標から戻ることもあるので絶対値を足し込むこと.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())

d = {}
for _ in range(N):
    X, C = map(int, readline().split())
    d.setdefault(C, [])
    d[C].append(X)

for k in d:
    d[k].sort()

q = {0: 0}
for k in sorted(d):
    balls = d[k]
    nq = {}
    for x in q:
        t = q[x]
        if x <= balls[0]:
            a, b = x, t
            b += balls[-1] - a
            a = balls[-1]
            nq.setdefault(a, 10 ** 18)
            nq[a] = min(nq[a], b)
        elif x >= balls[-1]:
            a, b = x, t
            b += a - balls[0]
            a = balls[0]
            nq.setdefault(a, 10 ** 18)
            nq[a] = min(nq[a], b)
        else:
            a, b = x, t
            b += balls[-1] - a
            a = balls[-1]
            b += a - balls[0]
            a = balls[0]
            nq.setdefault(a, 10 ** 18)
            nq[a] = min(nq[a], b)

            a, b = x, t
            b += a - balls[0]
            a = balls[0]
            b += balls[-1] - a
            a = balls[-1]
            nq.setdefault(a, 10 ** 18)
            nq[a] = min(nq[a], b)
    q = nq
print(min(abs(k) + q[k] for k in q))
```
