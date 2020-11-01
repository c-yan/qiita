# AtCoder Beginner Contest 181 参戦記

## [ABC181A - Heavy Rotation](https://atcoder.jp/contests/abc181/tasks/abc181_a)

1分で突破. 書くだけ.

```python
N = int(input())

if N % 2 == 0:
    print('White')
else:
    print('Black')
```

## [ABC181B - Trapezoid Sum](https://atcoder.jp/contests/abc181/tasks/abc181_b)

4分で突破. A以上B以下の合計を求める式が出てこなかったせいで、Bまでの合計からA-1までの合計を引く式に直したりしてたら時間がかかった.

```python
N = int(input())

result = 0
for _ in range(N):
    A, B = map(int, input().split())
    result += B * (B + 1) // 2
    result -= (A - 1) * A // 2
print(result)
```

落ち着いたら式がちゃんと出てきた(笑).

```python
N = int(input())

result = 0
for _ in range(N):
    A, B = map(int, input().split())
    result += (A + B) * (B - A + 1) // 2
print(result)
```

## [ABC181C - Collinearity](https://atcoder.jp/contests/abc181/tasks/abc181_c)

19分で突破. まず2点を通る直線の式をググるところからスタート. N≤10<sup>2</sup> だから *O*(*N*<sup>3</sup>) でも OK と気づくまでに少し時間を要した. 入出力例で0の除算が出て x=a タイプの直線を思い出す(笑). 微妙に式が間違っていたものの、運良く入出力例で拾ってもらえて直すことができ、ノーミスで AC.

```python
N = int(input())
xy = [tuple(map(int, input().split())) for _ in range(N)]

for i in range(N - 1):
    xi, yi = xy[i]
    for j in range(i + 1, N):
        xj, yj = xy[j]
        for k in range(j + 1, N):
            x, y = xy[k]
            if xj == xi:
                if x == xi:
                    print('Yes')
                    exit()
            elif yj == yi:
                if y == yi:
                    print('Yes')
                    exit()
            elif abs(y - (yj - yi) / (xj - xi) * (x - xi) - yi) < 0.000001:
                print('Yes')
                exit()
print('No')
```

## [ABC181D - Takahashi Unevolved](https://atcoder.jp/contests/abc181/tasks/abc181_d)

13分半で突破. 1000以上の桁は自動的に8の倍数になると気づけば即そこで終わりの問題ではある. で、下のコードで AC できたけど、なんかこれバグがあるような. 3桁以上の時に '008' とか '016' とかを木にしてないし、4桁で余った文字が0のパターンのチェックとかもしていない. と思ったけど、問題の制限でそもそも0は存在しなかったか、セーフ.

```python
from collections import Counter

S = input()

if len(S) == 1:
    if S == '8':
        print('Yes')
    else:
        print('No')
elif len(S) == 2:
    if ''.join(sorted(S)) in ['16', '24', '23', '04', '48', '56', '46', '27', '08', '88', '69']:
        print('Yes')
    else:
        print('No')
else:
    c = Counter(S)
    for x in range(104, 1000, 8):
        d = Counter(str(x))
        for k in d:
            if k not in c:
                break
            if d[k] > c[k]:
                break
        else:
            print('Yes')
            exit()
    print('No')
```

## [ABC181E - Traveling Salesman among Aerial Cities](https://atcoder.jp/contests/abc181/tasks/abc181_e)

36分半で突破. WA2. 先生とのペアを一人づつ試していくしかない. 試したあと合計を毎回計算すると *O*(*N*<sup>2</sup>) になって TLE するので、前後から累積和をとっておけば *O*(*N*) にできるなと. 先生のベストな変身形態は均しで *O*(1) でも求めれるなとは思ったもののめんどくさいので *O*(log<i>N</i>) でサボって、トータルで *O*(<i>N</i>log<i>N</i>) になった. 実装のカロリーは高いけど難しくはなかった.

```python
from itertools import accumulate
from bisect import bisect_left
from collections import Counter

INF = float('inf')

N, M = map(int, input().split())
H = list(map(int, input().split()))
W = list(map(int, input().split()))

W.sort()

c = Counter(H)
h = sorted(k for k in c if c[k] % 2 == 1)
n = len(h)

l = list(accumulate(h[i + 1] - h[i] for i in range(0, n - 1, 2)))
r = list(accumulate(h[i] - h[i - 1] for i in range(n - 1, 0, -2)))

result = INF
for i in range(n):
    t = 0
    a = i // 2 - 1
    if a != -1:
        t += l[a]
    a = (n - 1 - i) // 2 - 1
    if a != -1:
        t += r[a]
    j = bisect_left(W, h[i])
    u = INF
    if j != M:
        u = min(u, W[j] - h[i])
    if j != 0:
        u = min(u, h[i] - W[j - 1])
    t += u
    if i % 2 == 1:
        t += h[i + 1] - h[i - 1]
    result = min(result, t)
print(result)
```
