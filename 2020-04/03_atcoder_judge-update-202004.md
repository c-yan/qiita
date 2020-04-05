# AtCoder Judge System Update Test Contest 202004 参戦記

初の全完で、順位も上位9%という. なんで rated じゃないんですか!(机をバンバン).

## [A - Walking Takahashi](https://atcoder.jp/contests/judge-update-202004/tasks/judge_update_202004_a)

2分で突破. すでに日に当たっている場合は、現在位置が答え. 現在位置がL未満ならLが答え、現在位置がRを超えているのならRが答え.

```python
S, L, R = map(int, input().split())

if L <= S <= R:
    print(S)
elif S < L:
    print(L)
elif S > R:
    print(R)
```

## [B - Picking Balls](https://atcoder.jp/contests/judge-update-202004/tasks/judge_update_202004_b)

6分で突破. R と B に分別して、Rを小さい順に表示した後、Bを小さい順に表示するだけ. 小さい順は当然 sort するだけ.

```python
N = int(input())

R = []
B = []
for _ in range(N):
    X, C = input().split()
    if C == 'R':
        R.append(int(X))
    elif C == 'B':
        B.append(int(X))

R.sort()
B.sort()
if R:
    print('\n'.join(str(r) for r in R))
if B:
    print('\n'.join(str(b) for b in B))
```

## [C - Numbering Blocks](https://atcoder.jp/contests/judge-update-202004/tasks/judge_update_202004_c)

12分で突破. N がせいぜい9なんだから総当りをすればいいだけ. itertools.permutations 愛してる.

```python
from itertools import permutations

a1, a2, a3 = map(int, input().split())
a = [a1, a2, a3]

N = a1 + a2 + a3
result = 0
for p in permutations(range(1, N + 1)):
    X = [p[:a1], p[a1:a1 + a2], p[a1 + a2:]]
    flag = True
    for i in range(3):
        for j in range(1, a[i]):
            if X[i][j] <= X[i][j - 1]:
                flag = False
    for i in range(1, 3):
        for j in range(a[i]):
            if X[i][j] <= X[i - 1][j]:
                flag = False
    if flag:
        result += 1
print(result)
```

## [D - Calculating GCD](https://atcoder.jp/contests/judge-update-202004/tasks/judge_update_202004_d)

23分半で突破. ナイーブにやると O(10<sup>10</sup>) で TLE. X を全ての A と GCD を取るのは、全ての A の GCD と X の GCD を取るのと同じ. これで X が 1 にならない場合は O(10<sup>5</sup>) になり片付いた. X が 1 になった場合の操作回数だが、A の GCD を順に取っていって変わったところだけが候補になるので、変わったところだけをチェックするようにする. これでどのくらいオーダーが下がるのかは不明だけど、直感的には大きく下がるはずなので、提出したら無事 AC.

```python
from math import gcd

N, Q = map(int, input().split())
A = list(map(int, input().split()))
S = list(map(int, input().split()))

gcd_a = A[0]
a = [(1, A[0])]
for i in range(1, N):
    t = gcd(gcd_a, A[i])
    if t != gcd_a:
        gcd_a = t
        a.append((i + 1, gcd_a))

for i in range(Q):
    X = S[i]
    t = gcd(gcd_a, X)
    if t != 1:
        print(t)
    else:
        for j, g in a:
            if gcd(X, g) != 1:
                continue
            print(j)
            break
```

追記: みんなにぶたんで解いているようなので、にぶたんでも解いてみたw

```python
from math import gcd

N, Q = map(int, input().split())
A = list(map(int, input().split()))
S = list(map(int, input().split()))

for i in range(N - 1):
    A[i + 1] = gcd(A[i + 1], A[i])

for i in range(Q):
    X = S[i]
    t = gcd(A[-1], X)
    if t != 1:
        print(t)
    else:
        ng = -1
        ok = N - 1
        while ok - ng > 1:
            m = (ok + ng) // 2
            if gcd(X, A[m]) == 1:
                ok = m
            else:
                ng = m
        print(ok + 1)
```
