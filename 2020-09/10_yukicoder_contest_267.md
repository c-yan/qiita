# yukicoder contest 267 参戦記

## [A 1236 長針と短針](https://yukicoder.me/problems/no/1236)

12時間の間に長針と短針は11回巡り合う. 当然巡り合うのは12時間の秒数を11で割った秒数毎である. その秒数を求めると、現在時刻の次のその秒数との差が答えとなる.

```python
from bisect import bisect_left

A, B = map(int, input().split())

x = [i * 12 * 60 * 60 // 11 for i in range(12)]
t = (A * 60 + B) % (12 * 60) * 60
print(x[bisect_left(x, t)] - t)
```

## [B 1237 EXP Multiple!](https://yukicoder.me/problems/no/1237)

よく読むと「答えを10<sup>9</sup>+7で割った余り」じゃなくて、「答えで10<sup>9</sup>+7を割った余り」かよ orz. 確かにそれなら簡単に解ける. 解いた人数からしてこれに引っ掛かった人が多数だな(笑).

```python
m = 1000000007


def make_factorial_table(n):
    result = [0] * (n + 1)
    result[0] = 1
    for i in range(1, n + 1):
        result[i] = result[i - 1] * i % m
    return result


N, *A = map(int, open(0).read().split())

A.sort()

if A[0] == 0:
    print(-1)
    exit()

if A[-1] >= 4:
    print(m)
    exit()

fac = make_factorial_table(3)

t = 1
for a in A:
    t *= pow(a, fac[a])
    if t > m:
        break
print(m % t)
```

## [C 1238 選抜クラス](https://yukicoder.me/problems/no/1238)

K を引いた辺りで、「あれ、同じような問題を過去に解いてるぞ」と思ったのに解けなかった悲しみ. [ABC044C - 高橋君とカード](https://atcoder.jp/contests/abc044/tasks/arc060_a) と大体同じですね(平均がちょうどXか、平均がX以上かの違い).

a<sub>i</sub>=A<sub>i</sub>-K とすると、a<sub>i</sub>の合計が0以上になる選び方はいくつあるかという問題になる. DP をすれば簡単に求まる.

```python
m = 1000000007

N, K, *A = map(int, open(0).read().split())

for i in range(N):
    A[i] -= K

dp = {}
dp[0] = 1
for a in A:
    for k in sorted(dp, reverse=True) if a >= 0 else sorted(dp):
        dp.setdefault(k + a, 0)
        dp[k + a] += dp[k]

dp[0] -= 1
print(sum(dp[k] for k in dp if k >= 0) % m)
```
