# AtCoder Beginner Contest 221 参戦記

## [ABC221A - Seismic magnitude scales](https://atcoder.jp/contests/abc221/tasks/abc221_a)

1分半で突破. 書くだけ.

```python
A, B = map(int, input().split())

print(32 ** (A - B))
```

## [ABC221B - typo](https://atcoder.jp/contests/abc221/tasks/abc221_b)

5分で突破. エレガントに書く手段が思いつかなかった……. エレファントだなあ.

```python
S = input()
T = input()

a = []
for i in range(len(S)):
    if S[i] != T[i]:
        a.append(i)

if len(a) == 0 or (len(a) == 2 and a[0] + 1 == a[1] and S[a[0]] == T[a[1]] and S[a[1]] == T[a[0]]):
    print('Yes')
else:
    print('No')
```

## [ABC221C - Select Mul](https://atcoder.jp/contests/abc221/tasks/abc221_c)

10分で突破. やっぱりエレガントに書く手段が思いつかず. ビット全探索で2分して総当りすればいいやろというエレファントな世界に. 2グループに分けた後は、当然それぞれのグループで降順に並べたものが最大.

```python
from itertools import product

N = input()

result = 0
for p in product(range(2), repeat=len(N)):
    a = [[], []]
    for i in range(len(N)):
        a[p[i]].append(N[i])
    if len(a[0]) == 0 or len(a[1]) == 0:
        continue
    a[0].sort(reverse=True)
    a[1].sort(reverse=True)
    result = max(result, int(''.join(a[0])) * int(''.join(a[1])))
print(result)
```

## [ABC221D - Online games](https://atcoder.jp/contests/abc221/tasks/abc221_d)

29分半で突破、WA2. `s = sorted(s)` を `s = list(s)` と書くとんまでパフォを400溶かした……. 見るからに imos 法で一発なんだけど、値域のせいで出来ないので座標圧縮. 考察は一瞬でした.

```python
from sys import stdin
from itertools import accumulate

readline = stdin.readline

N = int(readline())
AB = [list(map(int, readline().split())) for _ in range(N)]

s = set()
for a, b in AB:
    s.add(a)
    s.add(a + b)
s = sorted(s)

t = {}
for i in range(len(s)):
    t[s[i]] = i
u = [0] * len(s)
for a, b in AB:
    u[t[a]] += 1
    u[t[a + b]] -= 1
u = list(accumulate(u))

D = [0] * (N + 1)
for i in range(len(s) - 1):
    D[u[i]] += s[i + 1] - s[i]
print(*D[1:])
```

言われてみると確かに imos 法は要らんなってなるな.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())

a = []
for _ in range(N):
    A, B = map(int, readline().split())
    a.append((A, 1))
    a.append((A + B, -1))
a.sort()

c = 0
p = 0
D = [0] * (N + 1)
for x, y in a:
    D[c] += x - p
    c += y
    p = x
print(*D[1:])
```

## [ABC221E - LEQ](https://atcoder.jp/contests/abc221/tasks/abc221_e)

入出力例4を手で解いて、あれなんで574じゃないの?と題意が取れていない状態が終了10分前まで続いて、試合終了でした(笑).
