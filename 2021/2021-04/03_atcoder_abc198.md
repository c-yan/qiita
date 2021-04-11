# AtCoder Beginner Contest 198 参戦記

## [ABC198A - Div](https://atcoder.jp/contests/abc198/tasks/abc198_a)

1分で突破. 書くだけ.

```python
N = int(input())

print(N - 1)
```

## [ABC198B - Palindrome with leading zeros](https://atcoder.jp/contests/abc198/tasks/abc198_b)

3分で突破. 書くだけ.

```python
N = input()

def is_palindrome(s):
    for i in range(len(s) // 2):
        if s[i] != s[len(s) - 1 - i]:
            return False
    return True

for i in range(len(N) + 1):
    if is_palindrome('0' * i + N):
        print('Yes')
        break
else:
    print('No')
```

## [ABC198C - Compass Walking](https://atcoder.jp/contests/abc198/tasks/abc198_c)

9分で突破、WA2. 近すぎるときは2回になるのが盲点だった.

```python
from math import ceil, sqrt

R, X, Y = map(int, input().split())

c = sqrt((X * X + Y * Y) / (R * R))
if c < 1:
    print(2)
else:
    print(ceil(c))
```

## [ABC198E - Unique Color](https://atcoder.jp/contests/abc198/tasks/abc198_e)

30分くらい?で突破. DFS でサクッと突破できた.

```python
from sys import setrecursionlimit, stdin

readline = stdin.readline
setrecursionlimit(10 ** 6)

N = int(readline())
C = list(map(int, readline().split()))

links = [[] for _ in range(N)]
for _ in range(N - 1):
    A, B = map(lambda x: int(x) - 1, readline().split())
    links[A].append(B)
    links[B].append(A)

dp = [(10 ** 18, False)] * N
dp[0] = (0, True)

def f(frm, to, steps, colors):
    if steps == dp[to][0]:
        if C[to] not in colors:
            dp[to] = (steps, True)
    elif steps < dp[to][0]:
        dp[to] = (steps, C[to] not in colors)
    colors.setdefault(C[to], 0)
    colors[C[to]] += 1
    for nto in links[to]:
        if nto == frm:
            continue
        f(to, nto, steps + 1, colors)
    colors[C[to]] -= 1
    if colors[C[to]] == 0:
        del colors[C[to]]

f(-1, 0, 0, {})
result = []
for i in range(N):
    if dp[i][1]:
        result.append(i + 1)
print(*result, sep='\n')
```

## [ABC198D - Send More Money](https://atcoder.jp/contests/abc198/tasks/abc198_d)

50分くらい?で突破、WA2TLE3. まずは変数は10個が最大であることに気づく(別の変数は別の値縛りから). 10!=3628800=3.6×10<sup>7</sup>なので、この問題の「実行時間制限: 5 sec」ならなんとか収まりそうと分かる. 実際は PyPy でもギリギリで定数倍削減する間に TLE の山が発生した.

```python
from itertools import permutations

S1 = input()
S2 = input()
S3 = input()

s1 = [ord(c) - 97 for c in S1]
s2 = [ord(c) - 97 for c in S2]
s3 = [ord(c) - 97 for c in S3]

a = sorted(set(s1 + s2 + s3))
if len(a) > 10:
    print('UNSOLVABLE')
    exit()

if len(a) < 10:
    for i in range(30, 30 + (10 - len(a))):
        a.append(i)

b = [0] * 100
l = set([s1[0], s2[0], s3[0]])
for p in permutations(a):
    if p[0] in l:
        continue

    for i in range(10):
        b[p[i]] = chr(i + 48)

    N1 = int(''.join(b[c] for c in s1))
    N2 = int(''.join(b[c] for c in s2))
    N3 = int(''.join(b[c] for c in s3))

    if N1 + N2 == N3:
        print(N1)
        print(N2)
        print(N3)
        exit()
print('UNSOLVABLE')
```
