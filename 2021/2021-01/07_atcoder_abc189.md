# AtCoder Beginner Contest 189 参戦記

終了50秒前に書けたE問題を投げたら奇跡的に一発 AC が出て久々に大勝利!

## [ABC189A - Slot](https://atcoder.jp/contests/abc189/tasks/abc189_a)

1分半で突破. 書くだけ.

```python
S = input()

if S[0] == S[1] and S[1] == S[2]:
    print('Won')
else:
    print('Lost')
```

## [ABC189B - Alcoholic](https://atcoder.jp/contests/abc189/tasks/abc189_b)

3分で突破. double でやるのは嫌な予感がしたので、思考停止で Decimal で. 30秒考えれば100倍すればいいじゃんと気づいただろうに(笑).

```python
from decimal import Decimal

N, X = map(int, input().split())

c = 0
for i in range(N):
    V, P = map(Decimal, input().split())
    c += V * (P / 100)
    if c > X:
        print(i + 1)
        break
else:
    print(-1)
```

みんなXを100倍してたので、天の邪鬼で割り算で(笑).

```python
N, X = map(int, input().split())

c = 0
for i in range(N):
    V, P = map(int, input().split())
    c += V * P
    if (c + 99) // 100 > X:
        print(i + 1)
        break
else:
    print(-1)
```

## [ABC189D - Logical Expression](https://atcoder.jp/contests/abc189/tasks/abc189_d)

20分くらいで突破. C問題に一旦敗退してこちらを先に. 普通に AND のほうが OR より優先順位が高いんだろうと思いつつ問題文を読んだら優先順位が同じで、何この簡単な問題ってなった.

```python
N, *S = open(0).read().split()

t = 1
f = 1
for s in S:
    if s == 'AND':
        f = t + f * 2
    elif s == 'OR':
        t = t * 2 + f
print(t)
```

公式解説も 2<sup>N</sup> が出てきているが、他の人の回答を見ても 2<sup>N</sup> が出てきている人が多いなあ.

```python
N = int(input())
S = [input() for _ in range(N)]


def dfs(n):
    if n == 0:
        return 1

    if S[n - 1] == 'AND':
        return dfs(n - 1)
    elif S[n - 1] == 'OR':
        return 2 ** n + dfs(n - 1)


print(dfs(N))
```

## [ABC189C - Mandarin Orange](https://atcoder.jp/contests/abc189/tasks/abc189_c)

20分くらいで突破、WA1. N≤10<sup>4</sup> だから *O*(*N*<sup>2</sup>) だと駄目だよなと思いつつ、順位表を見ると正当者数が多いので、多分テストケースが制限いっぱいに攻めてないんだろうと投げたら AC が出て一安心. 公式解法も *O*(*N*<sup>2</sup>) で、そもそも制限時間も2秒ではなく、1.5秒だったことに気づいて目が白黒してる.

```python
N, *A = map(int, open(0).read().split())

def f(x):
    result = 0
    i = 0
    j = 0
    while i < N:
        while j < N and x <= A[j]:
            j += 1
        result = max(result, x * (j - i))
        i = j + 1
        j = i
    return result

result = 0
for x in set(A):
    result = max(result, f(x))
print(result)
```

リストの中の最小要素で分割するを繰り返すと非常に高速に解けた. 上の解答は Python では TLE するので PyPy が必要だったが、こちらは Python でも AC する. とはいっても 1 から 10<sup>4</sup> まで順に並べた入力の処理に 3.5s かかるので、C問題のテストケースが弱いだけではある.

```python
from sys import setrecursionlimit

setrecursionlimit(10 ** 6)

N, *A = map(int, open(0).read().split())


def f(a):
    n = len(a)
    m = min(a)
    result = m * n
    i = 0
    for j in range(n):
        if a[j] != m:
            continue
        if i < j:
            result = max(result, f(a[i:j]))
        i = j + 1
    if i < n:
        result = max(result, f(a[i:n]))
    return result


print(f(A))
```

## [ABC189E - Rotate and Flip](https://atcoder.jp/contests/abc189/tasks/abc189_e)

56分で突破. アフィン変換、なんでしたっけ?(まて)

情報学科卒にも関わらずアフィン変換という単語は出てこなかったものの、行列とは気づかずに行列の遷移を書けたおかげで AC できた.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
XY = [tuple(map(int, readline().split())) for _ in range(N)]
M = int(readline())
op = [readline() for _ in range(M)]
Q = int(readline())
AB = [tuple(map(int, readline().split())) for _ in range(Q)]

# (x, y)
# 1 -> (y, -x)
# 2 -> (-y, x)
# 3 p -> (2p - x, y)
# 4 p -> (x, 2p - y)


def e(x, i):
    result = x[0]
    if x[1] == 1:
        result += XY[i][x[2]]
    elif x[1] == -1:
        result -= XY[i][x[2]]
    return result


x = (0, 1, 0)
y = (0, 1, 1)
applied = 0
result = [None] * Q
for a, b, i in sorted(((AB[i][0], AB[i][1], i) for i in range(Q)), key=lambda x: x[0]):
    while applied < a:
        o = op[applied]
        if o[0] == '1':
            t = x
            x = (y[0], y[1], y[2])
            y = (-t[0], -t[1], t[2])
        if o[0] == '2':
            t = x
            x = (-y[0], -y[1], y[2])
            y = (t[0], t[1], t[2])
        if o[0] == '3':
            p = int(o[2:])
            x = (2 * p - x[0], -x[1], x[2])
        if o[0] == '4':
            p = int(o[2:])
            y = (2 * p - y[0], -y[1], y[2])
        applied += 1
    result[i] = '%d %d' % (e(x, b - 1), e(y, (b - 1)))
print(*result, sep='\n')
```
