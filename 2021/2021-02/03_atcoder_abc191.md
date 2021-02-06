# AtCoder Beginner Contest 191 参戦記

2度目のC問題が解けない自体に顔面蒼白になったが、D問題を解いたら700位台と蘇生し、E問題を解いたら300位台と過去最高の成績へと飛翔した.

## [ABC191A - Vanishing Pitch](https://atcoder.jp/contests/abc191/tasks/abc191_a)

2分半で突破. 書くだけだが、一応移項して整数しか出ないように計算した. さすがA問題だけあって、素直に割って浮動小数点数を混ぜても大丈夫なようだ(コンテスト後に試した).

```python
V, T, S, D = map(int, input().split())

if T * V <= D <= S * V:
    print('No')
else:
    print('Yes')
```

## [ABC191B - Remove It](https://atcoder.jp/contests/abc191/tasks/abc191_b)

1分で突破. ほんと Python だと書くだけで、Aより簡単.

```python
N, X, *A = map(int, open(0).read().split())

print(*[a for a in A if a != X])
```

## [ABC191C - Digital Graffiti](https://atcoder.jp/contests/abc191/tasks/abc191_c)

10x10 の荒い解像度で何をもって角と見なすんだよと頭真っ白. yukicoder みたいにノーペナなら、適当に書いて AC 状況から探るところだけど、AtCoder でこれはキツイ. 定義を書いておいて欲しさ. (それとも自分には意味のわからない自己交叉が分かれば自明なのだろうか……)

## [ABC191D - Circle Lattice Points](https://atcoder.jp/contests/abc191/tasks/abc191_d)

50分くらいで突破. WA1、TLE1. 10000倍で書いたら上手くいかなくて、浮動小数点数で大体の数字を出して、整数で厳密にチェックするちゃんぽんになった. 多分ちゃんぽんにする必要はないので後で書き直す.

```python
def main():
    from decimal import Decimal
    from math import floor, ceil, sqrt

    X, Y, R = map(Decimal, input().split())

    result = 0
    X10000 = int(X * 10000)
    Y10000 = int(Y * 10000)
    R10000 = int(R * 10000)
    X = float(X)
    Y = float(Y)
    R = float(R)

    RR = R10000 * R10000

    def check(x, y):
        x10000 = x * 10000
        y10000 = y * 10000
        return (x10000 - X10000) * (x10000 - X10000) + (y10000 - Y10000) * (y10000 - Y10000) <= RR

    for y in range(int(Y - R) - 10, int(Y + R) + 10 + 1):
        a = 1
        b = -2 * X
        c = X * X + (y - Y) * (y - Y) - R * R
        try:
            x0 = int((-b - sqrt(b * b - 4 * a * c)) / 2 * a)
            x1 = int((-b + sqrt(b * b - 4 * a * c)) / 2 * a)
        except:
            continue
        for x in range(x0 - 5, x0 + 5):
            if check(x, y):
                x0 = x
                break
        else:
            continue
        for x in range(x1 + 5, x1 - 5, -1):
            if check(x, y):
                x1 = x
                break
        else:
            continue
        result += (x1 - x0) + 1
    print(result)


main()
```

## [ABC191E - Come Back Quickly](https://atcoder.jp/contests/abc191/tasks/abc191_e)

38分で突破. 全てのノードを始点にしてN回ダイクストラを回して大丈夫か不安になりつつ書いてえいやで出したら通った.

```python
from heapq import heappop, heappush
from sys import stdin

readline = stdin.readline
INF = 10 ** 16

N, M = map(int, readline().split())
links = [{} for _ in range(N)]
for _ in range(M):
    A, B, C = map(int, readline().split())
    if B - 1 in links[A - 1]:
        links[A - 1][B - 1] = min(links[A - 1][B - 1], C)
    else:
        links[A - 1][B - 1] = C

dp = [[INF] * N for _ in range(N)]
for i in range(N):
    t = dp[i]
    q = [(0, i)]
    while q:
        c, j = heappop(q)
        link = links[j]
        if t[j] < c:
            continue
        for k in link:
            if t[k] > c + link[k]:
                t[k] = c + link[k]
                heappush(q, (c + link[k], k))

result = [dp[i][i] for i in range(N)]
for i in range(N):
    for j in range(N):
        if i == j:
            continue
        result[j] = min(result[j], dp[j][i] + dp[i][j])

for i in range(N):
    if result[i] == INF:
        result[i] = -1
print(*result, sep='\n')
```
