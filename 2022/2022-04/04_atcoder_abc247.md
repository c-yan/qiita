# AtCoder Beginner Contest 247 参戦記

## [ABC247A - Move Right](https://atcoder.jp/contests/abc247/tasks/abc247_a)

2分半で突破. 書くだけ.

```python
S = input()

print('0' + S[:3])
```

## [ABC247B - Unique Nicknames](https://atcoder.jp/contests/abc247/tasks/abc247_b)

7分半で突破、WA1. 書くだけなんだけど、姓と名が同じ人がいてハマった.

```python
N = int(input())
st = [input().split() for _ in range(N)]

d = {}
for s, t in st:
    d.setdefault(s, 0)
    d.setdefault(t, 0)
    if s == t:
        d[s] += 1
    else:
        d[s] += 1
        d[t] += 1

for s, t in st:
    if d[s] != 1 and d[t] != 1:
        print('No')
        exit()
print('Yes')
```

## [ABC247C - 1 2 1 3 1 2 1](https://atcoder.jp/contests/abc247/tasks/abc247_c)

4分半で突破. *N*≦16なので、実際に数列を作っても問題ないので簡単.

```python
N = int(input())

S = [None] * N
S[0] = [1]
for i in range(N - 1):
    S[i + 1] = S[i] + [i + 2] + S[i]
print(*S[N - 1])
```

## [ABC247D - Cylinder](https://atcoder.jp/contests/abc247/tasks/abc247_d)

9分半で突破、WA1. 頭の中で書かないとと思ってた処理を書き忘れる凡ミス. 似たような問題が前にもあったような. 数とボールの個数のペアを deque で管理するだけの簡単な問題.

```python
from collections import deque
from sys import stdin

readline = stdin.readline

Q = int(readline())

result = []
q = deque([])
for _ in range(Q):
    query = readline()
    if query[0] == '1':
        _, x, c = map(int, query.split())
        q.append((x, c))
    if query[0] == '2':
        _, c = map(int, query.split())
        r = c
        t = 0
        while True:
            x, c = q.popleft()
            y = min(r, c)
            t += x * y
            r -= y
            c -= y
            if r == 0:
                if c != 0:
                    q.appendleft((x, c))
                break
        result.append(t)
if len(result) != 0:
    print(*result, sep='\n')
```

## [ABC247E - Max Min](https://atcoder.jp/contests/abc247/tasks/abc247_e)

52分で突破. 最大と最小を含む整数の組を直接計算できなかったので、すべての整数の組数から最大を含まない組数と最小を含まない組数を引いて、最大と最小を含まない組数を足して求めた. 最大と最小が同じ時は *L*=*R* もOKなのを見落としていたが、入出力例に救われた.

```python
N, X, Y, *A = map(int, open(0).read().split())

sections = []
t = []
for i in range(N):
    if A[i] > X or A[i] < Y:
        if len(t) != 0:
            sections.append(t)
            t = []
        continue
    t.append(A[i])
if len(t) != 0:
    sections.append(t)


def asplit(a, sep):
    t = []
    for e in a:
        if e in sep:
            if len(t) != 0:
                yield t
                t = []
            continue
        t.append(e)
    if len(t) != 0:
        yield t


result = 0
for s in sections:
    c = len(s) * (len(s) - 1) // 2
    for t in asplit(s, [X]):
        c -= len(t) * (len(t) - 1) // 2
    for t in asplit(s, [Y]):
        c -= len(t) * (len(t) - 1) // 2
    for t in asplit(s, [X, Y]):
        c += len(t) * (len(t) - 1) // 2
    result += c
if X == Y:
    result += A.count(X)
print(result)
```
