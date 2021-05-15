# yukicoder contest 295 参戦記

## [A 1505 Zero-Product Ranges](https://yukicoder.me/problems/no/1505)

0になるものを数え上げるのは大変なので、1になるものを数え上げて引けば良い. 0で分割されたそれぞれのブロックについて、その長さをn<sub>i</sub>とすると、それぞれの組み合わせの数は <sub>n<sub>i</sub>+1</sub>C<sub>2</sub> となる.

```python
N, *A = map(int, open(0).read().split())

c = 0
l = 0
for i in range(N):
    if A[i] == 1:
        continue
    t = i - l + 1
    c += t * (t - 1) // 2
    l = i + 1
t = N - l + 1
c += t * (t - 1) // 2
print((N + 1) * N // 2 - c)
```

A を文字列として扱って、0を区切り文字として split したほうが楽だったかな.

```python
N = int(input())
A = input()

print((N + 1) * N // 2 - sum((len(s) + 1) * len(s) // 2 for s in A.replace(' ', '').split('0')))
```


## [B 1506 Unbalanced Pocky Game](https://yukicoder.me/problems/no/1506)

A<sub>i</sub>≦10<sup>9</sup> ではあるが、結局は0にして渡すか1にして渡すかの2択でしかない. A<sub>i</sub>が1の時だけは0にして渡すの一択になるが. 後はメモ化再帰すれば片付きます.

```python
from sys import setrecursionlimit

setrecursionlimit(10 ** 6)

N, *A = map(int, open(0).read().split())

cache = [[None] * (N + 1) for _ in range(2)]


def f(i, turn):
    if i == 0:
        return turn ^ 1

    if cache[turn][i] is not None:
        return cache[turn][i]

    if A[i - 1] == 1:
        t = f(i - 1, turn ^ 1)
        cache[turn][i] = t
        return t

    t = f(i - 1, turn)
    if t == turn:
        cache[turn][i] = t
        return t
    t = f(i - 1, turn ^ 1)
    cache[turn][i] = t
    return t


if f(N, 0) == 0:
    print('Alice')
else:
    print('Bob')
```
