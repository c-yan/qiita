# AtCoder ZONeエナジー プログラミングコンテスト "HELLO SPACE" 参戦記

## [ZONE2021A - UFO襲来](https://atcoder.jp/contests/zone2021/tasks/zone2021_a)

3分半で突破. 書くだけ.

```python
S = input()

result = 0
i = 0
while True:
    i = S.find('ZONe', i) + 1
    if i == 0:
        break
    result += 1
print(result)
```

追記: 無駄に頑張ってしまっていたことに気づいた.

```python
S = input()

print(S.count('ZONe'))
```

## [ZONE2021B - 友好の印](https://atcoder.jp/contests/zone2021/tasks/zone2021_b)

9分半で突破. にぶたんで書いたけど、今考えるとB問題でにぶたん要らんよな. ミスった.

```python
N, D, H = map(int, input().split())
dh = [tuple(map(int, input().split())) for _ in range(N)]


def is_ok(n):
    for d, h in dh:
        if (H - n) / D * d + n >= h:
            continue
        return False
    else:
        return True


ng = 0
ok = 10 ** 3
for _ in range(10000):
    m = (ok + ng) / 2
    if is_ok(m):
        ok = m
    else:
        ng = m
print(ok)
```

追記: (*d<sub>i</sub>*, *h<sub>i</sub>*) と (*D*, *H*) を通る直線の式を求めて、*x*座標が0のときの*y*座標の最大値が答えですね.

```python
N, D, H = map(int, input().split())
dh = [tuple(map(int, input().split())) for _ in range(N)]

print(max(max(h - (H - h) / (D - d) * d for d, h in dh), 0))
```

## [ZONE2021C - MAD TEAM](https://atcoder.jp/contests/zone2021/tasks/zone2021_c)

突破できず. 解説動画を見てアハ体験. これは思いつけないなあ.

```python
from itertools import combinations

N = int(input())
ABCDE = [tuple(map(int, input().split())) for _ in range(N)]


def f(n, t):
    result = 0
    for i in range(5):
        if t[i] < n:
            continue
        result |= 1 << i
    return result


def is_ok(n):
    s = set()
    for i in range(N):
        s.add(f(n, ABCDE[i]))
    t = list(s)
    if len(t) == 1:
        return t[0] == 31
    elif len(t) == 2:
        return t[0] | t[1] == 31
    for c in combinations(t, 3):
        if c[0] | c[1] | c[2] != 31:
            continue
        return True
    else:
        return False


ok = 0
ng = 10 ** 9 + 1
while ng - ok > 1:
    m = ok + (ng - ok) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m
print(ok)
```

## [ZONE2021D - 宇宙人からのメッセージ](https://atcoder.jp/contests/zone2021/tasks/zone2021_d)

7分で突破. 過去問に同じようなのがあったような. 反転を真面目に実行すると TLE 一直線なので、フラグ管理して、くっつける向きを変えるだけ. 連続消去も最後にやるとめんどくさいだけなので、足しこみながらやればワンパス.

```python
from collections import deque

S = input()

T = deque([])
is_reversed = False

for c in S:
    if c == 'R':
        is_reversed = not is_reversed
        continue
    if len(T) == 0:
        T.append(c)
        continue

    if is_reversed:
        if T[0] == c:
            T.popleft()
        else:
            T.appendleft(c)
    else:
        if T[-1] == c:
            T.pop()
        else:
            T.append(c)

if is_reversed:
    T = reversed(T)
print(''.join(T))
```

## [ZONE2021E - 潜入](https://atcoder.jp/contests/zone2021/tasks/zone2021_e)

57分で突破、WA5、TLE3. 大体はダイクストラ法でいいのだが、素直にやると (*r*−*i*,*c*) の移動が重くて TLE になってしまう. (*r*+1, *c*) と (*r*−*i*,*c*) の移動の直後に (*r*−*i*,*c*) の移動をするのは無駄なので、フラグ管理でその時だけはしないようにしてやったら AC した.

```python
from heapq import heappop, heappush

INF = 10 ** 15

R, C = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(R)]
B = [list(map(int, input().split())) for _ in range(R - 1)]

dp = [[INF] * C for _ in range(R)]
dp[0][0] = 0
q = [(0, 0, 0, True)]
while q:
    cost, r, c, no_fall = heappop(q)
    if dp[r][c] != cost:
        continue
    # (r, c + 1)
    if c < C - 1:
        ncost = cost + A[r][c]
        if dp[r][c + 1] > ncost:
            dp[r][c + 1] = ncost
            heappush(q, (ncost, r, c + 1, False))
    # (r, c - 1)
    if c > 0:
        ncost = cost + A[r][c - 1]
        if dp[r][c - 1] > ncost:
            dp[r][c - 1] = ncost
            heappush(q, (ncost, r, c - 1, False))
    # (r + 1, c)
    if r < R - 1:
        ncost = cost + B[r][c]
        if dp[r + 1][c] > ncost:
            dp[r + 1][c] = ncost
            heappush(q, (ncost, r + 1, c, True))
    if no_fall:
        continue
    # (r - i, c)
    for i in range(1, r + 1):
        ncost = cost + (1 + i)
        if dp[r - i][c] > ncost:
            dp[r - i][c] = ncost
            heappush(q, (ncost, r - i, c, True))
print(dp[R - 1][C - 1])
```
