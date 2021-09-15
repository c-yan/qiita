# AtCoder Beginner Contest 217 バーチャル参戦記

87:39でABCDE完. とはいえ、本番だったらさすがにA問題のRE2はしなかったと思うので、実質は77:39と思うと、参加できてたらパフォ1225だった模様. 参加できなくてよかったですね(笑).

## [ABC217A - Signed Difficulty](https://atcoder.jp/contests/abc217/tasks/abc217_a)

3分で突破、RE2. 書くだけ. 全く動作確認をせず突っ込んで死んだ(笑).

```python
S, T = input().split()

if S < T:
    print('Yes')
else:
    print('No')
```

## [ABC217B - AtCoder Quiz](https://atcoder.jp/contests/abc217/tasks/abc217_b)

2分で突破. どうとでも書けるだろうけど、set が一番楽かなと思ってこうなった.

```python
S1 = input()
S2 = input()
S3 = input()

result = {'ABC' , 'ARC' , 'AGC' , 'AHC'}
result.remove(S1)
result.remove(S2)
result.remove(S3)
print(*result)
```

## [ABC217C - Inverse of Permutation](https://atcoder.jp/contests/abc217/tasks/abc217_c)

3分半で突破. 簡単すぎて、自分が題意を勘違いしているんじゃないかと心配になった.

```python
N, *p = map(int, open(0).read().split())

Q = [None] * N
for i in range(N):
    Q[p[i] - 1] = i + 1
print(*Q)
```

## [ABC217E - Sorting Queries](https://atcoder.jp/contests/abc217/tasks/abc217_e)

23分?で突破. 考察に時間がかかったけど、実装はすぐでしたね. 一つのデータ構造でどう実現するかを考えていたけど、無理そうだなと思って2つならと考えたら後はすぐでしたね.

```python
from sys import stdin
from collections import deque
from heapq import heappop, heappush

readline = stdin.readline

Q = int(readline())

h = []
d = deque([])
result = []
for _ in range(Q):
    query = readline()
    if query[0] == '1':
        _, x = map(int, query.split())
        d.append(x)
    elif query[0] == '2':
        if len(h) == 0:
            result.append(d.popleft())
        else:
            result.append(heappop(h))
    elif query[0] == '3':
        for x in d:
            heappush(h, x)
        d = deque([])
print(*result, sep='\n')
```

## [ABC217D - Cutting Woods](https://atcoder.jp/contests/abc217/tasks/abc217_d)

40分半?で突破. 座標圧縮と2つのセグ木で書いたけど、D問題にしては牛刀すぎるように感じるので、出題者の想定解とは違うだろうなと思っていたけど、Python における想定解そのものだったとは orz. 平衡二分探索木がビルトインな言語ってどれくらいあるんだろう. 少なくとも Python と Go は入ってないし、Java も無かった気がするよなあ. C++ だけ簡単な問題になっている感. 逆順と Union Find で出来ないかなとも考えてみて思いつかなかったけど、想定解にあるってことは書きようがあるんだなあ. どう書くんだろ.

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

L, Q = map(int, readline().split())
cx = [tuple(map(int, readline().split())) for _ in range(Q)]

a = sorted([x for _, x in cx] + [0, L])
b = {}
for i in range(len(a)):
    b[a[i]] = i

st1 = SegmentTree(len(a), max, -1)
st2 = SegmentTree(len(a), min, 10 ** 20)
st1[b[0]] = b[0]
st1[b[L]] = b[L]
st2[b[0]] = b[0]
st2[b[L]] = b[L]

result = []
for c, x in cx:
    if c == 1:
        st1[b[x]] = b[x]
        st2[b[x]] = b[x]
    elif c == 2:
        result.append(a[st2.query(b[x], b[L] + 1)] - a[st1.query(0, b[x])])
print(*result, sep='\n')
```
