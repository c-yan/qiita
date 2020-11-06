# yukicoder contest 273 参戦記

## [A 1279 Array Battle](https://yukicoder.me/problems/no/1279)

a<sub>i</sub>を降順にソートし、b<sub>i</sub>を昇順にソートして出した時の得点が常に最大値ではありそうだが、同じ最大値のものが複数ある場合いくつあるかを解く方法がなかなか思いつかない. よくよく考えると N≦9 なので N!≒3.6×10<sup>5</sup> なので、全部試すことができた.

```python
from itertools import permutations

N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

d = {}
for x in permutations(a):
    t = 0
    for i in range(N):
        t += max(x[i] - b[i], 0)
    d.setdefault(t, 0)
    d[t] += 1
print(d[max(d)])
```

## [B 1280 Beyond C](https://yukicoder.me/problems/no/1280)

確率はN×Mの組み合わせのうちCを超えている組み合わせの数をN×Mで割ったもの. b をソートすれば、ある a<sub>i</sub> との組み合わせでCを超える b<sub>i</sub> の数はにぶたんで *O*(log<i>M</i>) で求まるので、*O*((*N*+*M*)log<i>M</i>) となり解ける. a をソートしてもよい.

```python
from bisect import bisect_left

N, M, C = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

b.sort()
c = 0
for x in a:
    t = ((C + 1) + x - 1) // x
    c += M - bisect_left(b, t)
print(c / (N * M))
```

## [D 1282 Display Elements](https://yukicoder.me/problems/no/1282)

後に出したほうが大得点のチャンスなので、a を昇順に出していくのが最大値になりそう. 累積されていく b から a<sub>i</sub> より小さい枚数を求めるのににぶたんを使おうと思うと、ソートを維持しないといけなくてナイーブに毎回ソートすると *O*(*N*<sup>2</sup>log<i>N</i>) となり当然 TLE. ソート済配列を小さいのと大きいのの2つに分けて、毎回ソートするのは小さい方だけにして計算量を下げてやれば AC できる.

```python
from bisect import bisect_left

N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

a.sort()
t0 = []
t1 = []
result = 0
for i in range(N):
    result += bisect_left(t0, a[i])
    t1.append(b[i])
    t1.sort()
    result += bisect_left(t1, a[i])
    if len(t1) == 300:
        t0.extend(t1)
        t0.sort()
        t1.clear()
print(result)
```
