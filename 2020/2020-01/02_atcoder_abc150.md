# AtCoder Beginner Contest 150 参戦記

## [ABC150A - 500 Yen Coins](https://atcoder.jp/contests/abc150/tasks/abc150_a)

5分で突破. 書くだけ. 問題文が中々表示されなかったせいで時間がかかった.

```python
K, X = map(int, input().split())

if 500 * K >= X:
    print('Yes')
else:
    print('No')
```

## [ABC150B - Count ABC](https://atcoder.jp/contests/abc150/tasks/abc150_b)

2分半で突破. 書くだけ.

```python
N = int(input())
S = input()

result = 0
for i in range(N):
    if S[i:i+3] == 'ABC':
        result += 1
print(result)
```

## [ABC150C - Count Order](https://atcoder.jp/contests/abc150/tasks/abc150_c)

26分で突破. C問題にしては難しいというか、最初は解ける気がしなかった. 素直に積算すればよかったんだなと…….

```python
N = int(input())
P = list(map(int, input().split()))
Q = list(map(int, input().split()))

fac = [0] * 8
fac[1] = 1
for i in range(2, N):
    fac[i] = i * fac[i - 1]

a = 0
l = list(range(1, N + 1))
for i in range(N):
    a += l.index(P[i]) * fac[len(l) - 1]
    l.remove(P[i])

b = 0
l = list(range(1, N + 1))
for i in range(N):
    b += l.index(Q[i]) * fac[len(l) - 1]
    l.remove(Q[i])

print(abs(a - b))
```

## [ABC150D - Semi Common Multiple](https://atcoder.jp/contests/abc150/tasks/abc150_d)

敗退.

追記: PDF の解説どおり書いてみた.

```python
from fractions import gcd

N, M = map(int, input().split())
A = list(map(int, input().split()))

a = A[0]
n = 1
while a % 2 == 0:
    n *= 2
    a //= 2

lcm = 1
for a in A:
    if a % n != 0 or a % (2 * n) == 0:
        print(0)
        exit()
    lcm = lcm * a // gcd(lcm, a)
    if lcm // 2 > M:
        print(0)
        exit()

print((M - lcm // 2) // lcm + 1)
```
