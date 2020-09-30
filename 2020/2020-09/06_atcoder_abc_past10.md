# AtCoder Beginners Contest 過去問チャレンジ 10

## [ABC087D - People on a Line](https://atcoder.jp/contests/abc087/tasks/arc090_b)

情報により関連付けられているどれかの人の位置を0とおいて、DFS で矛盾がないかを確認すればいいだけ. もちろん1グループとなっているとは限らないことに注意.

```python
from sys import stdin

N, M = map(int, stdin.readline().split())

links = [[] for _ in range(N + 1)]
for _ in range(M):
    L, R, D = map(int, stdin.readline().split())
    links[L].append((R, D))
    links[R].append((L, -D))

t = [None] * (N + 1)
for i in range(1, N + 1):
    if t[i] is not None:
        continue
    t[i] = 0
    s = [i]
    while s:
        j = s.pop()
        for k, l in links[j]:
            if t[k] is None:
                t[k] = t[j] + l
                s.append(k)
            else:
                if t[k] != t[j] + l:
                    print('No')
                    exit()
print('Yes')
```

重み付き Union Find 木でも解ける.

```python
from sys import setrecursionlimit, stdin


def find(parent, diff_weight, i):
    t = parent[i]
    if t < 0:
        return i
    t = find(parent, diff_weight, t)
    diff_weight[i] += diff_weight[parent[i]]
    parent[i] = t
    return t


def weight(parent, diff_weight, i):
    find(parent, diff_weight, i)
    return diff_weight[i]


def unite(parent, diff_weight, i, j, d):
    d -= weight(parent, diff_weight, j)
    d += weight(parent, diff_weight, i)
    i = find(parent, diff_weight, i)
    j = find(parent, diff_weight, j)
    if i == j:
        return
    diff_weight[j] = d
    parent[i] += parent[j]
    parent[j] = i


setrecursionlimit(10 ** 6)

N, M = map(int, stdin.readline().split())
LRD = [tuple(map(int, stdin.readline().split())) for _ in range(M)]

parent = [-1] * (N + 1)
diff_weight = [0] * (N + 1)
for L, R, D in LRD:
    i = find(parent, diff_weight, L)
    j = find(parent, diff_weight, R)
    if i != j:
        unite(parent, diff_weight, L, R, D)
    else:
        if weight(parent, diff_weight, L) + D != weight(parent, diff_weight, R):
            print('No')
            exit()
print('Yes')
```

## [ABC136E - Max GCD](https://atcoder.jp/contests/abc136/tasks/abc136_e)

どれだけ操作をしても合計は変わらないので、答えの候補は合計の約数のみ. 後は K 回の操作でその約数の倍数に揃えれるかだが、その約数で割った余りをソートすれば引いたほうがいいのが前に、足したほうがいいのが後ろに来るので、前後から順番に数えていけば操作の必要回数の最小が計算できる.

```python
def make_divisor_list(n):
    result = []
    for i in range(1, int(n ** 0.5) + 1):
        if n % i == 0:
            result.append(i)
            result.append(n // i)
    return result


def calc_min_ops(r, d):
    i, j = 0, len(r)
    while i < j and r[i] == 0:
        i += 1
    i -= 1
    a, b = 0, 0
    while j - i != 1:
        if a <= b:
            i += 1
            a += r[i]
        else:
            j -= 1
            b += d - r[j]
    return a


N, K, *A = map(int, open(0).read().split())

c = sum(A)
divisors = make_divisor_list(c)
divisors.sort(reverse=True)
r = [None] * N

for d in divisors:
    for i in range(N):
        r[i] = A[i] % d
    r.sort()
    if calc_min_ops(r, d) <= K:
        print(d)
        break
```

## [ABC063D - Widespread](https://atcoder.jp/contests/abc063/tasks/arc075_b)

少なくとも ceil(max(h) / b) 以下なんだろうが、A と B の差分を体力の大きい方から順に割り当てるのを計算してしまうと間に合わない(heapq を使って確認済). にぶたんですと言われてみると、確かにそれで解けるって一瞬ででなったけどぱっと思いつきはしない不思議.

