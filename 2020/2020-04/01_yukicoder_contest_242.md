# yukicoder contest 242 参戦記

## [A 1015 おつりは要らないです](https://yukicoder.me/problems/no/1015)

まず1万円以上の店に、超えない範囲で1万円を割り当てます. (1万円だったら、1万円、1万1円でも1万円、2万円なら2万円). 全部割り当ててもまだ1万円が残っている場合には、残額が大きい店順に1枚づつ割り当てます. 同様のことを5千円でもします. 残額が残っている店がまだあるのであれば、千円札で払えるかを確認します.

ソートと払い終わった店の排除を繰り返すことにより解けました.

```python
N, X, Y, Z = map(int, input().split())
A = list(map(int, input().split()))

A = [a + 1 for a in A]
A.sort(reverse=True)

for i in range(len(A)):
    if Z == 0:
        break
    if A[i] >= 10000:
        t = A[i] // 10000
        t = min(t, Z)
        Z -= t
        A[i] -= t * 10000
    else:
        break
A = [a for a in A if a > 0]
A.sort(reverse=True)
A = A[Z:]

for i in range(len(A)):
    if Y == 0:
        break
    if A[i] >= 5000:
        t = A[i] // 5000
        t = min(t, Y)
        Y -= t
        A[i] -= t * 5000
    else:
        break
A = [a for a in A if a > 0]
A.sort(reverse=True)
A = A[Y:]

for i in range(len(A)):
    t = (A[i] + 999) // 1000
    t = min(t, X)
    X -= t
    A[i] -= t * 1000
A = [a for a in A if a > 0]

if len(A) == 0:
    print('Yes')
else:
    print('No')
```

ソートを繰り返す代わりに、優先度付きキューを使っても解けました.

```python
from heapq import heapify, heappop, heappush

N, X, Y, Z = map(int, input().split())
A = list(map(int, input().split()))

A = [-a for a in A]
heapify(A)

while A:
    if Z == 0:
        break
    x = -heappop(A)
    if x >= 10000:
        t = min(x // 10000, Z)
        Z -= t
        x -= 10000 * t
        heappush(A, -x)
    else:
        Z -= 1

while A:
    if Y == 0:
        break
    x = -heappop(A)
    if x >= 5000:
        t = min(x // 5000, Y)
        Y -= t
        x -= 5000 * t
        heappush(A, -x)
    else:
        Y -= 1

while A:
    if X == 0:
        break
    x = -heappop(A)
    t = min((x + 1000) // 1000, X)
    X -= t
    x -= t * 1000
    if x >= 0:
        heappush(A, -x)

if len(A) == 0:
    print('Yes')
else:
    print('No')
```
