# yukicoder contest 304 参戦記

## [A 1603 Manhattan Social Distance](https://yukicoder.me/problems/no/1603)

*N*=2だと(1, 1) と (H, W) に置くのが最大だなあとなり、*N*=3だと3個目はやっぱり (1, 1) か (H, W) に置くよなあの辺りで、(1, 1) と (H, W) に交互に置けばいいのかと分かる,

```python
N, H, W = map(int, input().split())

print((H + W - 2) * ((N + 1) // 2 * N // 2))
```

## [B 1604 Swap Sort:ONE](https://yukicoder.me/problems/no/1604)

答えは転倒数(蟻本では反転数)なので、過去のコードを貼る.

```python
class BinaryIndexedTree:
    def __init__(self, size):
        self._data = [0] * size

    def add(self, i, x):
        data = self._data
        i += 1
        n = len(data)
        while i <= n:
            data[i - 1] += x
            i += i & -i

    def _sum(self, stop):
        data = self._data
        result = 0
        i = stop
        while i > 0:
            result += data[i - 1]
            i -= i & -i
        return result

    def range_sum(self, start, stop):
        return self._sum(stop) - self._sum(start)


N, *P = map(int, open(0).read().split())

bit = BinaryIndexedTree(N + 1)

result = 0
for x in P:
    result += bit.range_sum(x + 1, N + 1)
    bit.add(x, 1)
print(result)
```
