# yukicoder contest 333 途中参戦記

## [A 1850 Rewrite Product](https://yukicoder.me/problems/no/1850)

A = X - 1, B = 1 とすれば N を1減らす操作は常に行えるため、N を Y 以上の値にできれば必ず N = Y にすることができる. N が4より大きければ、半々に分ければ値を無限に大きくできる. (例えば A = 3, B = 2 として N = 5 から6に遷移し、A = 3, B = 3 として N = 9 に遷移できる). 以上を踏まえれば、`Y <= X or X > 4` で判定できることが分かる.

```python
from sys import stdin

readline = stdin.readline

T = int(readline())
for _ in range(T):
    X, Y = map(int, readline().split())
    if Y <= X or X > 4:
        print('Yes')
    else:
        print('No')
```
