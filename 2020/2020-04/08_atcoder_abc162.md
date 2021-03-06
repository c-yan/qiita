# AtCoder Beginner Contest 162 参戦記

## [ABC162A - Lucky 7](https://atcoder.jp/contests/abc162/tasks/abc162_a)

2分で突破. 書くだけ.

```python
N = int(input())

if str(N).count('7') > 0:
    print('Yes')
else:
    print('No')
```

## [ABC162C - Sum of gcd of Tuples (Easy)](https://atcoder.jp/contests/abc162/tasks/abc162_c)

4分で突破. なぜか間違って先にC問題やってた. ジャッジシステムが詰まってて、オンライン実行が使えなくて時間がかかった. まあ、問題文通りに書くだけなんだけど、TLE になりそうだったので定数倍に手を入れた.

```python
def main():
    from math import gcd

    K = int(input())

    result = 0
    for a in range(1, K + 1):
        for b in range(1, K + 1):
            t = gcd(a, b)
            for c in range(1, K + 1):
                result += gcd(t, c)
    print(result)


main()
```

## [ABC162B - FizzBuzz Sum](https://atcoder.jp/contests/abc162/tasks/abc162_b)

2分半で突破. Fizz やら Buzz やら FizzBuzz やらのとき以外加算するだけ.

```python
N = int(input())

result = 0
for i in range(1, N + 1):
    if i % 3 == 0 or i % 5 == 0:
        continue
    result += i
print(result)
```

## [ABC162D - RGB Triplets](https://atcoder.jp/contests/abc162/tasks/abc162_d)

24分で突破. TLE1. N≤4000 だから2重ループなんだろうなと思った. ナイーブにやると3重になるが、累積和できるので特に難しくなかった.

```python
def main():
    N = int(input())
    S = input()

    rcs = [0] * N
    gcs = [0] * N
    bcs = [0] * N

    for i in range(N):
        if S[i] == 'R':
            rcs[i] = 1
        elif S[i] == 'G':
            gcs[i] = 1
        elif S[i] == 'B':
            bcs[i] = 1

    for i in range(1, N):
        rcs[i] += rcs[i - 1]
        gcs[i] += gcs[i - 1]
        bcs[i] += bcs[i - 1]

    result = 0
    for i in range(N):
        if S[i] == 'R':
            for j in range(i + 1, N):
                if S[j] == 'R':
                    continue
                elif S[j] == 'G':
                    result += bcs[N - 1] - bcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'B':
                        result -= 1
                elif S[j] == 'B':
                    result += gcs[N - 1] - gcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'G':
                        result -= 1
        elif S[i] == 'G':
            for j in range(i + 1, N):
                if S[j] == 'G':
                    continue
                elif S[j] == 'R':
                    result += bcs[N - 1] - bcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'B':
                        result -= 1
                elif S[j] == 'B':
                    result += rcs[N - 1] - rcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'R':
                        result -= 1
        elif S[i] == 'B':
            for j in range(i + 1, N):
                if S[j] == 'B':
                    continue
                elif S[j] == 'R':
                    result += gcs[N - 1] - gcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'G':
                        result -= 1
                elif S[j] == 'G':
                    result += rcs[N - 1] - rcs[j]
                    t = 2 * j - i
                    if t < N and S[t] == 'R':
                        result -= 1
    print(result)


main()
```

## [ABC162E - Sum of gcd of Tuples (Hard)](https://atcoder.jp/contests/abc162/tasks/abc162_e)

突破できず. 解けると思ったんだけどなあ. 悔しい.

追記: f(k, n) を 1 以上 k 以下の整数からなる長さ n の数列 {A<sub>1</sub>,...,A<sub>n</sub>} で gcd(A<sub>1</sub>,...,A<sub>n</sub>) が 1 の個数とすると求める答えは ∑ <sub>i = 1..K</sub> f(floor(K / i), N) * i となる.

```python
N, K = map(int, input().split())

MOD = 10 ** 9 + 7

cache = {}
def f(k, n):
    if k == 1:
        return 1
    if k in cache:
        return cache[k]
    result = pow(k, n, MOD)
    for i in range(2, k + 1):
        result -= f(k // i, n)
        result %= MOD
    cache[k] = result
    return result


result = 0
for i in range(1, K + 1):
    result += f(K // i, N) * i
    result %= MOD
print(result)
```

解説 PDF の想定解はこっちかな.

```python
N, K = map(int, input().split())

MOD = 10 ** 9 + 7

c = [0] * (K + 1)
for i in range(K, 0, -1):
    t = pow(K // i, N, MOD)
    for j in range(2, K // i + 1):
        t -= c[i * j]
        t %= MOD
    c[i] = t

result = 0
for i in range(1, K + 1):
    result += c[K // i] * i
    result %= MOD
print(result)
```
