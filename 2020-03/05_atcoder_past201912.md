# AtCoder 第一回 アルゴリズム実技検定 バーチャル参戦記

ようやく時間が取れたので PAST をバーチャル参加してみました. 結果は64点だったので中級(60-79点)となりました. 普段の AtCoder の数学的、ひらめき勝負とは違い、なんか実装が微妙に面倒くさい感じの問題が多く、ちょっと毛色が違うように感じました.

## [past201912A - 2 倍チェック](https://atcoder.jp/contests/past201912-open/tasks/past201912_a)

3分半で突破. 数字だけかをチェックして、OK だったら int に変換して2倍で出力するだけ.

```python
S = input()

for i in range(3):
    if S[i] not in '0123456789':
        print('error')
        exit()
print(int(S) * 2)
```

## [past201912B - 増減管理](https://atcoder.jp/contests/past201912-open/tasks/past201912_b)

7分で突破. 一つ前の値と比較して出力するだけ.

```python
N = int(input())
A = [int(input()) for _ in range(N)]

prev = A[0]
for i in range(1, N):
    if A[i] == prev:
        print('stay')
    elif A[i] < prev:
        print('down %d' % (prev - A[i]))
    elif A[i] > prev:
        print('up %d' % (A[i] - prev))
    prev = A[i]
```

## [past201912C - 3 番目](https://atcoder.jp/contests/past201912-open/tasks/past201912_c)

2分で突破. 降順ソートして、前から3番目の値を出力するだけ.

```python
ABCDEF = list(map(int, input().split()))

ABCDEF.sort(reverse=True)
print(ABCDEF[2])
```

## [past201912D - 重複検査](https://atcoder.jp/contests/past201912-open/tasks/past201912_d)

11分で突破. 辞書で出現数を数えて、0個のものと2個の物を特定して出力するだけ.

```python
N = int(input())
A = [int(input()) for _ in range(N)]

t = set(A)
if len(t) == N:
    print('Correct')
    exit()

d = {}
for i in range(N):
    if A[i] in d:
        d[A[i]] += 1
    else:
        d[A[i]] = 1

for i in range(1, N + 1):
    if i not in d:
        x = i
    elif d[i] == 2:
        y = i

print(y, x)
```

## [past201912E - SNS のログ](https://atcoder.jp/contests/past201912-open/tasks/past201912_e)

24分で突破. フォローフォローの実装が、処理中に増えたフォローのフォローしているユーザも追加してしまっていて、入力例1が通らずに時間がかかった. 入力例1がそこを引っ掛けてくれなければもっと時間がかかったと思う.

```python
N, Q = map(int, input().split())

t = [['N'] * N for _ in range(N)]
for _ in range(Q):
    S = input()
    if S[0] == '1':
        _, a, b = map(int, S.split())
        t[a - 1][b - 1] = 'Y'
    elif S[0] == '2':
        _, a = map(int, S.split())
        for i in range(N):
            if t[i][a - 1] == 'Y':
                t[a - 1][i] = 'Y'
    elif S[0] == '3':
        _, a = map(int, S.split())
        for i in [i for i in range(N) if t[a - 1][i] == 'Y']:
            for j in range(N):
                if t[i][j] == 'Y' and j != a - 1:
                    t[a - 1][j] = 'Y'

for i in range(N):
    print(''.join(t[i]))
```

## [past201912F - DoubleCamelCase Sort](https://atcoder.jp/contests/past201912-open/tasks/past201912_f)

7分で突破. 特に難しいところはなく、単語に切り出して、大文字小文字無視ソートをして、結合するだけ.

```python
S = input()

t = []
start = -1
for i in range(len(S)):
    if S[i].isupper():
        if start == -1:
            start = i
        else:
            t.append(S[start:i+1])
            start = -1
t.sort(key=str.lower)
print(''.join(t))
```

## [past201912G - 組分け](https://atcoder.jp/contests/past201912-open/tasks/past201912_g)

20分で突破. 最初分からないと思ったが、よく見ると N≦10 だったので総当りで行けた. グループが3つあるので bit 全探索できず、int 配列で繰り上げも実装だったけど、特に難しくはなかった.

```python
N = int(input())
a = [list(map(int, input().split())) for _ in range(N - 1)]

result = -float('inf')
t = [0] * N
for _ in range(3 ** N):
    s = 0
    for i in range(N):
        for j in range(i + 1, N):
            if t[i] == t[j]:
                s += a[i][j - i - 1]
    result = max(result, s)
    for i in range(N):
        if t[i] < 2:
            t[i] += 1
            break
        t[i] = 0
print(result)
```

## [past201912H - まとめ売り](https://atcoder.jp/contests/past201912-open/tasks/past201912_h)

44分半で突破. 制約を見るからにセット販売も全種類販売も素直に配列に反映すると TLE 必至なので、それぞれ用の累積変数を作って、辻褄合わせをする方針で通した. 全種類販売のときの `odd_min -= a` が漏れて気づくまでに結構時間がかかってしまった.

