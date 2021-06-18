# yukicoder contest 300 参戦記

## [A 1550 nullくんの町清掃 / null's Town Cleaning](https://yukicoder.me/problems/no/1550)

問題文の通り、余りを求めればよい.

```python
n = int(input())

print(n % 1000000007)
```

## [B 	1551 	誕生日の三角形 ](https://yukicoder.me/problems/no/1551)

辺の長さが L / 3 の正三角形が答えなのは直感的に分かるので、後は面積の公式をググれば良い.

```python
from math import sqrt

L = int(input())

print(L * L * sqrt(3) / 36)
```
