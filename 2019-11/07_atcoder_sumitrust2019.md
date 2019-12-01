# AtCoder 三井住友信託銀行プログラミングコンテスト2019 参戦記

## A - November 30

2分で突破. 書くだけ.

```python
M1, D1 = map(int, input().split())
M2, D2 = map(int, input().split())

if D2 == 1:
    print(1)
else:
    print(0)
```

## B - Tax Rate

4分半で突破. 整数にならなかった場合をどうすればいいかわからんなあと適当に出したのに AC だった. ふう.

```python
N = int(input())

X = N / 1.08
if int(X) * 108 // 100 == N:
    print(int(X))
elif int(X + 1) * 108 // 100 == N:
    print(int(X + 1))
else:
    print(':(')
```

## C - 100 to 105

7分半で突破. WA1. 最近の ABC のC問題より難しいなーと思いました. WA したのは難しいせいではなく単なるチョンボです.

```python
X = int(input())

dp = [0] * (X + 1 + 105)
dp[0] = 1

for i in range(X):
    if dp[i] == 1:
        for j in range(100, 106):
            dp[i + j] = 1
print(dp[X])
```

## D - Lucky PIN

22分半で突破. 3文字なので、000～999で最大1000パターンしか無いので全パターン試すのは余裕だけど、N≤30000 だと TLE しそうなので、同じ文字が4つ以上連続した場合は3つに圧縮して探索した. どうせ3文字しか使わないので、4文字以上の並びは無意味.

```python
N = int(input())
S = input()

t = [S[0]]
p = S[0]
r = 1
for i in range(1, N):
    if S[i] == p:
        r += 1
        if r < 4:
            t.append(S[i])
    else:
        r = 1
        t.append(S[i])
T = ''.join(t)

result = 0
for i in range(10):
    a = T.find(str(i))
    if a == -1:
        continue
    for j in range(10):
        b = T.find(str(j), a + 1)
        if b == -1:
            continue
        for k in range(10):
            if T.find(str(k), b + 1) != -1:
                result += 1
print(result)
```

## E - Colorful Hats 2

28分で突破. WA1. A1 が 0 であると信用したのがいけなかった. 先頭からありえるパターンをかけ続けるだけ. E問題にしては簡単.

```python
N = int(input())
A = list(map(int, input().split()))

result = 1
t = [0, 0, 0]
for i in range(N):
    a = A[i]
    f = -1
    k = 0
    for j in range(3):
        if t[j] == a:
            k += 1
            if f == -1:
                t[j] += 1
                f = j
    result = (result * k) % 1000000007
print(result)
```

## F - Interval Running

突破できず. でもF問題としてはものすごく簡単だよね. 初の全完行けそうだったのに、取りこぼして悔しい. 解説を見たら割り切れた時の処理が抜けていただけだった. マジくやしー.

```python
from sys import exit

T1, T2 = map(int, input().split())
A1, A2 = map(int, input().split())
B1, B2 = map(int, input().split())

if A1 > B1:
    A1, B1 = B1, A1
    A2, B2 = B2, A2

if A2 < B2:
    print(0)
    exit()

if A1 * T1 + A2 * T2 < B1 * T1 + B2 * T2:
    print(0)
    exit()

if A1 * T1 + A2 * T2 == B1 * T1 + B2 * T2:
    print('infinity')
    exit()

a = B1 * T1 - A1 * T1
b = (A1 * T1 + A2 * T2) - (B1 * T1 + B2 * T2)
t = a // b

if a % b == 0:
    print(t * 2)
else:
    print(t * 2 + 1)
```
