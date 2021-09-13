# AtCoder Beginner Contest 216 バーチャル参戦記

58:40でABCDE完. 参加できてたらパフォ1486だった模様. レーティング上がってたな、惜しい.

## [ABC216A - Signed Difficulty](https://atcoder.jp/contests/abc216/tasks/abc216_a)

3分で突破. 書くだけ.

```python
X, Y = map(int, open(0).read().split('.'))

if 0 <= Y <= 2:
    print('%d-' % X)
elif 3 <= Y <= 6:
    print('%d' % X)
elif 7 <= Y <= 9:
    print('%d+' % X)
```

## [ABC216B - Same Name](https://atcoder.jp/contests/abc216/tasks/abc216_b)

3分半で突破. ダブりチェックと言えばセット.

```python
N = int(input())

t = set()
for _ in range(N):
    S, T = input().split()
    t.add(S + ' ' + T)

if len(t) != N:
    print('Yes')
else:
    print('No')
```

## [ABC216C - Many Balls](https://atcoder.jp/contests/abc216/tasks/abc216_c)

5分で突破. 見た瞬間に逆向きにやれば最短が作れると思いました. なので、逆向きにやって、反転させて答えとして出力しておしまい.

```python
N = int(input())

a = []
while N != 0:
    if N % 2 == 0:
        a.append('B')
        N //= 2
    else:
        a.append('A')
        N -= 1
print(*reversed(a), sep='')
```

## [ABC216D - Pair of Balls](https://atcoder.jp/contests/abc216/tasks/abc216_d)

15分半で突破. ナイーブに書くと TLE するのは火を見るよりも明らか. 新しい色は今取り出して捨てた筒からしかでてこないということを大事にし、その2つの筒から新しく先頭になったボールと、一致する先頭のボールがあるのであればその筒番号が O(1) で判明するように実装しました.

```python
from sys import stdin
from collections import deque

readline = stdin.readline

N, M = map(int, readline().split())
a = [None] * M
for i in range(M):
    k = int(readline())
    a[i] = deque(map(int, readline().split()))

q = deque(range(M))
t = {}
while q:
    i = q.popleft()
    if len(a[i]) == 0:
        continue
    x = a[i].popleft()
    if x not in t:
        t[x] = i
        continue
    q.append(i)
    q.append(t[x])
    del t[x]

if all(len(d) == 0 for d in a):
    print('Yes')
else:
    print('No')
```

## [ABC216E - Amusement Park](https://atcoder.jp/contests/abc216/tasks/abc216_e)

31分半で突破. 素直に一番満足度が高いアトラクションに乗って、満足度を1下げるのループをシミュレーションするのは K≤2×10<sup>9</sup> から TLE するのは明らか. ざっくりとどの満足度まで下がるまで乗り倒せばいいかをにぶたんで求めて、余った部分は素直にシミュレーションすれば TLE しないだろうというあまり厳密ではない考察をして、書いて出したら通った. なお、最初は必ずK回乗る実装になっていて、入出力例2に救われた. 危ない.

```python
from heapq import heappop, heappush

N, K, *A = map(int, open(0).read().split())


def is_ok(n):
    result = 0
    for a in A:
        if a <= n:
            continue
        result += a - n
    return result <= K


ok = 10 ** 10
ng = 0
while ok - ng > 1:
    m = ng + (ok - ng) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m

result = 0
n = 0
t = []
for a in A:
    if a <= ok:
        heappush(t, -a)
        continue
    heappush(t, -ok)
    result += a * (a + 1) // 2 - ok * (ok + 1) // 2
    n += a - ok
for _ in range(K - n):
    x = -heappop(t)
    if x < 0:
        break
    result += x
    heappush(t, -(x - 1))
print(result)
```
