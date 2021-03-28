# yukicoder contest 288 参戦記

## [A 1438 Broken Drawers](https://yukicoder.me/problems/no/1438)

昇順に並んでいない組はどちらかしか開けれないのでその分だけ差し引けばいい.

```python
N, *A = map(int, open(0).read().split())

c = 0
i = 0
while i < N - 1:
    if A[i] > A[i + 1]:
        c += 1
        i += 1
    i += 1
print(N - c)
```

## [B 1439 Let's Compare!!!!](https://yukicoder.me/problems/no/1439)

不一致の最左点だけ比較すれば大小が分かるので、その位置を押さえる. その位置より右で変更があった場合は大小は変わらない. その位置より左で変更があった場合はそこの位置を見るように変える. 現在見ている場所が一地になった場合はそこから次に不一致の点に移動する. これ文字列の一番左と右を行ったり来たりさせられるテストケースで TLE すると思うけど AC できたのでヨシ!

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
S = list(readline()[:-1])
T = list(readline()[:-1])
Q = int(readline())

p = 0
while p < N and S[p] == T[p]:
    p += 1

result = []
for _ in range(Q):
    c, x, y = readline().split()
    x = int(x) - 1
    if  c == 'S':
        S[x] = y
    elif c == 'T':
        T[x] = y
    if x < p and S[x] != T[x]:
        p = x
    elif x == p and S[x] == T[x]:
        while p < N and S[p] == T[p]:
            p += 1

    if p == N:
        result.append('=')
    elif S[p] > T[p]:
        result.append('>')
    elif S[p] < T[p]:
        result.append('<')
print(*result, sep='\n')
```

heapq と set を組み合わせて、TLE しないコードを書いた.

```python
from sys import stdin
from heapq import heapify, heappop, heappush

readline = stdin.readline

N = int(readline())
S = list(readline()[:-1])
T = list(readline()[:-1])
Q = int(readline())

s = set()
q = []
for i in range(N):
    if S[i] == T[i]:
        continue
    q.append(i)
    s.add(i)
heapify(q)

result = []
for _ in range(Q):
    c, x, y = readline().split()
    x = int(x) - 1

    is_same = S[x] == T[x]
    if c == 'S':
        S[x] = y
    elif c == 'T':
        T[x] = y
    if (S[x] == T[x]) != is_same:
        if is_same:
            heappush(q, x)
            s.add(x)
        else:
            s.remove(x)

    while len(q) != 0 and q[0] not in s:
        heappop(q)

    if len(q) == 0:
        result.append('=')
        continue

    p = q[0]
    if S[p] > T[p]:
        result.append('>')
    elif S[p] < T[p]:
        result.append('<')
print(*result, sep='\n')
```
