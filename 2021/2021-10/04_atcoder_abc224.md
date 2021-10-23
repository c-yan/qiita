# AtCoder Beginner Contest 224 参戦記

## [ABC224A - Tires](https://atcoder.jp/contests/abc224/tasks/abc224_a)

3分半で突破、WA1. どう書くのがエレガントでバグが少なかっただろうか.

```python
S = input()

a = S.rfind('er')
b = S.rfind('ist')

if a > b:
    print('er')
else:
    print('ist')
```

## [ABC224B - Mongeness](https://atcoder.jp/contests/abc224/tasks/abc224_b)

5分で突破. 書くだけ.

```python
H, W = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(H)]

for i1 in range(H):
    for i2 in range(i1 + 1, H):
        for j1 in range(W):
            for j2 in range(j1 + 1, W):
                if A[i1][j1] + A[i2][j2] > A[i2][j1] + A[i1][j2]:
                    print('No')
                    exit()
print('Yes')
```

## [ABC224C - Triangle?](https://atcoder.jp/contests/abc224/tasks/abc224_c)

21分半で突破、TLE1. 三角形の面積の公式ググって書くだけ……なんだが、PyPy で提出しないと TLE になると思いながら書いてたのに、提出するときにそのまま Python で出したアホ.

```python
from itertools import combinations

N = int(input())
XY = [tuple(map(int, input().split())) for _ in range(N)]

result = 0
for i, j, k in combinations(range(N), 3):
    x0, y0 = XY[i]
    x1, y1 = XY[j]
    x2, y2 = XY[k]
    x1, x2 = x1 - x0, x2 - x0
    y1, y2 = y1 - y0, y2 - y0
    if x1 * y2 - x2 * y1 == 0:
        continue
    result += 1
print(result)
```

## [ABC224E - Integers on Grid](https://atcoder.jp/contests/abc224/tasks/abc224_e)

惜しくも突破できず. aが大きい方から順にやっていけばいいのだが、aが同じものがあり、更新を即座に反映するとバグるのでキューイングをしたのだが、キューをクリアするのを忘れてTLEしていた. アホすぎて泣ける.

それぞれの行と列で一番大きいときには移動できないので、各行と列の最大値テーブルを作る. aが大きい方から順に処理する. 移動可能回数は、それが所属する行と列におけるそれまでに一番移動できたやつ+1となるので、各行と列の過去に一番移動できた回数のテーブルを作る. あとはそれを更新しながら進めればメインの処理は *O*(*N*) で処理できる. オーダーは a が大きい順にソートするため *O*(log<i>N</i>) となる.

```python
from sys import stdin

readline = stdin.readline

H, W, N = map(int, readline().split())
rca = [tuple(map(int, readline().split())) for _ in range(N)]

my = [-1 for _ in range(H)]
mx = [-1 for _ in range(W)]
for r, c, a in rca:
    r, c = r - 1, c - 1
    my[r] = max(my[r], a)
    mx[c] = max(mx[c], a)

dpy = [0] * H
dpx = [0] * W
result = [0] * N
prev = -1
qy = []
qx = []
for i in sorted(range(N), key=lambda x: rca[x][2], reverse=True):
    r, c, a = rca[i][0] - 1, rca[i][1] - 1, rca[i][2]
    if prev != a:
        for y, t in qy:
            dpy[y] = max(dpy[y], t)
        for x, t in qx:
            dpx[x] = max(dpx[x], t)
        qy.clear()
        qx.clear()
    t = 0
    if a < my[r]:
        t = dpy[r] + 1
    if a < mx[c]:
        t = max(dpx[c] + 1, t)
    result[i] = t
    if t != 0:
        qy.append((r, t))
        qx.append((c, t))
    prev = a
print(*result, sep='\n')
```
