# AtCoder Beginner Contest 188 参戦記

## [ABC188A - Three-Point Shot](https://atcoder.jp/contests/abc188/tasks/abc188_a)

2分で突破. 書くだけ.

```python
X, Y = map(int, input().split())

if Y < X:
    X, Y = Y, X

if X + 3 > Y:
    print('Yes')
else:
    print('No')
```

## [ABC188B - Orthogonality](https://atcoder.jp/contests/abc188/tasks/abc188_b)

2分で突破. 書くだけ.

```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

if sum(a * b for a, b in zip(A, B)) == 0:
    print('Yes')
else:
    print('No')
```

## [ABC188C - ABC Tournament](https://atcoder.jp/contests/abc188/tasks/abc188_c)

7分で突破. N≤16 なので、素直にトーナメントを実行しても *O*(2<sup>17</sup>) なので TLE しないので、素直にやって AC.

```python
N, *A = map(int, open(0).read().split())

a = range(2 ** N)
while len(a) != 2:
    t = []
    for i in range(0, len(a), 2):
        if A[a[i]] > A[a[i + 1]]:
            t.append(a[i])
        else:
            t.append(a[i + 1])
    a = t
if A[a[0]] > A[a[1]]:
    print(a[1] + 1)
else:
    print(a[0] + 1)
```

コンテスト後に、山を真ん中で2つに分けて、一番強いやつがいない側の一番強いやつが準優勝だから、サクッと解けたなと気づいた.

```python
N, *A = map(int, open(0).read().split())

i = A.index(max(A))
if i < 2 ** (N - 1):
    print(A.index(max(A[:2 ** (N - 1)]) + 1)
else:
    print(A.index(max(A[2 ** (N - 1):]) + 1)
```


## [ABC188D - Snuke Prime](https://atcoder.jp/contests/abc188/tasks/abc188_d)

13分で突破. 問題文を見た瞬間に imos 法一発じゃんラッキーと思ったが b<sub>i</sub>≤10<sup>9</sup> を見て憤死した. 辞書で imos 法をするのを脳内シミュレーションしたら全然問題ないことに気づいて驚きつつ AC. 座圧でも良かったらしい. 後で座圧でも解こう.

```python
from sys import stdin

readline = stdin .readline

N, C = map(int, readline().split())

d = {}
for _ in range(N):
    a, b, c = map(int, readline().split())
    d.setdefault(a, 0)
    d[a] += c
    d.setdefault(b + 1, 0)
    d[b + 1] -= c

skeys = sorted(d)
for i in range(1, len(skeys)):
    d[skeys[i]] += d[skeys[i - 1]]
for k in d:
    if d[k] > C:
        d[k] = C

result = 0
for i in range(len(skeys) - 1):
    result += d[skeys[i]] * (skeys[i + 1] - skeys[i])
print(result)
```

追記: 座標圧縮+imos法で解いてみた.

```python
from sys import stdin
from itertools import accumulate

readline = stdin .readline

N, C = map(int, readline().split())
abc = [tuple(map(int, readline().split())) for _ in range(N)]

p = set()
for a, b, _ in abc:
    p.add(a)
    p.add(b)
    p.add(b + 1)
inv = sorted(p)
fwd = {}
for i in range(len(inv)):
    fwd[inv[i]] = i

t = [0] * len(inv)
for a, b, c in abc:
    t[fwd[a]] += c
    t[fwd[b + 1]] -= c
t = list(accumulate(t))

result = 0
for i in range(len(t) - 1):
    if t[i] < C:
        c = t[i]
    else:
        c = C
    result += c * (inv[i + 1] - inv[i])
print(result)
```

## [ABC188E - Peddler](https://atcoder.jp/contests/abc188/tasks/abc188_e)

67分で突破. WA2. 問題文を読んだ瞬間に後ろからやっていけばいいと分かったけど、何故か行けるところ管理に Union Find を使って自爆.

```python
from sys import stdin

readline = stdin.readline

N, M = map(int, readline().split())
A = list(map(int, readline().split()))

links = [[] for _ in range(N)]
for _ in range(M):
    X, Y = map(lambda x: int(x) - 1, readline().split())
    links[X].append(Y)

maxvs = [None] * N
result = -(10 ** 18)
for i in range(N - 1, -1, -1):
    if len(links[i]) == 0:
        maxvs[i] = A[i]
        continue
    maxv = max(maxvs[j] for j in links[i])
    result = max(result, maxv - A[i])
    maxvs[i] = max(maxv, A[i])
print(result)
```

## [ABC188F - +1-1x2](https://atcoder.jp/contests/abc188/tasks/abc188_f)

WA2 まで行ったものの突破できず. Greedy じゃなくてメモ化再帰でやればよかったのか. (Y-1)÷2 を優先していたが、(Y+1)÷2 のほうが良かったことがあったようだ. Xを変化させるのではなく、Yを変化させたほうがいいというのはどこかで似たような問題をやって知ってた.

```python
from functools import lru_cache

X, Y = map(int, input().split())


@lru_cache(maxsize=None)
def f(y):
    if X >= y:
        return abs(X - y)
    if y % 2 == 0:
        return min(abs(y - X), f(y // 2) + 1)
    else:
        return min(abs(y - X), f((y - 1) // 2) + 2, f((y + 1) // 2) + 2)


print(f(Y))
```
