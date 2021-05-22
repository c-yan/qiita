# エイシングプログラミングコンテスト2021(AtCoder Beginner Contest 202) 参戦記

## [ABC202A - Three Dice](https://atcoder.jp/contests/abc202/tasks/abc202_a)

1分で突破. 書くだけ.

```python
a, b, c = map(int, input().split())

print(21 - (a + b + c))
```

## [ABC202B - 180°](https://atcoder.jp/contests/abc202/tasks/abc202_b)

3分で突破. 書くだけ.

```python
S = input()

d = {'0': '0', '1': '1', '6': '9', '8': '8', '9': '6'}
print(''.join(d[c] for c in reversed(S)))
```

## [ABC202C - Made Up](https://atcoder.jp/contests/abc202/tasks/abc202_c)

11分半で突破、TLE1. Aに含まれる値 a に対して、同じ値である B のインデックスの値を求め、そのインデックスの値と同じ値が C にいくつ含まれているかを求めれば良い.

```python
from collections import Counter

N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))
C = list(map(int, input().split()))

x = {}
for i in range(N):
    b = B[i]
    x.setdefault(b, [])
    x[b].append(i)

y = {}
for i in range(N):
    c = C[i] - 1
    y.setdefault(c, 0)
    y[c] += 1

z = Counter(A)

result = 0
for a in z:
    if a not in x:
        continue
    c = 0
    for i in x[a]:
        if i not in y:
            continue
        c += y[i]
    result += c * z[a]
print(result)
```

## [ABC202D - aab aba baa](https://atcoder.jp/contests/abc202/tasks/abc202_d)

作れる文字列の総数は重複組合せで求められる. 次の文字を 'a' としたときの組み合わせの数を求めてそれを K と比較すれば一文字づつ確定していくことができる.

```python
A, B, K = map(int, input().split())


def comb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    a = 1
    b = 1
    for i in range(k):
        a *= n - i
        b *= k - i
    return a // b


def h(n, k):
    return comb(n + k - 1, k)


n = 0
result = ''
a, b = A, B
while a != 0 and b != 0:
    t = h(b + 1, a - 1)
    if n + t >= K:
        result += 'a'
        a -= 1
    else:
        result += 'b'
        b -= 1
        n += t
result += 'a' * a + 'b' * b
print(result)
```
