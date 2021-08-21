# AtCoder Beginner Contest 215 参戦記

## [ABC215A - Your First Judge](https://atcoder.jp/contests/abc215/tasks/abc215_a)

2分で突破. 書くだけ.

```python
S = input()

if S == 'Hello,World!':
    print('AC')
else:
    print('WA')
```

## [ABC215B - log2(N)](https://atcoder.jp/contests/abc215/tasks/abc215_b)

2分で突破. log を取って、誤差を考慮してその近辺をチェックするのでもいいんだけど、素直にシングルループ回しても64回くらいなので、素直に行った.

```python
N = int(input())

for k in range(100):
    if 2 ** k <= N:
        continue
    result = k - 1
    break
print(result)
```

## [ABC215C - One More aab aba baa](https://atcoder.jp/contests/abc215/tasks/abc215_c)

4分で突破. |*S*|≤8 だから全部生成しても大丈夫なので全部生成した.

```python
from itertools import permutations

S, K = input().split()
K = int(K)

print(sorted(set(''.join(p) for p in permutations(S)))[K - 1])
```

## [ABC215D - Coprime 2](https://atcoder.jp/contests/abc215/tasks/abc215_d)

8分で突破. TLE1、RE1. *A*の最小公倍数と*k*の最大公約数が1かどうかなだけじゃんと思って TLE した(馬鹿). *A*の最小公倍数なんてでかいに決まってるじゃん. *A*の最小公倍数に含まれる素数の集合と*k*を素因数分解した素数の集合の積集合が空かどうか.

```python
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


def prime_factorize(n):
    result = []
    while n != 1:
        p = prime_table[n]
        e = 0
        while n % p == 0:
            n //= p
            e += 1
        result.append((p, e))
    return result


N, M, *A = map(int, open(0).read().split())

prime_table = make_prime_table(10 ** 5)

s = set()
for a in A:
    for p, _ in prime_factorize(a):
        s.add(p)

result = []
for k in range(1, M + 1):
    if any(p in s for p, _ in prime_factorize(k)):
        continue
    result.append(k)
print(len(result))
print(*result, sep='\n')
```

## [ABC215E - Chain Contestant](https://atcoder.jp/contests/abc215/tasks/abc215_e)

53分で突破. TLE1. 最後に選択した文字と、これまでに選択した文字集合の組に対する選び方の数を使って DP をすれば良い. サラッとは言っているけど、とんまなバグを埋め込んで直すのに時間がかかってしまった…….

```python
m = 998244353

N = int(input())
S = input()


def conv(current, used):
    return current + (used << 5)


def deconv(x):
    return x & 31, x >> 5


a = {conv(0, 0): 1}
for x in [ord(c) - ord('A') + 1 for c in S]:
    b = {}
    for y in a:
        c, u = deconv(y)
        if c == x:
            b.setdefault(y, 0)
            b[y] += a[y]
            b[y] %= m
        elif u & (1 << x) == 0:
            t = conv(x, u + (1 << x))
            b.setdefault(t, 0)
            b[t] += a[y]
            b[t] %= m
        b.setdefault(y, 0)
        b[y] += a[y]
        b[y] %= m
    a = b

result = 0
for k in a:
    if k == conv(0, 0):
        continue
    result += a[k]
    result %= m
print(result)
```
