# yukicoder contest 290 参戦記

★1.5が3つ、★2が2つあって前回に続いて ABC っぽさを期待したが、まあいつもの星数詐欺でした.

## [A 1469 programing](https://yukicoder.me/problems/no/1469)

理由は知らないのですが、Python の文字列の加算は遅くないので、素直に加算すれば良い.

```python
S = input()

result = S[0]
for c in S[1:]:
    if c == result[-1]:
        continue
    result += c
print(result)
```

文字列の加算が遅い言語は StringBuilder を使うなり、リストに追加して最後に join するなりすればいいんじゃないですかね.

```python
S = input()

t = [S[0]]
for c in S[1:]:
    if c == t[-1]:
        continue
    t.append(c)
print(''.join(t))
```

## [B 1470 Mex Sum](https://yukicoder.me/problems/no/1470)

2つの値の mex は 1, 2, 3 のどれか. 後は場合分けして計算するだけ.

```python
N, *A = map(int, open(0).read().split())

a, b, c = 0, 0, 0

for x in A:
    if x == 1:
        a += 1
    elif x == 2:
        b += 1
    elif x >= 3:
        c += 1

result = 0
result += a * (a - 1) // 2 * 2  # mex(1, 1)
result += a * b * 3             # mex(1, 2)
result += a * c * 2             # mex(1, >=3)
result += b * (b - 1) // 2 * 1  # mex(2, 2)
result += b * c * 1             # mex(2, >=3)
result += c * (c - 1) // 2 * 1  # mex(>=3, >=3)
print(result)
```

## [C 1471 Sort Queries](https://yukicoder.me/problems/no/1471)

各インデックスまでのアルファベットの累積出現数を求めておけば *O*(26) でクエリに回答できる. *N*,*Q*≦10<sup>4</sup> なので、時間及び空間計算量は 2.6×10<sup>5</sup>となり問題ない.

```python
from sys import stdin

readline = stdin.readline

N, Q = map(int, readline().split())
S = readline()[:-1].encode('us-ascii')

a = [0] * 26
b = []
for c in S:
    i = c - 97
    a[i] += 1
    b.append(a[:])

result = []
for _ in range(Q):
    L, R, X = map(int, readline().split())
    L, R = L - 1, R - 1
    t = b[R][:]
    if L != 0:
        u = b[L - 1]
        for i in range(26):
            t[i] -= u[i]
    c = 0
    for i in range(26):
        c += t[i]
        if c < X:
            continue
        result.append(chr(i + 97))
        break
print(*result, sep='\n')
```

## [E 1473 おでぶなおばけさん](https://yukicoder.me/problems/no/1473)

到達可能な体重を最大化するように、同じ到達可能な体重であれば通る必要のある道の数を最小化するように幅優先探索するだけ.

```python
from collections import deque
from sys import stdin

readline = stdin.readline

INF = 10 ** 18

n, m = map(int, readline().split())

links = [[] for _ in range(n)]
for _ in range(m):
    s, t, d = map(int, readline().split())
    s, t = s - 1, t - 1
    links[s].append((t, d))
    links[t].append((s, d))

q = deque([(0, INF)])
dp = [(-1, -1) for _ in range(n)]
dp[0] = (INF, 0)
while q:
    cp, mw = q.popleft()
    if dp[cp][0] > mw:
        continue
    for np, lw in links[cp]:
        w, s = dp[np]
        nw = min(mw, lw)
        if w >= nw:
            continue
        if w == nw:
            dp[np] = (nw, min(s, dp[cp][1] + 1))
        else:
            dp[np] = (nw, dp[cp][1] + 1)
        q.append((np, nw))
print(dp[n - 1][0], dp[n - 1][1])
```
