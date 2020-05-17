# AtCoder Beginner Contest 168 参戦記

700番切ったの初めてなのでイヤッッホォォォオオォオウ!! 入水!!!

## [ABC168A - ∴ (Therefore)](https://atcoder.jp/contests/abc168/tasks/abc168_a)

2分で突破. 書くだけ.

```python
N = int(input())

if N % 10 in [2, 4, 5, 7, 9]:
    print('hon')
elif N % 10 in [0, 1, 6, 8]:
    print('pon')
elif N % 10 in [3]:
    print('bon')
```

## [ABC168B - ... (Triple Dots)](https://atcoder.jp/contests/abc168/tasks/abc168_b)

2分で突破. 書くだけ.

```python
K = int(input())
S = input()

if len(S) <= K:
    print(S)
else:
    print(S[:K] + '...')
```

## [ABC168C - : (Colon)](https://atcoder.jp/contests/abc168/tasks/abc168_c)

9分半で突破. 分でも時針が動くことを忘れているアホが発生. 数学は苦手だけど流石にこのレベルならなんとか…….

```python
from math import pi, sin, cos, sqrt

A, B, H, M = map(int, input().split())

y1 = A * cos(2 * pi * (H + M / 60) / 12)
x1 = A * sin(2 * pi * (H + M / 60) / 12)
y2 = B * cos(2 * pi * M / 60)
x2 = B * sin(2 * pi * M / 60)

print(sqrt((y1 - y2) * (y1 - y2) + (x1 - x2) * (x1 - x2)))
```

## [ABC168D - .. (Double Dots)](https://atcoder.jp/contests/abc168/tasks/abc168_d)

9分半で突破. 問題としては入り口から順に幅優先探索するだけで簡単なのだが、'No' の場合は無いんだと心を強く持って提出するのがきつかった.

```python
from collections import deque

N, M = map(int, input().split())
AB = [map(int, input().split()) for _ in range(M)]

links = [[] for _ in range(N + 1)]
for a, b in AB:
    links[a].append(b)
    links[b].append(a)

result = [-1] * (N + 1)
q = deque([1])
while q:
    i = q.popleft()
    for j in links[i]:
        if result[j] == -1:
            result[j] = i
            q.append(j)
print('Yes')
print('\n'.join(str(i) for i in result[2:]))
```

## [ABC168E - ∙ (Bullet)](https://atcoder.jp/contests/abc168/tasks/abc168_e)

解けそうで解けなかった. 多分 Ai か Bi のどちらかが0の時の場合が詰めきれてない.
