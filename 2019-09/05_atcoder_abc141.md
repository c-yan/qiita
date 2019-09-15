# AtCoder Beginner Contest 141 参戦記

## A - Weather Prediction

3分半で突破. ボーッとしてて開始に気づかなく、40秒くらい損している. もったいない. 書くだけ.

```python
S = input()
if S == 'Sunny':
  print('Cloudy')
elif S == 'Cloudy':
  print('Rainy')
else:
  print('Sunny')
```

## B - Tap Dance

3分で突破. 0-indexed と 1-indexed で偶奇がひっくり返ることだけ気にした.

```python
S = input()
if all(c in 'RUD' for c in S[::2]) and all(c in 'LUD' for c in S[1::2]):
  print('Yes')
else:
  print('No')
```

## C - Attack Survival

10分半で突破. 制約を見るに、素直に N - 1 人のスコアを -1 して回ると TLE になるのは明々白々なので、正解者を +1 点する方向性で書いたらあっさり通りました.

```python
N, K, Q = map(int, input().split())
score = [K - Q] * (N + 1)
for _ in range(Q):
  score[int(input())] += 1
for i in range(1, N + 1):
  if score[i] > 0:
    print('Yes')
  else:
    print('No')
```

## D - Powerful Discount Tickets

13分半で突破. ABC137D からそんなに経ってないのにまた優先度付きキューですかーと思いながら実装. Dだから素直にM回一番大きいやつを割るのではなく、まとめ割がいるかなあと思ったけどそんなことはなかった.

```python
from heapq import heapify, heappop, heappush
N, M = map(int, input().split())
a = [-int(a) for a in input().split()]
heapify(a)
for _ in range(M):
  heappush(a, heappop(a) / 2)
print(-sum(int(x) for x in a))
```
