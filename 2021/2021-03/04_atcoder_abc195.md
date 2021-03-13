# パナソニックプログラミングコンテスト (AtCoder Beginner Contest 195) 参戦記

20:58まで気づかずウマ娘をやってたので開始直後30秒くらい準備で無駄にしてしまった.

## [ABC195A - Health M Death](https://atcoder.jp/contests/abc195/tasks/abc195_a)

2分で突破. 書くだけ.

```python
M, H = map(int, input().split())

if H % M == 0:
    print('Yes')
else:
    print('No')
```

## [ABC195C - Comma](https://atcoder.jp/contests/abc195/tasks/abc195_c)

Bが難しかったので先にこちらを. 10分くらい? 一発で答えが合ってびっくりした.

```python
N = int(input())

t = 1
c = 0
while t <= N:
    t *= 1000
    c += 1
t //= 1000
c -= 1

result = 0
while t != 1:
    result += c * (N - t + 1)
    N = t - 1
    t //= 1000
    c -= 1
print(result)
```

## [ABC195B - Many Oranges](https://atcoder.jp/contests/abc195/tasks/abc195_b)

20分くらい?、WA1. B問題とは思えない難しさと思ったけど、解いた後は似たような問題どこかで解いたことあるようなってなった. off by one エラーをしまくった.

```python
A, B, W = map(int, input().split())

w = W * 1000
min_x = (w + B - 1) // B
max_x = w // A
if w < min_x * A or w > max_x * B:
    print('UNSATISFIABLE')
    exit()
print(min_x, max_x)
```

## [ABC195E - Lucky 7 Battle](https://atcoder.jp/contests/abc195/tasks/abc195_e)

Dの解き方がぱっと思いつかなかったが、こちらはぱっと見解けそうだったのでこちらから. 20分くらい? 持ち回さないといけないのは7で割った余りだけなので、メモ化再帰一発ですよね.

```python
from sys import setrecursionlimit
from functools import lru_cache


@lru_cache(maxsize=None)
def f(i, r):
    if i == N:
        return r == 0

    if X[i] == 'A':
        if not f(i + 1, r * 10 % 7):
            return False
        return f(i + 1, (r * 10 + int(S[i])) % 7)
    elif X[i] == 'T':
        if f(i + 1, r * 10 % 7):
            return True
        return f(i + 1, (r * 10 + int(S[i])) % 7)


setrecursionlimit(10 ** 6)

N = int(input())
S = input()
X = input()

if f(0, 0):
    print("Takahashi")
else:
    print("Aoki")
```

## [ABC195D - Shipping Center](https://atcoder.jp/contests/abc195/tasks/abc195_d)

25分くらい?、WA1. 難しく考えすぎてしまってドツボにハマった. 座圧+セグ木で実装してWAを出して、同じサイズの箱が複数あったらダメじゃんってなった後に制約を見て *O*(*NMQ*) でいいやんけーと5分で実装して AC. 数百パフォをドブに投げ込んでしまった…….

```python
N, M, Q = map(int, input().split())
WV = [list(map(int, input().split())) for _ in range(N)]
X = list(map(int, input().split()))

WV.sort(key=lambda x: x[1], reverse=True)
for _ in range(Q):
    L, R = map(lambda x: int(x) - 1, input().split())
    boxes = X[:L] + X[R + 1:]
    boxes.sort()
    result = 0
    for W, V in WV:
        for i in len(boxes):
            if boxes[i] >= W:
                result += V
                boxes = boxes[:i] + boxes[i+1:]
                break
    print(result)
```
