# AtCoder Library Practice Contest 参戦記 (Python)

ACL Contest は 1200 から rated なので rated で出場できるかどうしようと考えつつ、AtCoder Library Practice Contest を解いてみるテスト.

## [PRACTICE2A - Disjoint Set Union](https://atcoder.jp/contests/practice2/tasks/practice2_a)

ACL に Union Find 入ってないような、見落としてる??? あ、DSU = Disjoint Set Union = Union Find か.

```python
from sys import setrecursionlimit, stdin


def find(parent, i):
    t = parent[i]
    if t < 0:
        return i
    t = find(parent, t)
    parent[i] = t
    return t


def unite(parent, i, j):
    i = find(parent, i)
    j = find(parent, j)
    if i == j:
        return
    parent[j] += parent[i]
    parent[i] = j


readline = stdin.readline
setrecursionlimit(10 ** 6)

N, Q = map(int, readline().split())

parent = [-1] * (N + 1)
result = []
for _ in range(Q):
    t, u, v = map(int, readline().split())
    if t == 0:
        unite(parent, u, v)
    elif t == 1:
        if find(parent, u) == find(parent, v):
            result.append(1)
        else:
            result.append(0)

print(*result, sep='\n')
```

## [PRACTICE2B - Fenwick Tree](https://atcoder.jp/contests/practice2/tasks/practice2_b)

結構前に書いたのにまだ2度目の登板.

```python
from sys import setrecursionlimit, stdin


def bit_add(bit, i, x):
    i += 1
    n = len(bit)
    while i <= n:
        bit[i - 1] += x
        i += i & -i


def bit_sum(bit, i):
    result = 0
    i += 1
    while i > 0:
        result += bit[i - 1]
        i -= i & -i
    return result


def query(bit, start, stop):
    if start == 0:
        return bit_sum(bit, stop - 1)
    else:
        return bit_sum(bit, stop - 1) - bit_sum(bit, start - 1)


readline = stdin.readline
setrecursionlimit(10 ** 6)

N, Q = map(int, readline().split())
a = list(map(int, readline().split()))

bit = [0] * N
for i in range(N):
    bit_add(bit, i, a[i])

result = []
for _ in range(Q):
    Query = readline()
    if Query[0] == '0':
        _, p, x = map(int, Query.split())
        bit_add(bit, p, x)
    elif Query[0] == '1':
        _, l, r = map(int, Query.split())
        result.append(query(bit, l, r))

print(*result, sep='\n')
```

## [PRACTICE2C - Floor Sum](https://atcoder.jp/contests/practice2/tasks/practice2_c)

これの応用が想像できない.

```python
from sys import stdin


def floor_sum(n, m, a, b):
    result = 0
    if a >= m:
        result += (n - 1) * n * (a // m) // 2
        a %= m
    if b >= m:
        result += n * (b // m)
        b %= m

    yMax = (a * n + b) // m
    xMax = yMax * m - b
    if yMax == 0:
        return result
    result += (n - (xMax + a - 1) // a) * yMax
    result += floor_sum(yMax, a, m, (a - xMax % a) % a)
    return result


readline = stdin.readline

T = int(readline())

result = []
for _ in range(T):
    N, M, A, B = map(int, readline().split())
    result.append(floor_sum(N, M, A, B))
print(*result, sep='\n')
```

## [PRACTICE2I - Number of Substrings](https://atcoder.jp/contests/practice2/tasks/practice2_i)

TLE する、まいったな.

```python
from functools import cmp_to_key


def make_suffix_array(s):
    n = len(s)
    sa = list(range(n))
    rnk = [ord(c) for c in s]
    tmp = [0] * n
    k = 1
    while k < n:
        def cmp(x, y):
            if rnk[x] != rnk[y]:
                return rnk[x] - rnk[y]
            rx, ry = -1, -1
            if x + k < n:
                rx = rnk[x + k]
            if y + k < n:
                ry = rnk[y + k]
            return rx - ry
        sa.sort(key=cmp_to_key(cmp))
        tmp[sa[0]] = 0
        for i in range(1, n):
            tmp[sa[i]] = tmp[sa[i - 1]]
            if cmp(sa[i - 1], sa[i]):
                tmp[sa[i]] += 1
        tmp, rnk = rnk, tmp
        k *= 2
    return sa


def make_lcp_array(s, sa):
    s = [ord(c) for c in s]
    n = len(s)
    rnk = [None] * n
    for i in range(n):
        rnk[sa[i]] = i
    lcp = [0] * (n - 1)
    h = 0
    for i in range(n):
        if h > 0:
            h -= 1
        if rnk[i] == 0:
            continue
        j = sa[rnk[i] - 1]
        while j + h < n and i + h < n:
            if s[j + h] != s[i + h]:
                break
            h += 1
        lcp[rnk[i] - 1] = h
    return lcp


S = input()
sa = make_suffix_array(S)

result = len(S) * (len(S) + 1) // 2
for x in make_lcp_array(S, sa):
    result -= x
print(result)
```

## [PRACTICE2J - Segment Tree](https://atcoder.jp/contests/practice2/tasks/practice2_j)

TLE する、まいったな.

```python
from sys import stdin


class SegmentTree:
    _op = None
    _e = None
    _size = None
    _offset = None
    _data = None

    def __init__(self, size, op, e):
        self._op = op
        self._e = e
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [0] * (t * 2 - 1)

    def build(self, iterable):
        op = self._op
        data = self._data
        data[self._offset:self._offset + self._size] = iterable
        for i in range(self._offset - 1, -1, -1):
            data[i] = op(data[i * 2 + 1], data[i * 2 + 2])

    def update(self, index, value):
        op = self._op
        data = self._data
        i = self._offset + index
        data[i] = value
        while i >= 1:
            i = (i - 1) // 2
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

st = SegmentTree(N - 1, max, -1)
st.build(A)

result = []
for _ in range(Q):
    q = readline()
    if q[0] in '13':
        T, X, V = map(int, q.split())
        if T == 1:
            st.update(X - 1, V)
        elif T == 3:
            ok = N + 1
            ng = X - 1
            while abs(ng - ok) > 1:
                m = (ok + ng) // 2
                if st.query(X - 1, m) >= V:
                    ok = m
                else:
                    ng = m
            result.append(ok)
    elif q[0] == '2':
        T, L, R = map(int, q.split())
        result.append(st.query(L - 1, R))
print(*result, sep='\n')
```