```python
from bisect import bisect_right

N, A, B, *h = map(int, open(0).read().split())

h.sort()
d = A - B

ng = (h[-1] + (A - 1)) // A - 1
ok = (h[-1] + (B - 1)) // B
while ok - ng > 1:
    m = (ok + ng) // 2
    b = B * m
    if m >= sum((h[i] - b + (d - 1)) // d for i in range(bisect_right(h, b), N)):
        ok = m
    else:
        ng = m
print(ok)
```

## [ABC060D - Simple Knapsack](https://atcoder.jp/contests/abc060/tasks/arc073_b)

ナップサック=DPだろと思ってその方向で考えてみたが全然思いつかない. そもそも N≦100 で w1≦wi≦w1+3 なので、全探索しても (100/4+1)<sup>4</sup>≒4.6×10<sup>5</sup> にしからならず、全探索で大丈夫だった.

```python
from sys import stdin
from itertools import accumulate
readline = stdin.readline

N, W = map(int, input().split())
vs = [[] for _ in range(4)]
w, v = map(int, input().split())
w1 = w
vs[0].append(v)
for _ in range(N - 1):
    w, v = map(int, input().split())
    vs[w - w1].append(v)

for i in range(4):
    vs[i].sort(reverse=True)
    vs[i] = [0] + list(accumulate(vs[i]))

result = 0
for i in range(len(vs[0])):
    a = W - w1 * i
    if a < 0:
        break
    for j in range(len(vs[1])):
        b = a - (w1 + 1) * j
        if b < 0:
            break
        for k in range(len(vs[2])):
            c = b - (w1 + 2) * k
            if c < 0:
                break
            t = vs[0][i] + vs[1][j] + vs[2][k]
            for l in range(len(vs[3])):
                d = c - (w1 + 3) * l
                if d < 0:
                    break
                result = max(result, t + vs[3][l])
print(result)
```

## [ABC122D - We Like AGC](https://atcoder.jp/contests/abc122/tasks/abc122_d)

要するに AGC、ACG、GAC、A?GC、AG?C が駄目なんだから、せいぜい末尾3文字だけ保持すれば良い. 4<sup>3</sup>-3 種類だけ保持すればよく、そこからざっくり4倍に派生するので、計算量はざっくり 250×100=2.5×10<sup>4</sup> で余裕の範囲だった. という訳で DP で解けた.

```python
def is_ok(s):
    if s[1:] in ['AGC', 'ACG', 'GAC']:
        return False
    if s[0] == 'A' and s[2:] == 'GC':
        return False
    if s[:2] == 'AG' and s[3] == 'C':
        return False
    return True


m = 1000000007

N = int(input())

d = {}
for i in 'ACGT':
    for j in 'ACGT':
        for k in 'ACGT':
            d[i + j + k] = 1
for k in ['AGC', 'ACG', 'GAC']:
    del d[k]

for i in range(N - 3):
    t = {}
    for k in d:
        for i in 'ACGT':
            s = k + i
            if is_ok(s):
                t.setdefault(s[1:], 0)
                t[s[1:]] += d[k]
                t[s[1:]] %= m
    d = t

print(sum(d.values()) % m)
```

## [ABC100D - Patisserie ABC](https://atcoder.jp/contests/abc100/tasks/abc100_d)

絶対値がなければ簡単なのになあと思った. だったらそれぞれプラスとマイナスの最大値を求めればいいのかなあと思った. それくらいなら2<sup>3</sup>回、絶対値がない場合のをするのと同じなので計算量的には問題ない. 本当にそれで最大値になるという確信はなかったが AC したので考え方としてあっていたようだ.

```python
from itertools import product
from sys import stdin
readline = stdin.readline

N, M = map(int, readline().split())
xyz = [tuple(map(int, readline().split())) for _ in range(N)]

result = 0
for s in product([1, -1], repeat=3):
    xyz.sort(reverse=True, key=lambda e: s[0] * e[0] + s[1] * e[1] + s[2] * e[2])
    cx, cy, cz = 0, 0, 0
    for x, y, z in xyz[:M]:
        cx += x
        cy += y
        cz += z
    result = max(result, abs(cx) + abs(cy) + abs(cz))
print(result)
```
