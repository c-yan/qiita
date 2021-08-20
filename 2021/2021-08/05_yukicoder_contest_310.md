# yukicoder contest 310 参戦記

## [A 1650 Moving Coins](https://yukicoder.me/problems/no/1650)

少なくともどれかのコインは目標の場所に移動できるはずなので、移動できるやつを一つ一つ順に動かしていけば答えになる. 移動できるやつを毎回全ての中から探すと *O*(*N*<sup>2</sup>) になってしまい TLE になるが、*A*<sub>*i*</sub> を移動して新しく移動可能になりうるものは *A*<sub>*i*-1</sub> と *A*<sub>*i*+1</sub> しかないので、最大でも *O*(3<i>N</i>) となる.

```python
from collections import deque

N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

def is_movable(n):
    if A[n] == B[n]:
        return False
    elif A[n] < B[n]:
        if n == N - 1:
            return True
        else:
            return B[n] < A[n + 1]
    elif A[n] > B[n]:
        if n == 0:
            return True
        else:
            return B[n] > A[n - 1]

result = []
q = deque(range(N))
while q:
    i = q.popleft()
    if not is_movable(i):
        continue

    if A[i] < B[i]:
        moves = B[i] - A[i]
        dir = 'R'
    elif A[i] > B[i]:
        moves = A[i] - B[i]
        dir = 'L'

    for _ in range(moves):
        result.append('%d %s' % (i + 1, dir))

    A[i] = B[i]

    if i != 0:
        q.append(i - 1)
    if i != N - 1:
        q.append(i + 1)

print(len(result))
print(*result, sep='\n')
```

左に動かすもののうち一番左にあるものは移動可能に決まっていた. 左に動かすものを左から順に動かしていき、その後で右に動かすものを右から順に動かしていけば簡単に解ける.

```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

result = []
for i in range(N):
    if A[i] <= B[i]:
        continue
    for _ in range(A[i] - B[i]):
        result.append('%d L' % (i + 1))
for i in range(N - 1, -1, -1):
    if A[i] >= B[i]:
        continue
    for _ in range(B[i] - A[i]):
        result.append('%d R' % (i + 1))
print(len(result))
print(*result, sep='\n')
```
