# AtCoder 第二回 アルゴリズム実技検定 バーチャル参戦記

結果は76点で中級(60-79点)となりました. 前回は64点でギリギリ中級でしたが、今回は後一問解ければ上級でした.

## [past202004A - エレベーター](https://atcoder.jp/contests/past202004-open/tasks/past202004_a)

3分で突破. Bx を -x、xF を x としてしまうと、B1 と 1F の差が1にならないところだけ注意すればいいでしょう.

```python
S, T = input().split()


def f(s):
    if s[0] == 'B':
        return -int(s[1])
    else:
        return int(s[0]) - 1


print(abs(f(S) - f(T)))
```

## [past202004B - 多数決](https://atcoder.jp/contests/past202004-open/tasks/past202004_b)

3分半で突破. 集計して一番多かったのを出力するだけ.

```python
S = input()

d ={}
for c in S:
    d.setdefault(c, 0)
    d[c] += 1
print(sorted(((d[k], k) for k in d), reverse=True)[0][1])
```

## [past202004C - 山崩し](https://atcoder.jp/contests/past202004-open/tasks/past202004_c)

12分で突破. 問題文の通りに書くだけ.

```python
N = int(input())
S = [input() for _ in range(N)]

T = [list(l) for l in S]
for i in range(N - 2, -1, -1):
    for j in range(2 * N - 1):
        if T[i][j] == '.':
            continue
        if j - 1 >= 0:
            if T[i + 1][j - 1] == 'X':
                T[i][j] = 'X'
        if T[i + 1][j] == 'X':
            T[i][j] = 'X'
        if j + 1 < 2 * N - 1:
            if T[i + 1][j + 1] == 'X':
                T[i][j] = 'X'
for l in T:
    print(''.join(l))
```

## [past202004D - パターンマッチ](https://atcoder.jp/contests/past202004-open/tasks/past202004_d)

11分で突破. "." が任意の文字というのは正規表現なので、マッチするかは正規表現一発. 後はパターンを全生成をするだけですが、Python は `itertools.product` 一発でした.

```python
from itertools import product
from re import search

S = input()

cs = set(S)
cs.add('.')

result = 0
for i in range(1, 4):
    for p in product(cs, repeat=i):
        if search(''.join(p), S):
            result += 1
print(result)
```

## [past202004E - 順列](https://atcoder.jp/contests/past202004-open/tasks/past202004_e)

8分半で突破. 問題文に書かれているとおりに書くだけなので、難しいことはなにもないかと思う.

```python
N = int(input())
A = list(map(int, input().split()))

result = []
for i in range(N):
    x = i
    j = 1
    x = A[x] - 1
    while x != i:
        x = A[x] - 1
        j += 1
    result.append(j)
print(*result)
```

## [past202004F - タスクの消化](https://atcoder.jp/contests/past202004-open/tasks/past202004_f)

