# AtCoder Beginner Contest 210 参戦記

## [ABC210A - Cabbages](https://atcoder.jp/contests/abc210/tasks/abc210_a)

2分で突破. 書くだけ.

```python
N, A, X, Y = map(int, input().split())

if N <= A:
    print(X * N)
else:
    print(X * A + Y * (N - A))
```

## [ABC210B - Bouzu Mekuri](https://atcoder.jp/contests/abc210/tasks/abc210_b)

2分半で突破. 書くだけ.

```python
N = int(input())
S = input()

if S.find('1') % 2 == 0:
    print('Takahashi')
else:
    print('Aoki')
```

## [ABC210C - Colorful Candies](https://atcoder.jp/contests/abc210/tasks/abc210_c)

固定ウインドウで処理をすればいいのが分かる. 10<sup>9</sup> 種類なので、配列で個数を管理できないので辞書で個数管理をした. 座圧すれば 10<sup>5</sup> まで縮むので配列でも管理できますが…….

```python
from collections import Counter

N, K, *c = map(int, open(0).read().split())

ct = Counter(c[:K])
t = len(ct)
result = t
for i in range(0, N - K):
    a = c[i]
    ct[a] -= 1
    if ct[a] == 0:
        t -= 1
    b = c[i + K]
    ct.setdefault(b, 0)
    if ct[b] == 0:
        t += 1
    ct[b] += 1
    result = max(result, t)
print(result)
```

## [ABC210D - National Railway](https://atcoder.jp/contests/abc210/tasks/abc210_d)

78分半で突破. WA5. 全く思いつかず、ダメ元で最悪 *O*(10<sup>12</sup>)、最善 *O*(10<sup>6</sup>) のコードを書いてみたら通った. *C*=1 のときに死ぬかなと思ったので意外.

```python
from sys import stdin

readline = stdin.readline

H, W, C = map(int, readline().split())
A = [list(map(int, readline().split())) for _ in range(H)]

result = A[0][0] + A[0][1] + C
m = min(min(a) for a in A)
for i in range(H):
    for j in range(W):
        for d in range(1, 10 ** 6):
            t = A[i][j] + C * d
            if t + m > result:
                break
            for k in range(d + 1):
                if i + k >= H:
                    break
                l = d - k
                if j + l < W:
                    result = min(result, t + A[i + k][j + l])
                if j - l >= 0:
                    result = min(result, t + A[i + k][j - l])
print(result)
```

解説を読んで *O*(10<sup>6</sup>) の DP で書いてみたが、予想外なことに上のコードのほうがちょっとだけ速かった. mjk という感想しかない.

```python
from sys import stdin

readline = stdin.readline

H, W, C = map(int, readline().split())
A = [list(map(int, readline().split())) for _ in range(H)]

result = 10 ** 15

dp = [A[i][:] for i in range(H)]
for i in range(H):
    for j in range(W):
        if i != 0:
            result = min(result, dp[i][j] + dp[i - 1][j] + C)
        if j != 0:
            result = min(result, dp[i][j] + dp[i][j - 1] + C)
        if i != 0:
            dp[i][j] = min(dp[i][j], dp[i - 1][j] + C)
        if j != 0:
            dp[i][j] = min(dp[i][j], dp[i][j - 1] + C)

dp = [A[i][:] for i in range(H)]
for i in range(H):
    for j in range(W - 1, -1, -1):
        if i != 0:
            result = min(result, dp[i][j] + dp[i - 1][j] + C)
        if j != W - 1:
            result = min(result, dp[i][j] + dp[i][j + 1] + C)
        if i != 0:
            dp[i][j] = min(dp[i][j], dp[i - 1][j] + C)
        if j != W - 1:
            dp[i][j] = min(dp[i][j], dp[i][j + 1] + C)

print(result)
```
