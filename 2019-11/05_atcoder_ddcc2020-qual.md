# AtCoder DISCO presents ディスカバリーチャンネル コードコンテスト2020 予選 参戦記

## A - DDCC Finals

4分で突破. 書くだけ. ARC の A 問題ってこんなに簡単だっけ?

```python
X, Y = map(int, input().split())


def f(n):
    if n == 3:
        return 100000
    elif n == 2:
        return 200000
    elif n == 1:
        return 300000
    else:
        return 0


result = f(X) + f(Y)
if X == 1 and Y == 1:
    result += 400000
print(result)
```

## B - Iron Bar Cutting

26分で突破. WA 1個.

切れ目の位置を p、棒の長さを l とすると、p が真ん中より左にある場合は、左にその長さの差である (l - p) - p = l - 2p 足すか、右を l - 2p 減らす必要がある. 同様に p が真ん中より右にある場合は、右に 2p - l 足すか、左を 2p - l 減らす必要がある. つまるところ |2p - l| のコストとなり、すべての切れ目で必要なコストを求めて最小値を取ればいい.

```python
N = int(input())
A = list(map(int, input().split()))

a = A[:-1]
for i in range(1, N - 1):
    a[i] += a[i - 1]

l = sum(A)
print(min(abs(p + p - l) for p in a))
```

最初全部計算せず、真ん中の2つを計算するように書いたせいで WA を食らった……. 計算量的に問題ないんだから素直に全部計算すればよかった.

## C - Strawberry Cakes

突破できず. 終了12分半後にAC.

いちごをスタート地点として、ぶつからない限り左、上、右に領域を広げ、最後に下に残ったところは上をコピーという方針.

```python
H, W, K = map(int, input().split())
s = [input() for _ in range(H)]

a = [[-1] * W for _ in range(H)]

sbs = []
n = 1
for i in range(H):
    si = s[i]
    for j in range(W):
        if si[j] == '#':
            a[i][j] = n
            n += 1
            sbs.append((i, j))

for sb in sbs:
    i, j = sb
    n = a[i][j]
    t, l = i - 1, j - 1
    r = j + 1
    while t > -1:
        if a[t][j] != -1:
            break
        t -= 1
    t += 1
    while l > -1:
        if a[i][l] != -1:
            break
        l -= 1
    l += 1
    while r < W:
        if a[i][r] != -1:
            break
        r += 1
    r -= 1
    for y in range(t, i + 1):
        ay = a[y]
        for x in range(l, r + 1):
            ay[x] = n

for h in range(H - 1, -1, -1):
    if a[h][0] != -1:
        break
h += 1

for i in range(h, H):
    ai1 = a[i - 1]
    ai = a[i]
    for j in range(W):
        ai[j] = ai1[j]

for i in range(H):
    print(*a[i])
```
