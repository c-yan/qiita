# AtCoder Beginner Contest 196 参戦記

誤読で爆死して、8回連続単調増加で記録がストップしました.

## [ABC196A - Difference Max](https://atcoder.jp/contests/abc196/tasks/abc196_a)

1分半で突破. 書くだけ.

```python
a, b = map(int, input().split())
c, d = map(int, input().split())

print(b - c)
```

## [ABC196B - Round Down](https://atcoder.jp/contests/abc196/tasks/abc196_b)

2分で突破. 書くだけ.

```python
X = input()

if '.' in X:
    print(X.split('.')[0])
else:
    print(X)
```

## [ABC196D - Hanjo](https://atcoder.jp/contests/abc196/tasks/abc196_d)

20分くらい?で突破. Cがぱっと見では分からなくて、Dの問題文を眺めたら簡単だったのでこちらへ. HW≤16 なので、総当たりでいいですね.

```python
H, W, A, B = map(int, input().split())

t = [[False] * W for _ in range(H)]


def g(y, x):
    if x + 1 < W:
        return y, x + 1
    return y + 1, 0


def f(y, x, a, b):
    if y == H:
        return a + b == 0

    if t[y][x]:
        ny, nx = g(y, x)
        return f(ny, nx, a, b)

    result = 0
    if a >= 0:
        if x + 1 < W and not t[y][x + 1]:
            t[y][x] = True
            t[y][x + 1] = True
            ny, nx = g(y, x)
            result += f(ny, nx, a - 1, b)
            t[y][x + 1] = False
            t[y][x] = False
        if y + 1 < H and not t[y + 1][x]:
            t[y][x] = True
            t[y + 1][x] = True
            ny, nx = g(y, x)
            result += f(ny, nx, a - 1, b)
            t[y + 1][x] = False
            t[y][x] = False
    if b >= 0:
        t[y][x] = True
        ny, nx = g(y, x)
        result += f(ny, nx, a, b - 1)
        t[y][x] = False
    return result


print(f(0, 0, A, B))
```

## [ABC196C - Doubled](https://atcoder.jp/contests/abc196/tasks/abc196_c)

50分くらい?で突破、RE1、WA2. 何故か123321みたいに回文になっているものと問題を誤読して死んだ. その誤読状態でも入出力例は通るという……. N<10<sup>12</sup> なので前半部はせいぜい最大でも6桁なので、*O*(*N*) ループでも通る.

```python
N = input()

if len(N) % 2 == 1:
    N = '9' * (len(N) - 1)
if len(N) == 0:
    print(0)
    exit()

a = N[:len(N) // 2]
b = int(N)
result = 0
for i in range(1, int(a) + 1):
    if int(str(i) * 2) > b:
        continue
    result += 1
print(result)
```

## [ABC196E - Filters](https://atcoder.jp/contests/abc196/tasks/abc196_e)

突破できず. 問題を解くのにあまり時間が使えなかったので脳筋メモ化再帰で高速に実装しましたが TLE でした. やっぱりそうか…….
