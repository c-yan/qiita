# AtCoder Beginner Contest 184 参戦記

ABC でC問題解けなかったの初めて…….

## [ABC184A - Determinant](https://atcoder.jp/contests/abc184/tasks/abc184_a)

1分で突破. 書くだけ.

```python
a, b = map(int, input().split())
c, d = map(int, input().split())

print(a * d - b * c)
```

## [ABC184B - Quizzes](https://atcoder.jp/contests/abc184/tasks/abc184_b)

3分で突破. 書くだけ.

```python
N, X = map(int, input().split())
S = input()

result = X
for c in S:
    if c == 'o':
        result += 1
    elif c == 'x':
        if result != 0:
            result -= 1
print(result)
```

## [ABC184C - Super Ryuma](https://atcoder.jp/contests/abc184/tasks/abc184_c)

突破できず. 頭真っ白になりました.

## [ABC184D - increment of coins](https://atcoder.jp/contests/abc184/tasks/abc184_d)

突破できず. 入出力例でも TLE したり、NaN になったりと上手く行かず.

追記: 確率DP. 金貨が X 枚、銀貨が Y 枚、銅貨が Z 枚になる確率を f(X, Y, Z) とすると、f(X, Y, Z) = (f(X - 1, Y, Z) * (X - 1) + f(X, Y - 1, Z) * (Y - 1) + f(X, Y, Z - 1) * (Z - 1)) / (X + Y + Z - 1) となる. 0≦X,Y,Z≦100 なので、全て求めてもたかだか *O*(10<sup>6</sup>) なので、全て求めて問題ない. 確率が全て求まれば 0≦X,Y≦99 に対して、f(100, X, Y)、f(X, 100, Y)、f(X, Y, Z) に操作回数である (100 + X + Y) - (A + B + C) を掛けたものを積算すれば期待値が求まる.

```python
A, B, C = map(int, input().split())

M = 101


def despread(x, y, z):
    return x * M * M + y * M + z


dp = [0] * (despread(100, 100, 100) + 1)
dp[despread(A, B, C)] = 1.0

for i in range(A, 100):
    for j in range(B, 100):
        for k in range(C, 100):
            if i + j + k == A + B + C:
                continue
            t = 0
            if i != 0:
                t += dp[despread(i - 1, j, k)] * (i - 1)
            if j != 0:
                t += dp[despread(i, j - 1, k)] * (j - 1)
            if k != 0:
                t += dp[despread(i, j, k - 1)] * (k - 1)
            dp[despread(i, j, k)] = t / (i + j + k - 1)

result = 0
D = A + B + C
for i in range(100):
    for j in range(100):
        t = 0
        t += dp[despread(99, i, j)]
        t += dp[despread(i, 99, j)]
        t += dp[despread(i, j, 99)]
        result += ((100 + i + j) - D) * t * 99 / (99 + i + j)
print(result)
```

## [ABC184E - Third Avenue](https://atcoder.jp/contests/abc184/tasks/abc184_e)

CとDを諦めてから始めたので何分かかったのか不明. 56分未満なのは確か. 見るからに難しそうなところが何もなく、何か罠があるのかと思いながら実装したけど、何も罠はなかった(笑). ワープだけは繰り返しトライするとヤバそうだったので、一回しかトライしないようにしたけど、これは見え見えすぎて罠ではないな(笑).

```python
from collections import deque

INF = 10 ** 9

H, W = map(int, input().split())
a = [input() for _ in range(H)]

d = {}
for h in range(H):
    for w in range(W):
        c = a[h][w]
        if c in 'SG':
            d[c] = (h, w)
        elif c in '.#':
            continue
        else:
            if c in d:
                d[c].append((h, w))
            else:
                d[c] = [(h, w)]

not_warped = {}
for c in 'abcdefghijklmnopqrstuvwxyz':
    not_warped[c] = True


def move(h, w, p):
    c = a[h][w]
    if c == '#':
        return
    if t[h][w] > p:
        t[h][w] = p
        q.append((h, w))


t = [[INF] * W for _ in range(H)]
h, w = d['S']
t[h][w] = 0
q = deque([(h, w)])
while q:
    h, w = q.popleft()
    c = a[h][w]
    p = t[h][w] + 1
    if 'a' <= c <= 'z' and not_warped[c]:
        for nh, nw in d[c]:
            if t[nh][nw] > p:
                t[nh][nw] = p
                q.append((nh, nw))
        not_warped[c] = False
    if h != 0:
        move(h - 1, w, p)
    if h != H - 1:
        move(h + 1, w, p)
    if w != 0:
        move(h, w - 1, p)
    if w != W - 1:
        move(h, w + 1, p)

h, w = d['G']
if t[h][w] == INF:
    print(-1)
else:
    print(t[h][w])
```

## [ABC184F - Programming Contest](https://atcoder.jp/contests/abc184/tasks/abc184_f)

突破できず. 半分全列挙を知っていればそれほど難しくもなかった.

```python
from bisect import bisect_left

N, T, *A = map(int, open(0).read().split())

s0 = {0}
for a in A[:N // 2]:
    for x in s0.copy():
        if x + a > T:
            continue
        s0.add(x + a)

s1 = {0}
for a in A[N // 2:]:
    for x in s1.copy():
        if x + a > T:
            continue
        s1.add(x + a)

ys = sorted(s1)
result = 0
for x in s0:
    i = bisect_left(ys, T - x + 1) - 1
    result = max(result, x + ys[i])
print(result)
```
