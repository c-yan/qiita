# yukicoder contest 293 参戦記

## [A 1491 銀将](https://yukicoder.me/problems/no/1491)

さっぱり分からないので、まずはナイーブなコードを書いてみる.

```python
from itertools import product

def f(n):
    a = [(-1, -1), (0, -1), (1, -1), (-1, 1), (1, 1)]
    result = set()
    for i in range(n + 1):
        for p in product(a, repeat=i):
            x, y = 0, 0
            for xx, yy in p:
                x += xx
                y += yy
            result.add((x, y))
    return len(result)
```

実際に実行してみる.

```
>>> f(0)
1
>>> f(1)
6
>>> f(2)
18
>>> f(3)
38
>>> f(4)
66
>>> f(5)
102
>>> f(6)
146
>>> f(7)
198
>>> f(8)
258
>>> f(9)
326
```

どうやら *f*(*n*) = *f*(*n*-1) + (*f*(*n*-1) - *f*(*n*-2) + 8) になっているように見える. [Wolfram|Alpha](https://ja.wolframalpha.com/) に `f(n)=f(n-1)+(f(n-1)-f(n-2)+8), f(2)=18, f(3)=38` を入力したら *f*(*n*) = 4*n*<sup>2</sup>+2 と教えてくれたので解けた.

```python
K = int(input())

if K == 0:
    print(1)
else:
    print(4 * K * K + 2)
```