```python
N = int(input())
C = list(map(int, input().split()))
Q = int(input())

all_min = min(C)
odd_min = min(C[::2])
all_sale = 0
odd_sale = 0
ind_sale = 0
for _ in range(Q):
    S = input()
    if S[0] == '1':
        _, x, a = map(int, S.split())
        t = C[x - 1] - all_sale - a
        if x % 2 == 1:
            t -= odd_sale
        if t >= 0:
            ind_sale += a
            C[x - 1] -= a
            all_min = min(all_min, t)
            if x % 2 == 1:
                odd_min = min(odd_min, t)
    elif S[0] == '2':
        _, a = map(int, S.split())
        if odd_min >= a:
            odd_sale += a
            odd_min -= a
            all_min = min(odd_min, all_min)
    elif S[0] == '3':
        _, a = map(int, S.split())
        if all_min >= a:
            all_sale += a
            all_min -= a
            odd_min -= a
print(all_sale * N + odd_sale * ((N + 1) // 2) + ind_sale)
```

## [past201912I - 部品調達](https://atcoder.jp/contests/past201912-open/tasks/past201912_i)

14分で突破. 見るからに DP なので特に難しいことはなく. Yを1、Nを0と各桁のビットにみなした整数に変換して、DP するだけ.

```python
N, M = map(int, input().split())


def conv(S):
    result = 0
    for c in S:
        result *= 2
        if c == 'Y':
            result += 1
    return result


dp = [-1] * 2001
dp[0] = 0
for _ in range(M):
    S, C = input().split()
    S = conv(S)
    C = int(C)
    for i in range(2000, -1, -1):
        if dp[i] == -1:
            continue
        if dp[i | S] == -1 or dp[i | S] > dp[i] + C:
            dp[i | S] = dp[i] + C
print(dp[(1 << N) - 1])
```

## [past201912J - 地ならし](https://atcoder.jp/contests/past201912-open/tasks/past201912_j)

突破できず. 左下から右下への最安ルートを DP で求め、バックトラックして通ったところを費用0に書き換えて、右下から右上への最安ルートを DP で求め、それぞれでかかった費用を合計するという実装を組んでみたが、半分くらい WA.

追記: 解説通りに解いた.

```python
INF = float('inf')


def cost_from(H, W, A, y0, x0):
    dp = [[INF] * W for _ in range(H)]
    dp[y0][x0] = 0
    q = [(y0, x0)]
    while q:
        y, x = q.pop(0)
        t = dp[y][x]
        if y - 1 >= 0:
            u = t + A[y - 1][x]
            if dp[y - 1][x] > u:
                dp[y - 1][x] = u
                q.append((y - 1, x))
        if y + 1 < H:
            u = t + A[y + 1][x]
            if dp[y + 1][x] > u:
                dp[y + 1][x] = t + A[y + 1][x]
                q.append((y + 1, x))
        if x - 1 >= 0:
            u = t + A[y][x - 1]
            if dp[y][x - 1] > u:
                dp[y][x - 1] = u
                q.append((y, x - 1))
        if x + 1 < W:
            u = t + A[y][x + 1]
            if dp[y][x + 1] > u:
                dp[y][x + 1] = u
                q.append((y, x + 1))
    return dp


H, W = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(H)]

result = INF
cost1 = cost_from(H, W, A, H - 1, 0)
cost2 = cost_from(H, W, A, H - 1, W - 1)
cost3 = cost_from(H, W, A, 0, W - 1)
for i in range(H):
    for j in range(W):
        t = cost1[i][j] + cost2[i][j] + cost3[i][j] - 2 * A[i][j]
        if t < result:
            result = t
print(result)
```

## [past201912K - 巨大企業](https://atcoder.jp/contests/past201912-open/tasks/past201912_k)

突破できず. 当然ナイーブな実装は書けるわけだけど、計算量を減らす手段が全く思いつかず.

追記: オイラーツアーを学習して AC しました.

```python
N = int(input())

root = -1
children = [[] for _ in range(N + 1)]
left = [0] * (N + 1)
right = [0] * (N + 1)

for i in range(1, N + 1):
    p = int(input())
    if p == -1:
        root = i
    else:
        children[p].append(i)

i = 0
s = [root]
while s:
    n = s.pop()
    if n > 0:
        left[n] = i
        i += 1
        s.append(-n)
        for c in children[n]:
            s.append(c)
    else:
        right[-n] = i
        i += 1

Q = int(input())
for _ in range(Q):
    a, b = map(int, input().split())
    if left[b] <= left[a] <= right[b]:
        print('Yes')
    else:
        print('No')
```

ダブリングでも解いてみました.

```python
from math import log
from collections import deque
from sys import stdin

readline = stdin.readline

N = int(readline())

root = -1
children = [[] for _ in range(N + 1)]
parent = [[-1] * (N + 1) for _ in range(18)]

for i in range(1, N + 1):
    p = int(readline())
    parent[0][i] = p
    if p == -1:
        root = i
    else:
        children[p].append(i)

for i in range(1, 18):
    parenti1 = parent[i - 1]
    parenti = parent[i]
    for j in range(1, N + 1):
        t = parenti1[j]
        if t == -1:
            parenti[j] = -1
        else:
            parenti[j] = parenti1[t]

depth = [-1] * (N + 1)
q = deque([(root, 0)])
while q:
    n, d = q.pop()
    depth[n] = d
    for c in children[n]:
        q.append((c, d + 1))

Q = int(input())
result = []
for _ in range(Q):
    a, b = map(int, readline().split())
    if depth[a] <= depth[b]:
        result.append('No')
        continue
    while depth[a] != depth[b]:
        t = int(log(depth[a] - depth[b], 2))
        a = parent[t][a]
    if a == b:
        result.append('Yes')
    else:
        result.append('No')
print('\n'.join(result))
```
