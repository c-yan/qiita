# サイシードプログラミングコンテスト2021 (AtCoder Beginner Contest 219) 参戦記

## [ABC219A - AtCoder Quiz 2](https://atcoder.jp/contests/abc219/tasks/abc219_a)

3分で突破. 書くだけ.

```python
X = int(input())

if 0 <= X < 40:
    print(40 - X)
elif 40 <= X < 70:
    print(70 - X)
elif 70 <= X < 90:
    print(90 - X)
else:
    print('expert')
```

## [ABC219B - Maritozzo](https://atcoder.jp/contests/abc219/tasks/abc219_b)

3分で突破. 書くだけ.

```python
S = {}
S[1] = input()
S[2] = input()
S[3] = input()
T = input()

print(''.join(S[int(c)] for c in T))
```

## [ABC219C - Neo-lexicographic Ordering](https://atcoder.jp/contests/abc219/tasks/abc219_c)

6分半で突破. キーソートするだけ.

```python
X = input()
N = int(input())
S = [input() for _ in range(N)]

t = {}
for i in range(26):
    t[X[i]] = chr(i + ord('a'))

print(*sorted(S, key=lambda s: ''.join(t[c] for c in s)), sep='\n')
```

## [ABC219D - Strange Lunchbox](https://atcoder.jp/contests/abc219/tasks/abc219_d)

29分で突破. DPするだけなんだけど、2次元で2回ダブって取らないようにする方法がぱっと思いつかなくて、1次元化したりしてたら結構時間を溶かしてしまった.

```python
INF = 10 ** 18

N = int(input())
X, Y = map(int, input().split())


def conv(x, y):
    return x * 301 + y


def deconv(n):
    return n // 301, n % 301


dp = [INF] * (301 * 301)
dp[0] = 0
m = 0
for _ in range(N):
    A, B = map(int, input().split())
    for j in range(m, -1, -1):
        if dp[j] == INF:
            continue
        x, y = deconv(j)
        x = min(x + A, 300)
        y = min(y + B, 300)
        k = conv(x, y)
        if dp[k] > dp[j] + 1:
            dp[k] = dp[j] + 1
            m = max(m, k)

result = INF
for j in range(m, -1, -1):
    if dp[j] == INF:
        continue
    x, y = deconv(j)
    if x < X or y < Y:
        continue
    result = min(result, dp[j])

if result == INF:
    print(-1)
else:
    print(result)
```
