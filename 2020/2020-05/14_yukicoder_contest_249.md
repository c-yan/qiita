# yukicoder contest 249 参戦記

## [A 1057 素敵な日](https://yukicoder.me/problems/no/1057)

初日は前日に食べたケーキがないので素敵な日になりえない. なのでショートケーキとチョコケーキの数が違うのであれば多い方を初日に食べる. 後は交互に食べていけば良い.

```python
A, B = map(int, input().split())

if A == B:
    print(2 * A - 1)
else:
    print(min(A, B) * 2)
```

## [B 1058 素敵な数](https://yukicoder.me/problems/no/1058)

求める数が、 素数ではなく2以上10<sup>5</sup>以下の約数を持たないということなので、要するに10<sup>5</sup>より大きい素数2つの積の小さい順となる.

なので、適当にそういう素数を求め、

```python
>>> for i in range(10**5, 10**5+1000):
...   flag = True
...   for j in range(2, int(sqrt(i))+1):
...     if i % j == 0:
...       flag = False
...       break
...   if flag:
...     print(i)
...
100003
100019
100043
100049
100057
100069
100103
100109
100129
100151
...
```

その積を小さい順に求め、

```python
>>> result = []
>>> for i in [100003,100019,100043,100049,100057,100069,100103,100109,100129,100151]:
...   for j in [100003,100019,100043,100049,100057,100069,100103,100109,100129,100151]:
...     result.append(i*j)
...
>>> print(sorted(set(result))[:10])
[10000600009, 10002200057, 10003800361, 10004600129, 10005200147, 10006000171, 10006200817, 10006800931, 10007200207, 10007601083]
```

ソースコードに埋め込んでおしまい.

```python
N = int(input())

print([1, 10000600009, 10002200057, 10003800361, 10004600129, 10005200147, 10006000171, 10006200817, 10006800931, 10007200207][N - 1])
```

## [C 1059 素敵な集合](https://yukicoder.me/problems/no/1059)

突破できず.

まずコストは0か1である. なぜなら i, i + 1 の組はコスト1だからである. 後はエラトステネスの篩の要領でコストが0になるものを落とせば終わりかと思ったら実はそうではない. 例えば 2, 3, 6 があったとすると、2と3の組ではコストが1になってしまうが、3, 6 の組でコスト0で3を消して、2, 6 の組でコスト0で6を消すと、コスト0で素敵な集合を達成できる. つまり共通の公倍数を持つもの同士もコスト0で消せるのだ. となると Union Find で Union がいくつあるかという問題に帰着し、それで解ける.

```python
from sys import setrecursionlimit


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
    parent[i] += parent[j]
    parent[j] = i


setrecursionlimit(10 ** 6)

L, R = map(int, input().split())

parent = [-1] * (R + 1)
for i in range(L, R):
    if find(parent, i) != i:
        continue
    for j in range(i * 2, R + 1, i):
        unite(parent, i, j)
print(sum(1 for i in range(L, R + 1) if find(parent, i) == i) - 1)
```
