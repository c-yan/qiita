# yukicoder contest 246 参戦記

## [A 1040 垂直大学](https://yukicoder.me/problems/no/1040)

まあ、90度か270度だけが垂直なので書くだけですね.

```python
N = int(input())

t = N % 360
if t == 90 or t == 270:
    print('Yes')
else:
    print('No')
```

## [B 1041 直線大学](https://yukicoder.me/problems/no/1041)

N≤100 なので、全ての2点の組み合わせを試せばいいだけなので特に悩むことはありませんね. 当然二点を通る直線の方程式なんて忘れてしまっているのでググりました. 通ったかどうかの判定に適当に `< 0.0001` を使ったけど、本来どうするべきだったのかな.

```python
N = int(input())
XY = [list(map(int, input().split())) for _ in range(N)]

result = 0
for i in range(N):
    for j in range(i + 1, N):
        x1, y1 = XY[i]
        x2, y2 = XY[j]
        if x1 == x2:
            result = max(result, len([None for x, y in XY if x == x1]))
            continue
        t = 0
        for k in range(N):
            x, y = XY[k]
            if abs((y2 - y1) / (x2 - x1) * (x - x1) + y1 - y) < 0.0001:
                t += 1
        result = max(result, t)
print(result)
```

## [C 1042 愚直大学](https://yukicoder.me/problems/no/1042)

見るからに、にぶたんですねーとしか言いようがない問題. 安全を取って `> 0.000001` を `> 0.0000001` にすると精度が足りなくて無限ループになるのか TLE になるのがエグい. is_ok のなかでさらっと 10 ** 35 がでてきてるけど平気なのが Python の楽なところですね.

```python
from math import log2


def is_ok(N):
    return N * N <= Q * log2(N) * N + P


P, Q = map(int, input().split())

ok = 1
ng = 10 ** 18
while ng - ok > 0.000001:
    m = (ok + ng) / 2
    if is_ok(m):
        ok = m
    else:
        ng = m

print(ok)
```
