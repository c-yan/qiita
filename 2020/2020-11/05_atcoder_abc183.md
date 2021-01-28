# AtCoder Beginner Contest 183 参戦記

## [ABC183A - ReLU](https://atcoder.jp/contests/abc183/tasks/abc183_a)

1分で突破. 書くだけ.

```python
x = int(input())

if x >= 0:
    print(x)
else:
    print(0)
```

追記: 普段は `max(x, 0)` って書いてるなって思った.

```python
x = int(input())

print(max(x, 0))
```

## [ABC183B - Billiards](https://atcoder.jp/contests/abc183/tasks/abc183_b)

5分半で突破. こういう数学的なのがつらい…….

```python
Sx, Sy, Gx, Gy = map(int, input().split())

print(Sx + (Gx - Sx) * Sy / (Sy + Gy))
```

## [ABC183C - Travel](https://atcoder.jp/contests/abc183/tasks/abc183_c)

5分半で突破. N≤8 なので全部パターンやってみれば OK. といいつつ、`itertools.permutations` が無いと解ける気がしない…….

```python
from itertools import permutations

N, K = map(int, input().split())
T = [list(map(int, input().split())) for _ in range(N)]

result = 0
for p in permutations(range(1, N)):
    i = 0
    t = 0
    for x in p:
        t += T[i][x]
        i = x
    t += T[i][0]
    if t == K:
        result += 1
print(result)
```

## [ABC183D - Water Heater](https://atcoder.jp/contests/abc183/tasks/abc183_d)

5分で突破. imos 法一発.

```python
from itertools import accumulate

N, W = map(int, input().split())

t = [0] * (2 * 10 ** 5 + 1)

for _ in range(N):
    S, T, P = map(int, input().split())
    t[S] += P
    t[T] -= P

if all(x <= W for x in accumulate(t)):
    print('Yes')
else:
    print('No')
```

追記: まあ、imos 法を使わなくても簡単に解けるよね.

```python
N, W = map(int, input().split())

t = {}
for _ in range(N):
    S, T, P = map(int, input().split())
    t.setdefault(S, 0)
    t[S] += P
    t.setdefault(T, 0)
    t[T] -= P

c = 0
for k in sorted(t):
    c += t[k]
    if c <= W:
        continue
    print('No')
    break
else:
    print('Yes')
```

```python
N, W = map(int, input().split())

a = []
for _ in range(N):
    S, T, P = map(int, input().split())
    a.append((S, P))
    a.append((T, -P))

a.sort()
b = []
prev = -1
c = 0
for t, p in a:
    if prev != t:
        b.append(c)
    prev = t
    c += p
b.append(c)

if max(b) <= W:
    print('Yes')
else:
    print('No')
```

## [ABC183E - Queen on Grid](https://atcoder.jp/contests/abc183/tasks/abc183_e)

突破できず. 配列の添字の式がぱっと分からなくて仮に入れておいたのを直し漏れてあとちょっとのところで AC を漏らした. 仮に入れずにコンパイルエラーになるようにしておけばよかった、失敗.

結果を書き込む2次元配列だけで計算できるかと考えてみたが、ナイーブにやると *O*(*HW*(*H*+*W*)) になってしまうから駄目だし、SegmentTree を使って緩和しようにも一番近い石のある位置が *O*(1) で分からないと計算量は減らないしということで、結局左、上、左上方向のアキュムレータが一つづつ必要だという結論になった. 後はラスタスキャンで結果配列にそこへの移動する方法の通り数を埋めていけば良い.

よくよく考えると、結果配列は要らなかった. 以下、PyPy で通るコード.

```python
m = 1000000007

H, W = map(int, input().split())
S = [input() for _ in range(H)]

au = [0] * W             # 上方向のアキュムレータ
aul = [0] * (H + W - 1)  # 左上方向のアキュムレータ
for h in range(H):
    al = 0               # 左方向のアキュムレータ
    for w in range(W):
        n = h - w + (W - 1)
        if h == 0 and w == 0:
            al = 1
            au[w] = 1
            aul[n] = 1
        elif S[h][w] == '#':
            al = 0
            au[w] = 0
            aul[n] = 0
        else:
            t = al + au[w] + aul[n]
            al += t
            al %= m
            au[w] += t
            au[w] %= m
            aul[n] += t
            aul[n] %= m
print(t % m)
```

## [ABC183F - Confluence](https://atcoder.jp/contests/abc183/tasks/abc183_f)

突破できず. 合流処理は Union Find だよなあと一目で思うが、クラスごとの人数の集計は素直にハッシュテーブルでやっても TLE しないんだなと.

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


def merge(a, b):
    if len(a) < len(b):
        a, b = b, a
    for k in b:
        if k in a:
            a[k] += b[k]
        else:
            a[k] = b[k]
    return a


setrecursionlimit(10 ** 6)
readline = stdin.readline

N, Q = map(int, readline().split())
C = list(map(lambda x: int(x) - 1, readline().split()))

parent = [-1] * N
breakdown = [{C[i]: 1} for i in range(N)]
result = []
for _ in range(Q):
    Query = list(map(int, readline().split()))
    if Query[0] == 1:
        a, b = Query[1] - 1, Query[2] - 1
        i = find(parent, a)
        j = find(parent, b)
        if i == j:
            continue
        unite(parent, a, b)
        breakdown[find(parent, a)] = merge(breakdown[i], breakdown[j])
    elif Query[0] == 2:
        x, y = Query[1] - 1, Query[2] - 1
        b = breakdown[find(parent, x)]
        if y in b:
            result.append(b[y])
        else:
            result.append(0)
print(*result, sep='\n')
```
