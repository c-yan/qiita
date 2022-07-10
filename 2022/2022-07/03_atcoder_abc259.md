# AtCoder Beginner Contest 259 参戦記

## [ABC259A - Growth Record](https://atcoder.jp/contests/abc259/tasks/abc259_a)

2分半で突破. 書くだけ.

```python
N, M, X, T, D = map(int, input().split())

print(T - (X - min(X, M)) * D)
```

## [ABC259B - Counterclockwise Rotation](https://atcoder.jp/contests/abc259/tasks/abc259_b)

19分で突破、WA1. 回転の公式を使えば一発.

```python
from math import radians, cos, sin

a, b, d = map(int, input().split())

theata = radians(d)
print(a * cos(theata) - b * sin(theata), a * sin(theata) + b * cos(theata))
```

角度を求めても良いけど、求めるときに acos, asin を使ってはダメ(WAの理由).


```python
from math import hypot, atan2, radians, cos, sin

a, b, d = map(int, input().split())

r = hypot(a, b)
theta = atan2(b, a) + radians(d)
print(cos(theta) * r, sin(theta) * r)
```

## [ABC259C - XX to XXX](https://atcoder.jp/contests/abc259/tasks/abc259_c)

9分半で突破. 色々考えると文字とその連続数の組のストリームに変換するのが楽だと分かる.

```python
def encode_runlength(s):
    result = []
    p = s[0]
    a = 1
    for c in s[1:]:
        if p == c:
            a += 1
            continue
        result.append((p, a))
        p = c
        a = 1
    result.append((p, a))
    return result

S = input()
T = input()

s = encode_runlength(S)
t = encode_runlength(T)

if len(s) != len(t):
    print('No')
    exit()

for (c0, a0), (c1, a1) in zip(s, t):
    if c0 != c1:
        print('No')
        exit()
    if a0 >= 2 and a1 > a0:
        a0 = a1
    if a0 != a1:
        print('No')
        exit()
print('Yes')
```

## [ABC259D - Circumferences](https://atcoder.jp/contests/abc259/tasks/abc259_d)

27分で突破. $N \le 3000$ なので全ての組み合わせでも $ {}_ {3000}\mathrm{C}_{2} \fallingdotseq 4.5 \times 10^6$ となりすべての組み合わせで交差判定することができる. 交差判定で得た接続情報は UnionFind で管理するのが一番楽. 後はググって交差判定のコードを書くだけ.

```python
from sys import setrecursionlimit, stdin


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


readline = stdin.readline
setrecursionlimit(10 ** 6)


def intersect(c1, c2):
    (x1, y1, r1), (x2, y2, r2) = c1, c2
    d = (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)
    return d >= (r1 - r2) * (r1 - r2) and d <= (r1 + r2) * (r1 + r2)


def on_circle(x1, y1, c):
    x2, y2, r = c
    return (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) == r * r


N = int(readline())
sx, sy, tx, ty = map(int, readline().split())
xyr = [list(map(int, readline().split())) for _ in range(N)]

parent = [-1] * (N + 1)

for i in range(N - 1):
    for j in range(i + 1, N):
        if intersect(xyr[i], xyr[j]):
            unite(parent, i, j)

for i in range(N):
    if on_circle(sx, sy, xyr[i]):
        a = i
    if on_circle(tx, ty, xyr[i]):
        b = i

if find(parent, a) == find(parent, b):
    print('Yes')
else:
    print('No')
```

## [ABC259E - LCM on Whiteboard](https://atcoder.jp/contests/abc259/tasks/abc259_e)

31分で突破、WA1. 最小公倍数が変化するのはある $p$ に対して $a_i$ が最大の $e$ を単独で持っているときだけ. なので $p$ 毎に最大の $e$ がいくつで、それを持っている $a_i$ が一つだけなのかの情報を収集すれば、その $a_i$ を1にしたときに最小公倍数が変化するのかが分かる. 変化した場合、単独で持っているという条件より、同じ最小公倍数に変化することは、他の $a_i$ を1にしても発生しないため、1にすると最小公倍数が変化する $a_i$ の数を求めると答えが得られる.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())

a = []
maxe = {}
unique = {}

for _ in range(N):
    m = int(readline())
    d = {}
    for _ in range(m):
        p, e = map(int, readline().split())
        d[p] = e
        maxe.setdefault(p, 0)
        if e > maxe[p]:
            maxe[p] = e
            unique[p] = True
        elif e == maxe[p]:
            unique[p] = False
    a.append(d)

changes = 0
for d in a:
    for p in d:
        if d[p] == maxe[p] and unique[p]:
            changes += 1
            break
nochanges = N - changes
print(changes + min(1, nochanges))
```
