# AtCoder Beginner Contest 250 参戦記

## [ABC250A - Adjacent Squares](https://atcoder.jp/contests/abc250/tasks/abc250_a)

4分半で突破. 書くだけ.

```python
H, W = map(int, input().split())
R, C = map(int, input().split())

result = 0
if R != 1:
    result += 1
if C != 1:
    result += 1
if R != H:
    result += 1
if C != W:
    result += 1
print(result)
```

## [ABC250B - Enlarged Checker Board](https://atcoder.jp/contests/abc250/tasks/abc250_b)

9分半で突破. 書くだけ.

```python
N, A, B = map(int, input().split())

l = ('.' * B + '#' * B) * N

for i in range(N):
    for _ in range(A):
        if i % 2 == 0:
            print(l[:N * B])
        else:
            print(l[B:(N + 1) * B])
```

## [ABC250C - Adjacent Swaps](https://atcoder.jp/contests/abc250/tasks/abc250_c)

10分で突破. *x* のある位置を毎回探していると *O*(*NQ*) = 4×10<sup>10</sup> になって TLE するので、*x* の存在位置も管理するだけ.

```python
from sys import stdin

readline = stdin.readline

N, Q = map(int, readline().split())

a = list(range(N))
b = list(range(N))
for _ in range(Q):
    x = int(readline()) - 1
    i = b[x]
    if i != N - 1:
        j = i + 1
    else:
        j = i - 1
    y = a[j]
    a[i] = y
    a[j] = x
    b[x] = j
    b[y] = i
print(*[i + 1 for i in a])
```

## [ABC250D - 250-like Number](https://atcoder.jp/contests/abc250/tasks/abc250_d)

45分半で突破. *q* を基準に *O*(*N*) で *p* を探すと TLE になりそうなので、尺取法だよなあとあっさり方針は決まったけど、結構バグらせて時間がかかった.

```python
N = int(input())

def make_prime_table(n):
    sieve = list(range(n + 1))
    sieve[0] = -1
    sieve[1] = -1
    for i in range(4, n + 1, 2):
        sieve[i] = 2
    for i in range(3, int(n ** 0.5) + 1, 2):
        if sieve[i] != i:
            continue
        for j in range(i * i, n + 1, i * 2):
            if sieve[j] == j:
                sieve[j] = i
    return sieve

prime_table = make_prime_table(10 ** 6)
primes = [i for i in range(10 ** 6 + 1) if prime_table[i] == i]

result = 0
l = 0
for r in range(len(primes) - 1, 0, -1):
    while primes[l] * (primes[r] ** 3) <= N and l < r:
        l += 1
    result += l
    if l == r:
        l -= 1
print(result)
```

## [ABC250E - Prefix Equality](https://atcoder.jp/contests/abc250/tasks/abc250_e)

解けず. 単調増加になる順で解けばいいやと正しい方針は立ったもののなかなかに実装が重くて間に合わず. ハッシュが取れれば簡単だけど、出現順が変わると同じ値にならないしなあと思っていたら、解説で集合用のハッシュである Zobrist Hash が紹介されていて、これだーってなった. Zobrist Hash 自体は昔にやねうらおさんのブログで見ていたようなのだが、当時は競プロしてなかったしなあ(笑).

```python
from sys import stdin
from random import sample

readline = stdin.readline

N = int(readline())
a = list(map(int, readline().split()))
b = list(map(int, readline().split()))

v = set(a + b)
d = {}
for x, y in zip(v, sample(range(10 ** 18), len(v))) :
    d[x] = y

def hash(xs):
    result = []
    appeared = set()
    h = 0
    for x in xs:
        if x not in appeared:
            appeared.add(x)
            h ^= x
        result.append(h)
    return result

ha = hash(d[x] for x in a)
hb = hash(d[x] for x in b)

Q = int(readline())
result = []
for _ in range(Q):
    x, y = map(int, readline().split())
    if ha[x - 1] == hb[y - 1]:
        result.append('Yes')
    else:
        result.append('No')
print(*result, sep='\n')
```
