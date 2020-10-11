# AtCoder Regular Contest 105 参戦記

## [ARC105A - Fourtune Cookies](https://atcoder.jp/contests/arc105/tasks/arc105_a)

3分で突破. 全部試せば良い.

```python
A, B, C, D = map(int, input().split())

s = sum([A, B, C, D])
for x in [A, B, C, D, A + B, A + C, A + D, B + C, B + D, C + D]:
    if x == s - x:
        print('Yes')
        break
else:
    print('No')
```

## [ARC105B - MAX-=min](https://atcoder.jp/contests/arc105/tasks/arc105_b)

10分で突破. 言うまでもなくナイーブにシミュレートしたりすると、最大値を優先度付きキューとかで高速に取り出せたとしても、最小値 x = 1 とかの時に TLE 必死. かといって、余りを使ってシミュレートしても TLE する、鬼か.

この計算って実は最大公約数を求める問題だと気づければ、以下で解ける.

```python
from math import gcd
from functools import reduce

N, *a = map(int, open(0).read().split())

print(reduce(gcd, a))
```

## [ARC105C - Camels and Bridge](https://atcoder.jp/contests/arc105/tasks/arc105_c)

敗退. 終了10分前くらいに入出力例が通って「やった!」と思ったけど、半分くらいWA、TLE 2つという感じで…….
