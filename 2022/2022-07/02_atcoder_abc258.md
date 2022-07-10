# AtCoder Beginner Contest 258 参戦記

気づいた時点で21時10分になっていて unrated になった.

## [ABC258A - Last Two Digits](https://atcoder.jp/contests/abc258/tasks/abc258_a)

書くだけ.

```python
K = int(input())

x = 21 * 60 + K
print('%02d:%02d' % (x // 60, x % 60))
```

## [ABC258B - Number Box](https://atcoder.jp/contests/abc258/tasks/abc258_b)

書くだけ. 流石に実装がアレすぎるとは思う.

```python
N = int(input())
A = [list(map(int, input())) for _ in range(N)]

result = 0
for y in range(N):
    for x in range(N):
        t = 0
        for i in range(N):
            t = t * 10 + A[(y - i + N) % N][x]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[(y - i + N) % N][(x + i) % N]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[y][(x + i) % N]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[(y + i) % N][(x + i) % N]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[(y + i) % N][x]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[(y + i) % N][(x - i + N) % N]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[y][(x - i + N) % N]
        result = max(result, t)

        t = 0
        for i in range(N):
            t = t * 10 + A[(y - i + N) % N][(x - i + N) % N]
        result = max(result, t)
print(result)
```

## [ABC258C - Rotation](https://atcoder.jp/contests/abc258/tasks/abc258_c)

開始位置が変わっているだけとみなせば良い.

```python
from sys import stdin

readline = stdin.readline

N, Q = map(int, readline().split())
S = readline()[:-1]

i = 0
result = []
for _ in range(Q):
    t, x = map(int, readline().split())
    if t == 1:
        i -= x
    elif t == 2:
        result.append(S[(i + x - 1) % len(S)])
print(*result, sep='\n')
```

## [ABC258D - Trophy](https://atcoder.jp/contests/abc258/tasks/abc258_d)

WA4. どこかの面まで進めて、ストーリースキップしてクリアしまくるのが最短. どの面がいいかはわからないけど、全て試すのが $O(N)$ で実装できるので問題なし. クリアしたあとのステージを無視しなくてボロボロ.

```python
from sys import stdin

readline = stdin.readline

result = 10 ** 20
t = 0
N, X = map(int, readline().split())
for i in range(min(X, N)):
    A, B = map(int, readline().split())
    result = min(result, t + A + B * (X - i))
    t += A + B
print(result)
```
