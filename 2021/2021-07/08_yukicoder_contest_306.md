# yukicoder contest 306 参戦記

## [A 1622 三角形の面積](https://yukicoder.me/problems/no/1622)

答えは当然、円に内接する正三角形の面積. ググれば公式がでてくるかもしれないが、2次元なので答えは半径の二乗に比例するので、サンプルを見れば定数が分かる.

```python
t = int(input())
for _ in range(t):
    r = int(input())
    print(1.299038105676657 * r * r)
```

## [B 1623 三角形の制作](https://yukicoder.me/problems/no/1623)

n は大きくて2重ループは無理だが、r<sub>i</sub>,g<sub>i</sub>,b<sub>i</sub> の値域は2重ループできる程度には小さいのがポイント. OK な条件は r<sub>i</sub>≧g<sub>j</sub> かつ r<sub>i</sub>≧b<sub>k</sub> かつ r<sub>i</sub>＜g<sub>j</sub>+b<sub>k</sub>. ｒの小さい順に処理しながら順次有効なg, bを投入していけば良い. 条件に合う個数を単純に集計する遅くて仕方がないので、BIT で高速化した.

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


n = int(input())
r = list(map(int, input().split()))
g = list(map(int, input().split()))
b = list(map(int, input().split()))

rr = [0] * 3001
for i in r:
    rr[i] += 1

gg = [0] * 3001
for i in g:
    gg[i] += 1

bb = [0] * 3001
for i in b:
    bb[i] += 1

gb = [[] for _ in range(3001)]
for i in range(1, 3001):
    x = gg[i]
    if x == 0:
        continue
    for j in range(1, 3001):
        y = bb[j]
        if y == 0:
            continue
        gb[max(i, j)].append((i << 12) + j)

bit = BinaryIndexedTree(6001)
result = 0
for i in range(1, 3001):
    for v in gb[i]:
        x = v >> 12
        y = v & 4095
        bit.add(x + y, gg[x] * bb[y])
    result += rr[i] * bit.range_sum(i + 1, 6001)
print(result)
```
