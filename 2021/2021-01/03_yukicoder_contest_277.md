# yukicoder contest 277 参戦記

## [A 1329 Square Sqsq](https://yukicoder.me/problems/no/1329)

Python だったら考察しなくても正面から踏み抜けるだろうと踏んで流したら、やっぱり通った(笑). Python を相手にするには 10<sup>10<sup>7</sup></sup> くらい必要なようである.

```python
from math import isqrt

N = int(input())

print(len(str(isqrt(N))))
```

## [B 1330 Multiply or Divide](https://yukicoder.me/problems/no/1330)

コンテスト中は AC したものの、コンテスト後の追加テストによるリジャッジで落とされてしまった.

ゴールインするときは A<sub>i</sub> の最大値を使えばいいことはすぐ分かる. それ以外については一番効率の良い A<sub>i</sub> を使えば基本的にいいのだが、ギリギリ滑り込みできるちょっと効率は悪いけど操作回数が少ない A<sub>i</sub> を使ったほうが操作回数が最小になることがあるので Greedy だと誤答になってしまう. 追加されたテストがそのパターンであり、コンテスト中は Greedy でも AC 出来たというわけである.

実装は同じ操作回数でそれよりも増加量が多いものがある A<sub>i</sub> を捨てたり、優先度付きキューを使ってダイクストラ法風に最小値を求めていくようにした. どこまで効果があったかは不明だが、Writer 解の倍速かったようである.

```python
from heapq import heappop, heappush

N, M, P, *A = map(int, open(0).read().split())


def f(x):
    c = 0
    while x % P == 0:
        c += 1
        x //= P
    return (x, c)


max_A = max(A)

if max_A > M:
    print(1)
    exit()

b = {}
for m, c in map(f, A):
    b.setdefault(c, 0)
    b[c] = max(b[c], m)
t = 1
for k in sorted(b):
    if b[k] <= t:
        del b[k]
    else:
        t = b[k]

if len(b) == 0:
    print(-1)
    exit()

q = [(0, 1)]
max_x = {}
max_x[0] = 1
while q:
    c, x = heappop(q)
    if max_x[c] > x:
        continue
    if x * max_A > M:
        print(c + 1)
        break
    for k in b:
        nc = c + 1 + k
        nx = x * b[k]
        max_x.setdefault(nc, 0)
        if nx > max_x[nc]:
            max_x[nc] = nx
            heappush(q, (nc, nx))
```
