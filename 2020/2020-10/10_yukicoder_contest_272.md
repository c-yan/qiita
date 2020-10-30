# yukicoder contest 272 (Weird math contest) 参戦記

最近数学ばっかりなのなんでなんだ…….

## [A 1271 初めての級数](https://yukicoder.me/problems/no/1271)

式のとおりに実装すれば通った.

```python
k = int(input())

print(sum(1 / (n * (n + k)) for n in range(1, 10 ** 6)))
```

## [B 1272 珍しい級数](https://yukicoder.me/problems/no/1272)

式のとおりに実装すれば通った.

```python
from math import sin

k = int(input())

print(sum(sin(k * n) / (n ** n) for n in range(1, 100)))
```

## [C 1273 はじめのζ関数](https://yukicoder.me/problems/no/1273)

testcase_20 だけ通らず試合終了.
