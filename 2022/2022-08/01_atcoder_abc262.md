# 第三回日本最強プログラマー学生選手権-予選- (AtCoder Beginner Contest 262) 不参戦記

## [ABC262A - World Cup](https://atcoder.jp/contests/abc262/tasks/abc262_a)

書くだけ.

```python
Y = int(input())

if Y % 4 == 0:
    print(Y + 2)
elif Y % 4 == 1:
    print(Y + 1)
elif Y % 4 == 2:
    print(Y)
elif Y % 4 == 3:
    print(Y + 3)
```

## [ABC262B - Triangle (Easier)](https://atcoder.jp/contests/abc262/tasks/abc262_b)

素直に3重ループの総当たりで OK.

```python
from sys import stdin

readline = stdin.readline

N, M = map(int, readline().split())

links = [set() for _ in range(N)]
for _ in range(M):
    U, V = map(int, readline().split())
    links[U - 1].add(V - 1)

result = 0
for a in range(N - 2):
    for b in range(a + 1, N - 1):
        for c in range(b + 1, N):
            if b not in links[a]:
                continue
            if c not in links[b]:
                continue
            if c not in links[a]:
                continue
            result += 1
print(result)
```

## [ABC262C - Min Max Pair](https://atcoder.jp/contests/abc262/tasks/abc262_c)

$a_i = i, a_j = j$ もしくは $a_i = j, a_j = i$ である組み合わせ全部.

```python
from itertools import accumulate

N, *a = map(int, open(0).read().split())

b = [0] * N
for i in range(N):
    if a[i] != i + 1:
        continue
    b[i] = 1
b = list(accumulate(b))

result = 0
for i in range(N):
    if a[i] != i + 1:
        continue
    result += b[-1] - b[i]

d = {}
for i in range(N):
    d[i + 1] = a[i]
t = 0
for i in range(1, N + 1):
    if d[i] == i:
        continue
    if d[d[i]] != i:
        continue
    t += 1
result += t // 2

print(result)
```

## [ABC262D - I Hate Non-integer Number](https://atcoder.jp/contests/abc262/tasks/abc262_d)

どストレートな DP で特に難しいところはなかったのだが、`%= m` を `% m` と typo していることに気づくのに無駄に時間がかかった _(┐「ε:)_

$N \le 100$ なので、x 個選んだとき x で割った余りで DP すれば良い.

```python
m = 998244353

N, *a = map(int, open(0).read().split())

result = 0
for i in range(1, N + 1):
    dp = [[0] * i for _ in range(i + 1)]
    dp[0][0] = 1
    for j in range(N):
        for k in range(i - 1, -1, -1):
            for l in range(i):
                if dp[k][l] == 0:
                    continue
                dp[k + 1][(l + a[j]) % i] += dp[k][l]
                dp[k + 1][(l + a[j]) % i] %= m
    result += dp[i][0]
    result %= m
print(result)
```
