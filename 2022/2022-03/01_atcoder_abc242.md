# AtCoder Beginner Contest 242 参戦記

ジャッジが詰まっていて大変だった上に、Dが解けなくて落緑_(┐「ε:)_

## [ABC242A - T-shirt](https://atcoder.jp/contests/abc242/tasks/abc242_a)

書くだけ. `elif` を `if` と誤記して WA1(泣).

```python
A, B, C, X = map(int, input().split())

if X <= A:
    print(1)
elif X > B:
    print(0)
else:
    print(C / (B - A))
```

## [ABC242B - Minimize Ordering](https://atcoder.jp/contests/abc242/tasks/abc242_b)

書くだけ.

```python
S = input()

print(''.join(sorted(S)))
```

## [ABC242C - 1111gal password](https://atcoder.jp/contests/abc242/tasks/abc242_c)

素直に DP するだけ. C問題で DP はもう普通なんだな.

```python
m = 998244353

N = int(input())

t = [1] * 10
t[0] = 0

for i in range(N - 1):
    u = [0] * 10
    for i in range(1, 10):
        for j in [-1, 0, 1]:
            k = i + j
            if k < 1 or k > 9:
                continue
            u[k] += t[i]
            u[k] %= m
    t = u
print(sum(t) % m)
```
