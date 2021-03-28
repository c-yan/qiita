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

20分くらい?で突破. Cがぱっと見では分からなくて、Dの問題文を眺めたら簡単だったのでこちらへ. HW≤16 なので、再帰関数で全て試せばいいですね.

```python
H, W, A, B = map(int, input().split())

placed = [[False] * W for _ in range(H)]


def proceed(p):
    y, x = p
    if x + 1 < W:
        return (y, x + 1)
    return (y + 1, 0)


def f(p, a, b):
    y, x = p

    if y == H:
        return a + b == 0

    if placed[y][x]:
        return f(proceed(p), a, b)

    result = 0
    if a >= 0:
        # right
        if x + 1 < W and not placed[y][x + 1]:
            placed[y][x] = True
            placed[y][x + 1] = True
            result += f(proceed(p), a - 1, b)
            placed[y][x + 1] = False
            placed[y][x] = False
        # down
        if y + 1 < H and not placed[y + 1][x]:
            placed[y][x] = True
            placed[y + 1][x] = True
            result += f(proceed(p), a - 1, b)
            placed[y + 1][x] = False
            placed[y][x] = False
    if b >= 0:
        placed[y][x] = True
        result += f(proceed(p), a, b - 1)
        placed[y][x] = False
    return result


print(f((0, 0), A, B))
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
        break
    result += 1
print(result)
```

追記: なんでこんなに難しく考えてたんだろうと反省した.

```python
N = int(input())

for i in range(1, 1000001):
    if int(str(i) * 2) <= N:
        continue
    print(i - 1)
    break
```

## [ABC196E - Filters](https://atcoder.jp/contests/abc196/tasks/abc196_e)

突破できず. 問題を解くのにあまり時間が使えなかったので脳筋メモ化再帰で高速に実装しましたが TLE でした. やっぱりそうか…….
