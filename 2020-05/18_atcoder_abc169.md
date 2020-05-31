# AtCoder Beginner Contest 169 参戦記

## [ABC169A - Multiplication 1](https://atcoder.jp/contests/abc169/tasks/abc169_a)

1分半で突破. 書くだけ. コードテストがなかなか実行されなかったせいで無駄に時間がかかってしまった.

```python
A, B = map(int, input().split())

print(A * B)
```

## [ABC169B - Multiplication 2](https://atcoder.jp/contests/abc169/tasks/abc169_b)

3分で突破. 64bit 整数でもオーバーフローする問題だけど、Python だと何も考えなくていいので楽ですねー. 0を先頭にするためにソートするのを忘れなければ OK!

```python
N = int(input())
A = list(map(int, input().split()))

limit = 10 ** 18
A.sort()
result = A[0]
for a in A[1:]:
    result *= a
    if result > limit:
        print(-1)
        exit()
print(result)
```

## [ABC169C - Multiplication 3](https://atcoder.jp/contests/abc169/tasks/abc169_c)

3分で突破. A<sub>i</sub>≤10<sup>18</sup> を見た瞬間に double だとヤバいと理解したので、decimal に逃げた.

```python
from decimal import Decimal

A, B = map(Decimal, input().split())

print(int(A * B))
```

## [ABC169D - Div Game](https://atcoder.jp/contests/abc169/tasks/abc169_d)

22分半で突破、WA1. とりあえずエラトステネスの篩を貼り付けようと思ったが、N≤10<sup>12</sup> なので貼れず、sqrt(N) までの処理に直すところからスタート. その後に p<sup>e</sup> を列挙して、ガンガン順に割っていって残った値を処理するところで失敗して WA を貰ったけど、もしかしてこのコードも嘘解法かもしれない. (ちゃんと残ったのが素数かどうかチェックしないと駄目なような?)

```python
from math import sqrt

N = int(input())

rn = int(sqrt(N))
sieve = [0] * (rn + 1)
sieve[0] = -1
sieve[1] = -1
t = [0] * (rn + 1)
for i in range(2, rn + 1):
    if sieve[i] != 0:
        continue
    sieve[i] = i
    j = i
    while j < rn + 1:
        t[j] = 1
        j *= i
    for j in range(i * i, rn + 1, i):
        if sieve[j] == 0:
            sieve[j] = i

result = 0
last = -1
for i in range(2, rn + 1):
    if t[i] == 0:
        continue
    if N % i == 0:
        result += 1
        N //= i
        last = i
if N != 1 and N > rn:
    result += 1
print(result)
```

## [ABC169E - Count Median](https://atcoder.jp/contests/abc169/tasks/abc169_e)

突破できず. 1時間以上考えてたわけだが、考えれば考えるほど難しい.
