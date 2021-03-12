# yukicoder contest 286 参戦記

## [A 1422 Social Distance in Train](https://yukicoder.me/problems/no/1422)

奇数の場合には1種類しかないのはすぐ分かる. 偶数について、N=2 の場合には「ox」「xo」の2つ、N=4 の場合には「oxox」「oxxo」「xoxo」の3つ、N=6 の場合には「oxoxox」「oxoxxo」「oxxoxo」「xoxoxo」の4つ……の辺りでピンとくるはず.

```python
N = int(input())

if N % 2 == 0:
    print(N // 2 + 1)
elif N % 2 == 1:
    print(1)
```

## [B 1423 Triangle of Multiples](https://yukicoder.me/problems/no/1423)

非退化三角形ってなんだよ、3頂点が同一直線上にあるってことは面積が0だけど、それって三角形なのかよとあまりの分からなさに混乱したが、非退化三角形でググったら Codeforces の解説が出てきて、満たすべき条件は分かったので解けた…が、単に A✕B✕C や lcm(A, B, C) でも良いことには全く気づいてなかった.

```python
from math import gcd


def lcm(x, y):
    return x * y // gcd(x, y)


T = int(input())

for _ in range(T):
    A, B, C = map(int, input().split())
    x = lcm(lcm(A, B), C)
    print(x + A, x + B, x + C)
```
