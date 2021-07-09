# yukicoder 競プロ典型 90 問　期末試験 参戦記

コンテスト終了するまでジャッジが詰まっていたのは初めてですね. いつもより参加者が多かったとはいえ、倍にもなっていないのにここまでのことになるとは、待ち行列理論は恐ろしい.

## [A 1591 Two Digits](https://yukicoder.me/problems/no/1591)

2桁の数というのは10から99なので何も難しいことはないですよね.

```python
N = int(input())

if 10 <= N <= 99:
    print('Yes')
else:
    print('No')
```

## [B 1592 Tenkei 90](https://yukicoder.me/problems/no/1592)

含まれている文字の数を数えて、等しければ作ることができますね.

```python
from collections import Counter

S = input()

c1 = Counter('kyoprotenkei90')
c2 = Counter(S)
for k in c1:
    if c1[k] == c2[k]:
        continue
    print('No')
    break
else:
    print('Yes')
```

わざわざ数えずにソートすればよかった.

```python
S = input()

if sorted('kyoprotenkei90') == sorted(S):
    print('Yes')
else:
    print('No')
```

## [C 1593 Perfect Distance](https://yukicoder.me/problems/no/1593)

*N*≦200000 なので *x* を1から*N*まで試せば分かりますね.

```python
from math import isqrt

N = int(input())

result = 0
for x in range(1, N):
    y = isqrt(N * N - x * x)
    if x * x + y * y == N * N:
        result += 1
print(result)
```

## [D 1594 Three Classes](https://yukicoder.me/problems/no/1594)

*N*≦12 なので全組み合わせを試しても 3<sup>12</sup>≒5.3×10<sup>5</sup> なので大丈夫.

```python
from itertools import product

N, *E = map(int, open(0).read().split())

for p in product([0, 1, 2], repeat=N):
    t = [0] * 3
    for i in range(N):
        t[p[i]] += E[i]
    if t[0] != t[1] or t[1] != t[2] or t[0] != t[2]:
        continue
    print('Yes')
    break
else:
    print('No')
```

## [E 1595 The Final Digit](https://yukicoder.me/problems/no/1595)

時間内に解けず. 何度か似たような問題を解いているのに情けない. まず一番下の桁しか気にしなくていい. 3つのAの組み合わせは10<sup>3</sup>しかないので、少なくともそれだけ回せばループする. ループ長が分かれば余りを取れば良くなり、一瞬で計算できる.

```python
p, q, r, K = map(int, input().split())

A = {}
A[0] = p % 10
A[1] = q % 10
A[2] = r % 10

def conv(a, b, c):
    return (a % 10) * 100 + (b % 10) * 10 + c % 10

for k in range(3, 2000):
    A[k] = (A[k - 1] + A[k - 2] + A[k - 3]) % 10

if K - 1 < 2000:
    print(A[K - 1])
    exit()

d = {}
for k in range(2, 2000):
    x = conv(A[k], A[k - 1], A[k - 2])
    if x in d:
        loop_start = d[x]
        loop_len = k - x

n = ((K - 1) - loop_start) % loop_len + loop_start
print(A[n])
```
