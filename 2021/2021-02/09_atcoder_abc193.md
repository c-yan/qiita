# キャディプログラミングコンテスト2021(AtCoder Beginner Contest 193) 参戦記

## [ABC193A - Discount](https://atcoder.jp/contests/abc193/tasks/abc193_a)

2分で突破. 書くだけ.

```python
A, B = map(int, input().split())

print(100 - (B / A) * 100)
```

## [ABC193B - Play Snuke](https://atcoder.jp/contests/abc193/tasks/abc193_b)

3分半で突破. 各店舗ごとに売り切れ前にたどり着けるか判定し、売り切れ前にたどり着けた中で一番安いのを出力するだけ.

```python
INF = 10 ** 18

N = int(input())

result = INF
for _ in range(N):
    A, P, X = map(int, input().split())
    if X - A > 0:
        result = min(result, P)
if result == INF:
    print(-1)
else:
    print(result)
```

## [ABC193C - Unexpressed](https://atcoder.jp/contests/abc193/tasks/abc193_c)

8分で突破. 入出力例2でa<sup>b</sup>で表せるものが少ないことが分かるので、全列挙すればいいなと思いました. 最低2乗なので、Nの二乗根まで回せば OK で計算量的にも問題なし.

```python
N = int(input())

s = set()
for i in range(2, int(N ** 0.5) + 1):
    for j in range(2, N + 1):
        t = i ** j
        if t > N:
            break
        s.add(t)
print(N - len(s))
```

## [ABC193D - Poker](https://atcoder.jp/contests/abc193/tasks/abc193_d)

18分半で突破. 81パターンしかないので、何も難しいことはなく、全部計算すれば OK.

```python
def f(cards):
    c = {}
    for i in range(1, 10):
        c[i] = 0
    for i in cards:
        c[i] += 1
    result = 0
    for k in c:
        result += k * pow(10, c[k])
    return result


K = int(input())
S = input()
T = input()

all = {}
for i in range(1, 10):
    all[i] = K

s = list(map(int, S[:4]))
t = list(map(int, T[:4]))

for i in s:
    all[i] -= 1
for i in t:
    all[i] -= 1

x = 9 * K - 8

result = 0
for i in range(1, 10):
    if all[i] == 0:
        continue
    a = all[i] / x
    all[i] -= 1
    k = f(s + [i])
    for j in range(1, 10):
        if all[j] == 0:
            continue
        b = all[j] / (x - 1)
        l = f(t + [j])
        if k > l:
            result += a * b
    all[i] += 1
print(result)
```
