# AtCoder Beginner Contest 172 参戦記

前回はE問題まで20分で行ってF問題を100分考えて駄目だったけど、今回は30分でD問題まで行ってE問題を90分考えたけど駄目だった. そもそもC問題で制限を見ながら計算量の緩和をすることが珍しいのに、2重で緩和しないといけないのってやっぱり ABC にしては難しいような. 実際前回の阿鼻叫喚の EXCEL 列名C問題でも半分弱解いているのに、今回のC問題は3割弱しか解けていない時点で難しいんだよなあ.

## [ABC172A - Calc](https://atcoder.jp/contests/abc172/tasks/abc172_a)

2分くらい?で突破. コードテストが動かなくて、B問題を書いたあとに提出した. 書くだけ.

```python
a = int(input())

print(a + a * a + a * a * a)
```

## [ABC172B - Minor Change](https://atcoder.jp/contests/abc172/tasks/abc172_b)

2分くらい?で突破. 書くだけ. 違うところの数ですね.

```python
S = input()
T = input()

result = 0
for i in range(len(S)):
    if S[i] == T[i]:
        continue
    result += 1
print(result)
```

## [ABC172C - Tsundoku](https://atcoder.jp/contests/abc172/tasks/abc172_c)

9分半で突破. 累積和+二分探索で *O*(<i>N</i>log<i>N</i>). 二分探索で境界値バグを出さないように気を揉んだので、解説の *O*(*N*+*M*) 解はスマートだなあって思いました.

```python
from itertools import accumulate
from bisect import bisect_left

N, M, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

a = list(accumulate(A))
b = list(accumulate(B))

result = 0
j = bisect_left(b, K)
if j == M:
  result = M
elif b[j] == K:
    result = j + 1
else:
    result = j
for i in range(N):
    t = K - a[i]
    if t < 0:
        break
    j = bisect_left(b, t)
    if j == M:
        result = max(result, i + 1 + M)
    elif b[j] == t:
        result = max(result, i + 1 + j + 1)
    else:
        result = max(result, i + 1 + j)
print(result)
```

## [ABC172D - Sum of Divisors](https://atcoder.jp/contests/abc172/tasks/abc172_d)

14分で突破. 半分くらいは [ABC152E - Flatten](https://atcoder.jp/contests/abc152/tasks/abc152_e) のコードと同じ. エラトステネスの篩で素因数分解して、各素数の個数 + 1 を積算すると素数の数が出る. それを使って f(X) を書いて、後は問題文通りに∑<sup>N</sup><sub>K=1</sub>K×f(K) を求めるだけ. PyPy で2.8秒なので割と制限ギリギリだった.

```python
# エラトステネスの篩
N = int(input())

sieve = [0] * (N + 1)
sieve[0] = -1
sieve[1] = -1
for i in range(2, N + 1):
    if sieve[i] != 0:
        continue
    sieve[i] = i
    for j in range(i * i, N + 1, i):
        if sieve[j] == 0:
            sieve[j] = i


def f(X):
    t = []
    a = X
    while a != 1:
        if len(t) != 0 and t[-1][0] == sieve[a]:
            t[-1][1] += 1
        else:
            t.append([sieve[a], 1])
        a //= sieve[a]
    result = 1
    for _, n in t:
        result *= n + 1
    return result


result = 0
for K in range(1, N + 1):
    result += K * f(K)
print(result)
```

## [ABC172E - NEQ](https://atcoder.jp/contests/abc172/tasks/abc172_e)

90分考えたけど解けず.
