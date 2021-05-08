# 京セラプログラミングコンテスト2021(AtCoder Beginner Contest 200) 参戦記

2回前に3桁パフォを取ったレーティングダウンから回復する前に、また3桁パフォを取ってしまうとは…….

## [ABC200A - Century](https://atcoder.jp/contests/abc200/tasks/abc200_a)

1分半で突破. 書くだけ.

```python
N = int(input())

print((N + 99) // 100)
```

## [ABC200B - 200th ABC-200](https://atcoder.jp/contests/abc200/tasks/abc200_b)

3分で突破. K≤20 なので素直に実行するだけ.

```python
N, K = map(int, input().split())

for _ in range(K):
    if N % 200 == 0:
        N //= 200
    else:
        N *= 1000
        N += 200
print(N)
```

## [ABC200C - Ringo's Favorite Numbers 2](https://atcoder.jp/contests/abc200/tasks/abc200_c)

10分で突破. 似たような問題が過去問にありましたね. 200で割った余りが同じA<sub>i</sub>同士の組み合わせの数の合計.

```python
N, *A = map(int, open(0).read().split())

d = {}
for a in A:
    t = a % 200
    d.setdefault(t, 0)
    d[t] += 1

result = 0
for k in d:
    result += d[k] * (d[k] - 1) // 2
print(result)
```

## [ABC200D - Happy Birthday! 2](https://atcoder.jp/contests/abc200/tasks/abc200_d)

突破できず. バックトラックが無限ループしまくっていた.

```python
N, *A = map(int, open(0).read().split())


def backtrack(n):
    result = []
    while dp[n] is not None:
        pn = n
        result.append(dp[n] + 1)
        n -= A[dp[n]]
        n %= 200
        if n == pn:
            break
    return list(reversed(result))


def finish(B, C):
    print('Yes')
    print(len(B), *B)
    print(len(C), *C)
    exit()


dp = [None] * 200
for i in range(N):
    a = A[i] % 200

    if dp[0] is not None:
        B = backtrack(0) + [i + 1]
        C = [i + 1]
        finish(B, C)

    if dp[a] is not None:
        B = backtrack(a)
        C = [i + 1]
        finish(B, C)

    ndp = dp[:]
    ndp[a] = i
    for j in range(1, 200):
        if dp[j] is None:
            continue
        k = (j + a) % 200
        if dp[k] is not None:
            B = backtrack(k)
            C = backtrack(j) + [i + 1]
            finish(B, C)
        ndp[k] = i
    dp = ndp
print('No')
```
