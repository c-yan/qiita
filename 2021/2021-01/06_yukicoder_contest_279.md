# yukicoder contest 279 (Zelkova and Cherry) 参戦記

事前にググって Zelkova が欅なのは分かってたが、問題タイトルを見るまで欅坂だとは分からなかった(名前が変わったことを知らなかった).

## [E 1362 [Zelkova 8th Tune] Black Sheep](https://yukicoder.me/problems/no/1362)

書くだけ. yukicoder でこんなに簡単な問題出るの珍しい. 辞書かなんかで素直に出現数を集計して、出現数が1の文字を確認して、もう一回スキャンして位置を確認すれば良い.

```python
from collections import Counter

S = input()

c = Counter(S)
a = [k for k in c if c[k] == 1][0]
print(S.index(a) + 1, a)
```

## [C 1360 [Zelkova 4th Tune] 協和音](https://yukicoder.me/problems/no/1360)

N≦18 なので、見るからにビット全探索だなと. 一つも鳴らさないのは答えにならないことに注意.

```python
from itertools import product

N = int(input())
A = list(map(int, input().split()))
B = [list(map(int, input().split())) for _ in range(N)]

W = -(10 ** 18)
for bits in product(range(2), repeat=N):
    if sum(bits) == 0:
        continue
    w = 0
    for i in range(N):
        if bits[i] == 1:
            w += A[i]
    for i in range(N):
        for j in range(i + 1, N):
            if bits[i] & bits[j] == 1:
                w += B[i][j]
    if w > W:
        W = w
        max_bits = bits
print(W)
print(*(i + 1 for i in range(N) if max_bits[i] == 1))
```

## [B 1359 [Zelkova 3rd Tune] 四人セゾン](https://yukicoder.me/problems/no/1359)

直感的に小さい順か大きい順に消化してかないとだめだよなと思いつつ、★の数が多いのでそんなに単純ではないのかとドキドキしながら流したら AC したのであっていたようだ.

```python
N, K, M = map(int, input().split())
P = list(map(int, input().split()))
E = list(map(int, input().split()))
A = list(map(int, input().split()))
H = list(map(int, input().split()))

P.sort()
E.sort()
A.sort()
H.sort()

result = 0
for x in zip(P, E, A, H):
    y = sorted(x)
    result += pow(y[-1] - y[0], K, M)
    result %= M
print(result)
```
