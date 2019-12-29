# AtCoder Beginner Contest 149 参戦記

## A - Strings

1分で突破. 書くだけ

```python
S, T = input().split()

print(T + S)
```

## B - Greedy Takahashi

6分で突破. 書くだけ. 最初文章の意味がわかってなくて、どっちもK枚減らすのかと思ったけど、入出力例を試して正しい意味に気づいた.

```python
A, B, K = map(int, input().split())

a = max(A - K, 0)
K -= A - a
b = max(B - K, 0)
print(*[a, b])
```

## C - Next Prime

7分で突破. ABC084D - 2017-like Number で書いたエラトステネスの篩を持ってきて書くだけ.

```python
from math import sqrt

N = 10 ** 6

p = [True] * (N + 1)
p[0] = False
p[1] = False
n = int(sqrt(N) + 1)
for j in range(4, N + 1, 2):
  p[j] = False
for i in range(3, n + 1, 2):
  if p[i]:
    for j in range(i + i, N + 1, i):
      p[j] = False


X = int(input())
for i in range(X, N + 1):
    if p[i]:
        print(i)
        break
```

## D - Prediction and Restriction

44分で突破. WA1. 取り敢えず勝つ手を出す前提で話を進め、K個前の手と同じ手になってしまった場合はあいこにするという実装にしたら WA が出て、「DP しないといけないのかあ!? でも突破人数的にそれはないか(腐った推論)」とだいぶ長時間悩んで、負けの手のほうがいい可能性があることに気づいて AC. そのせいで順位が1400番台になってしまったので、unrated になって助かった感.

```python
N, K = map(int, input().split())
R, S, P = map(int, input().split())
T = input()

hands = [None] * N
d1 = {'r': 'p', 's': 'r', 'p': 's'}
d2 = {'r': P, 's': R, 'p': S}
result = 0
for i in range(len(T)):
    if i >= K and d1[T[i]] == hands[i - K]:
        if i + K < N:
            hands[i] = set('rps') - set(T[i + K]) - set(T[i])
        else:
            hands[i] = T[i]
    else:
        hands[i] = d1[T[i]]
        result += d2[T[i]]
print(result)
```

## E - Handshake

突破できず. オーダーを切り下げる方法が思いつかず TLE いっぱいのまま終了.
