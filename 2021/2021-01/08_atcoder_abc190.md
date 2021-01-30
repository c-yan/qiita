# AtCoder Beginner Contest 190 参戦記

F問題が解けなかったらやばいことになるところだった.

## [ABC190A - Very Very Primitive Game](https://atcoder.jp/contests/abc190/tasks/abc190_a)

3分で突破. 書くだけだが、境界値嫌だなあと思いながら書いた.

```python
A, B, C = map(int, input().split())

if C == 0:
    if B >= A:
        print('Aoki')
    else:
        print('Takahashi ')
elif C == 1:
    if A >= B:
        print('Takahashi ')
    else:
        print('Aoki')
```

## [ABC190B - Magic 3](https://atcoder.jp/contests/abc190/tasks/abc190_b)

3分で突破. ある意味、Aより簡単だなあ.

```python
N, S, D = map(int, input().split())

for _ in range(N):
    X, Y = map(int, input().split())
    if X >= S:
        continue
    if Y <= D:
        continue
    print('Yes')
    break
else:
    print('No')
```

## [ABC190C - Bowls and Dishes](https://atcoder.jp/contests/abc190/tasks/abc190_c)

7分半で突破. C問題のビット全探索何回目かな.

```python
from itertools import product

N, M = map(int, input().split())
AB = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(M)]
K = int(input())
CD = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(K)]

result = 0
for p in product(range(2), repeat=K):
    t = [False] * N
    for i in range(K):
        t[CD[i][p[i]]] = True
    c = 0
    for a, b in AB:
        if t[a] and t[b]:
            c += 1
    result = max(result, c)
print(result)
```

解説の product の使い方がエレガントだった.

```python
from itertools import product

N, M = map(int, input().split())
AB = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(M)]
K = int(input())
CD = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(K)]

result = 0
for p in product(*CD):
    s = set(p)
    c = 0
    for a, b in AB:
        if a in s and b in s:
            c += 1
    result = max(result, c)
print(result)
```

## [ABC190D - Staircase Sequences](https://atcoder.jp/contests/abc190/tasks/abc190_d)

突破できず. 素因数分解する感じだなあというのが見えた辺りで時間切れ.

## [ABC190E - Magical Ornament](https://atcoder.jp/contests/abc190/tasks/abc190_e)

問題文を読んで、これは解けなさそうだとスキップ.

## [ABC190F - Shift and Inversions](https://atcoder.jp/contests/abc190/tasks/abc190_f)

15分で突破、TLE1. [Chokudai SpeedRun 001J - 転倒数](https://atcoder.jp/contests/chokudai_S001/tasks/chokudai_S001_j) をやっててよかった. 3分くらい考えたら答えが閃いた. 実装は簡単で良かった.

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


N, *a = map(int, open(0).read().split())

bit = BinaryIndexedTree(N)

c = 0
for x in a:
    c += bit.range_sum(x + 1, N)
    bit.add(x, 1)

result = [c]
for i in range(N - 1):
    c -= bit.range_sum(0, a[i])
    c += bit.range_sum(a[i] + 1, N)
    result.append(c)
print(*result, sep='\n')
```

最終的に BIT は 0..N-1 が全て1が立っている状態なので、差分を求めるのに BIT は要らなかった.

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


N, *a = map(int, open(0).read().split())

bit = BinaryIndexedTree(N)

c = 0
for x in a:
    c += bit.range_sum(x + 1, N)
    bit.add(x, 1)

result = [c]
for i in range(N - 1):
    c += (N - (a[i] + 1)) - a[i]
    result.append(c)
print(*result, sep='\n')
```
