# AtCoder Beginner Contest 185 参戦記

また全完のチャンスを逃してしまった、悔しい!

## [A - ABC Preparation](https://atcoder.jp/contests/abc185/tasks/abc185_a)

1分で突破. 書くだけ. min を間違えて max と書いて無駄に時間を使った.

```python
A = list(map(int, input().split()))

print(min(A))
```

## [ABC185B - Smartphone Addiction](https://atcoder.jp/contests/abc185/tasks/abc185_b)

7分半で突破. え、これがB問題!?ってなった. とはいっても、off-by-one エラーをしないように考えながら丁寧に書いていくだけではある.

```python
N, M, T = map(int, input().split())

n = N
p = 0
for _ in range(M):
    A, B = map(int, input().split())
    n -= A - p
    if n <= 0:
        print('No')
        exit()
    n = min(N, n + B - A)
    p = B
n -= T - p
if n <= 0:
    print('No')
    exit()
print('Yes')
```

## [ABC185C - Duodecim Ferra](https://atcoder.jp/contests/abc185/tasks/abc185_c)

8分半で突破. <sub>L-1</sub>C<sub>11</sub> を求めるだけなのだが、間違って <sub>L</sub>C<sub>12</sub> で書いて無駄に時間をロストした. ダダっと掛けた後に、ダダっと割って求めたけど、int64 だと溢れちゃうんですかねえ? Python なので何も考えずに解いたけど.

```python
L = int(input())

result = 1
for i in range(11):
    result *= (L - 1) - i
for i in range(1, 12):
    result //= i
print(result)
```

## [ABC185D - Stamp](https://atcoder.jp/contests/abc185/tasks/abc185_d)

22分で突破. WA1. M = 0 のときに0を出力するトンマをやらかした.

白の連続長の最小値が k. 後は各区間ではんこを何回使えばよいかを求めて、その総和が答えとなる. D問題にしては簡単? max を min と書いて時間を無駄にした. 今日は max と min の入れ違いが多いな(笑).

```python
N, M, *A = map(int, open(0).read().split())

if M == 0:
    print(1)
    exit()

A.sort()
p = 0
intervals = []
for a in A:
    t = a - p - 1
    if t != 0:
        intervals.append(t)
    p = a
t = N - A[-1]
if t != 0:
    intervals.append(t)
if len(intervals) == 0:
    print(0)
    exit()
k = max(min(intervals), 1)

result = 0
for x in intervals:
    result += (x + k - 1) // k
print(result)
```
## [ABC185F - Range Xor Query](https://atcoder.jp/contests/abc185/tasks/abc185_f)

10分で突破. 「F問題なのにこんなに簡単でいいの!? 何か見落としがある!?」ってなったくらい簡単な問題. セグ木一発.

```python
from sys import stdin


class SegmentTree:
    def __init__(self, size, op, e):
        self._op = op
        self._e = e
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [e] * (t * 2 - 1)

    def __getitem__(self, key):
        return self._data[self._offset + key]

    def __setitem__(self, key, value):
        op = self._op
        data = self._data
        i = self._offset + key
        data[i] = value
        while i >= 1:
            i = (i - 1) // 2
            data[i] = op(data[i * 2 + 1], data[i * 2 + 2])

    def build(self, iterable):
        op = self._op
        data = self._data
        data[self._offset:self._offset + self._size] = iterable
        for i in range(self._offset - 1, -1, -1):
            data[i] = op(data[i * 2 + 1], data[i * 2 + 2])

    def query(self, start, stop):
        def iter_segments(data, l, r):
            while l < r:
                if l & 1 == 0:
                    yield data[l]
                if r & 1 == 0:
                    yield data[r - 1]
                l = l // 2
                r = (r - 1) // 2
        op = self._op
        it = iter_segments(self._data, start + self._offset,
                           stop + self._offset)
        result = self._e
        for v in it:
            result = op(result, v)
        return result


readline = stdin.readline

N, Q = map(int, readline().split())
A = list(map(int, readline().split()))

st = SegmentTree(N, lambda x, y: x ^ y, 0)
st.build(A)

result = []
for _ in range(Q):
    T, X, Y = map(int, readline().split())
    if T == 1:
        st[X - 1] = st[X - 1] ^ Y
    elif T == 2:
        result.append(st.query(X - 1, Y))
print(*result, sep='\n')
```
