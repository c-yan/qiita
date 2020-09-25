# yukicoder contest 267 参戦記

A も B も星の数詐欺すぎて.

## [A 1236 長針と短針](https://yukicoder.me/problems/no/1236)

12時間の間に長針と短針は11回巡り合う. 当然巡り合うのは12時間の秒数を11で割った秒数毎である. その秒数を求めると、現在時刻の次のその秒数との差が答えとなる.

```python
from bisect import bisect_left

A, B = map(int, input().split())

x = [i * 12 * 60 * 60 // 11 for i in range(12)]
t = (A * 60 + B) % (12 * 60) * 60
print(x[bisect_left(x, t)] - t)
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