7分半で突破. [ABC137D - Summer Vacation](https://atcoder.jp/contests/abc137/tasks/abc137_d) を思い出した. 実行可能日が来たタスクを優先度付きキューに突っ込んで、優先度付きキューから一番ポイントが高いタスクを取り出して消化するを繰り返すだけ.

```python
from heapq import heappush, heappop

N = int(input())

d = {}
for _ in range(N):
    A, B = map(int, input().split())
    d.setdefault(A, [])
    d[A].append(B)

t = []
result = 0
for i in range(1, N + 1):
    if i in d:
        for b in d[i]:
            heappush(t, -b)
    result += -heappop(t)
    print(result)
```

## [past202004H - 1-9 Grid](https://atcoder.jp/contests/past202004-open/tasks/past202004_h)

32分で突破. 一つ前の全ての場所から、次の全ての場所へ最小コストを求める DP で解けた.

```python
N, M = map(int, input().split())
A = [input() for _ in range(N)]

d = {}
for i in range(N):
    for j in range(M):
        d.setdefault(A[i][j], [])
        d[A[i][j]].append((i, j))


for i in list(range(1, 10)) + ['S', 'G']:
    if str(i) not in d:
        print(-1)
        exit()

dp = [[float('inf')] * M for _ in range(N)]
i, j = d['S'][0]
dp[i][j] = 0

current = 'S'
while current != 'G':
    if current == 'S':
        next = '1'
    elif current == '9':
        next = 'G'
    else:
        next = str(int(current) + 1)
    for i, j in d[current]:
        for ni, nj in d[next]:
            dp[ni][nj] = min(dp[ni][nj], dp[i][j] + abs(ni - i) + abs(nj - j))
    current = next


i, j = d['G'][0]
print(dp[i][j])
```

## [past202004G - ストリング・クエリ](https://atcoder.jp/contests/past202004-open/tasks/past202004_g)

23分半で突破. 問題文通りに素直に文字列を作ってしまうと TLE 必至なので、文字と長さの組のリストを管理して解いた.

```python
from collections import deque

Q = int(input())

S = deque()
for _ in range(Q):
    Query = input().split()
    if Query[0] == '1':
        C = Query[1]
        X = int(Query[2])
        S.append((C, X))
    elif Query[0] == '2':
        D = int(Query[1])
        d = {}
        while D != 0 and len(S) != 0:
            C, X = S.popleft()
            d.setdefault(C, 0)
            if X >= D:
                d[C] += D
                if X != D:
                    S.appendleft((C, X - D))
                D = 0
            else:
                d[C] += X
                D -= X
        print(sum(v * v for v in d.values()))
```

## [past202004I - トーナメント](https://atcoder.jp/contests/past202004-open/tasks/past202004_i)

12分で突破. 人の数が 2<sup>N</sup> で N も16以下と小さいので、素直にシミュレーションするだけで簡単.

```python
N = int(input())
A = list(map(int, input().split()))

result = [1] * (2 ** N)
t = [(i, A[i]) for i in range(2 ** N)]
while len(t) != 1:
    nt = []
    for i in range(0, len(t), 2):
        ai, aj = t[i]
        bi, bj = t[i + 1]
        if aj > bj:
            result[ai] += 1
            nt.append(t[i])
        else:
            result[bi] += 1
            nt.append(t[i + 1])
    t = nt
result[t[0][0]] -= 1

for i in result:
    print(i)
```

## [past202004J - 文字列解析](https://atcoder.jp/contests/past202004-open/tasks/past202004_j)

20分で突破. 最終的な文字列Sの長さが1000以下なので、素直にシミュレーションすれば大丈夫. 左カッコを見つけたら、対になる右カッコを探して、その内側にもカッコがあったら先に処理しないといけないので、処理を再帰関数で書いて終わり.

```python
S = input()


def f(s):
    i = s.find('(')
    if i == -1:
        return s

    result = s[:i]
    s = s[i:]
    i = 1
    n = 1
    while n != 0:
        if s[i] == '(':
            n += 1
        elif s[i] == ')':
            n -= 1
        i += 1
    t = f(s[1:i - 1])
    result += t + t[::-1]
    result += f(s[i:])
    return result


print(f(S))
```

## [past202004L - 辞書順最小](https://atcoder.jp/contests/past202004-open/tasks/past202004_l)

素直に書くと *O*(*N*<sup>2</sup>) になって TLE なので計算量の軽減を考える. 選択可能な範囲から最小値の左端を取る貪欲法なので、区間最小値をセグ木で求めれば *O*(<i>N</i>log<i>N</i>) になって解けました.

```python
class SegmentTree:
    _f = None
    _size = None
    _offset = None
    _data = None

    def __init__(self, size, f):
        self._f = f
        self._size = size
        t = 1
        while t < size:
            t *= 2
        self._offset = t - 1
        self._data = [0] * (t * 2 - 1)

    def build(self, iterable):
        f = self._f
        data = self._data
        data[self._offset:self._offset + self._size] = iterable
        for i in range(self._offset - 1, -1, -1):
            data[i] = f(data[i * 2 + 1], data[i * 2 + 2])

    def query(self, start, stop):
        def iter_segments(data, l, r):
            while l < r:
                if l & 1 == 0:
                    yield data[l]
                if r & 1 == 0:
                    yield data[r - 1]
                l = l // 2
                r = (r - 1) // 2
        f = self._f
        it = iter_segments(self._data, start + self._offset,
                           stop + self._offset)
        result = next(it)
        for e in it:
            result = f(result, e)
        return result


N, K, D = map(int, input().split())
A = list(map(int, input().split()))

if 1 + (K - 1) * D > N:
    print(-1)
    exit()

st = SegmentTree(N, min)
st.build(A)

result = []
i = 0
for k in range(K - 1, -1, -1):
    m = st.query(i, N - k * D)
    result.append(m)
    while A[i] != m:
        i += 1
    i += D
print(*result)
```

追記: 解説通り優先度付きキューで解いてみた. キューの中にすでに通り過ぎたところの値が残っているので、それが最小になったときどうするのだろうと思ったが、そうか、捨てれば良いのか…….

```python
from heapq import heappush, heappop

N, K, D = map(int, input().split())
A = list(map(int, input().split()))

if 1 + (K - 1) * D > N:
    print(-1)
    exit()

result = []
q = []
i = 0
l = 0
for k in range(K - 1, -1, -1):
    for j in range(l, N - k * D):
        heappush(q, (A[j], j))
    l = N - k * D
    while True:
        a, j = heappop(q)
        if j >= i:
            break
    result.append(a)
    i = j + D
print(*result)
```

Sparse table でも解いてみた. 今回の問題だとセグ木が *O*(*N* + (*N* / *D*)log<i>N</i>) に対し、*O*(<i>N</i>log<i>N</i> + *N* / *D*) なのでちょっと遅い.

```python
class SparseTable():
    _data = None
    _lookup = None
    _f = None

    def __init__(self, a):
        data = []
        lookup = []
        f = min
        b = 0
        while (1 << b) <= len(a):
            b += 1
        for _ in range(b):
            data.append([0] * (1 << b))
        data[0][:len(a)] = a
        for i in range(1, b):
            j = 0
            di = data[i]
            di1 = data[i - 1]
            while j + (1 << i) <= (1 << b):
                di[j] = f(di1[j], di1[j + (1 << (i - 1))])
                j += 1
        lookup = [0] * (len(a) + 1)
        for i in range(2, len(lookup)):
            lookup[i] = lookup[i >> 1] + 1
        self._data = data
        self._lookup = lookup
        self._f = f

    def query(self, start, stop):
        b = self._lookup[stop - start]
        db = self._data[b]
        return self._f(db[start], db[stop - (1 << b)])


N, K, D = map(int, input().split())
A = list(map(int, input().split()))

if 1 + (K - 1) * D > N:
    print(-1)
    exit()

st = SparseTable(A)

result = []
i = 0
for k in range(K - 1, -1, -1):
    m = st.query(i, N - k * D)
    result.append(m)
    while A[i] != m:
        i += 1
    i += D
print(*result)
```

## [past202004K - 括弧](https://atcoder.jp/contests/past202004-open/tasks/past202004_k)

突破できず. TLE するけど、正しい答えが出るナイーブな実装すら書けず.
