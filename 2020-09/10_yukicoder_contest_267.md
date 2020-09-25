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
