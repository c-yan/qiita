# yukicoder contest 274 参戦記

## [A 1285 ゴミ捨て](https://yukicoder.me/problems/no/1285)

小さい順でも大きい順でもいいので一つづつ重ねれるかをチェックして行けばいいだけ.

```python
from heapq import heappush, heapreplace

N, *A = map(int, open(0).read().split())

A.sort()
q = [A[0]]
for a in A[1:]:
    if a <= q[0] + 1:
        heappush(q, a)
    else:
        heapreplace(q, a)
print(len(q))
```

## [B 1286 Stone Skipping](https://yukicoder.me/problems/no/1286)

1回も跳ねないと x、1回跳ねると 3/2 * x、2回跳ねると 7/4 * x、3回跳ねると 15/8 * x、n回跳ねると (2<sup>n+1</sup>-1)/2<sup>n</sup> * x となる. D≦10<sup>18</sup> なので60回も跳ねるとそれ以降は飛距離が伸びなくなる. なので、60回、59回、……、1回跳ねた場合の答えがあるかを調べていけば良い. 切り捨ての影響があるので、適当に前後±100くらいをチェックしたら AC した.

```python
D = int(input())


def f(x):
    result = 0
    while x != 0:
        result += x
        x //= 2
        if result >= D:
            break
    return result


for i in range(60, 0, -1):
    t = D * (2 ** (i - 1)) // (2 ** i - 1)
    for j in range(-100, 100):
        if t + j < 0:
            continue
        if f(t + j) == D:
            print(t + j)
            exit()
```
