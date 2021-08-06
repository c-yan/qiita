# AtCoder Beginner Contest 212 参戦記

## [ABC212A - Alloy](https://atcoder.jp/contests/abc212/tasks/abc212_a)

2分半で突破. 書くだけ.

```python
A, B = map(int, input().split())

if A > 0 and B == 0:
    print('Gold')
elif A == 0 and B > 0:
    print('Silver')
elif A > 0 and B > 0:
    print('Alloy')
```

## [ABC212B - Weak Password](https://atcoder.jp/contests/abc212/tasks/abc212_b)

4分で突破. 書くだけ.

```python
X = list(map(int, input()))

if X.count(X[0]) == 4 or all(X[i + 1] == (X[i] + 1) % 10 for i in range(3)):
    print('Weak')
else:
    print('Strong')
```

## [ABC212C - Min Difference](https://atcoder.jp/contests/abc212/tasks/abc212_c)

12分半で突破. 単純に2重ループの *O*(*NM*) で駄目なら、にぶたんで *O*((*N+M*)log<i>M</i>) にすることを思いつく. A<sub>i</sub> 以下の B のインデックスを見つけて、その前後だけ差分を取ればいい.

```python
from bisect import bisect_left

N, M = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

B.sort()

result = 10 ** 9
for a in A:
    i = bisect_left(B, a)
    if i != 0:
        result = min(result, abs(a - B[i - 1]))
    if i != M:
        result = min(result, abs(a - B[i]))
print(result)
```

A も小さい順に並べてしまえば、にぶたんを使わなくても順次 A<sub>i</sub> 以下の B のインデックスを見つけることができる.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

B.sort()

result = 10 ** 9
i = 0
for a in sorted(A):
    while i != M and B[i] < a:
        i += 1
    if i != 0:
        result = min(result, abs(a - B[i - 1]))
    if i != M:
        result = min(result, abs(a - B[i]))
print(result)
```

## [ABC212D - Querying Multiset](https://atcoder.jp/contests/abc212/tasks/abc212_d)

6分で突破. 操作2をバカ正直にやると *O*(*Q*<sup>2</sup>) になって TLE. 袋の中の数字と袋の外の補正値を足すと本当の数値になるという持ち方にすると操作2が *O*(1) になる. 操作3をバカ正直にやるとやっぱり *O*(*Q*<sup>2</sup>) になって TLE するので、優先度付きキューか平衡二分探索木の出番です. Python には組み込みの平衡二分探索木がないので heapq になるいつもの奴.

```python
from sys import stdin
from heapq import heappop, heappush

readline = stdin.readline

Q = int(readline())
t = 0
result = []
bag = []
for _ in range(Q):
    query = readline()
    if query[0] == '1':
        _, X = map(int, query.split())
        heappush(bag, X - t)
    elif query[0] == '2':
        _, X = map(int, query.split())
        t += X
    elif query[0] == '3':
        result.append(heappop(bag) + t)
print(*result, sep='\n')
```
