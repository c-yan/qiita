# yukicoder contest 275 参戦記

## [A 1291 小手調べ](https://yukicoder.me/problems/no/1291)

答えは0以上の整数なので、1桁にトラップがあるので注意. d<sub>i</sub>≦100 なので、素直に整数で答えを計算すると int64 の範囲を超えますね. Python だから何も考えずに計算できるけど.

```python
N, *d = map(int, open(0).read().split())

for i in range(N):
    if d[i] == 1:
        print(10)
    else:
        print(9 * 10 ** (d[i] - 1))
```

まあ、文字列で計算しても難しくはないですが.

```python
N, *d = map(int, open(0).read().split())

for i in range(N):
    if d[i] == 1:
        print(10)
    else:
        print('9' + '0' * (d[i] - 1))
```

## [B 1292 パタパタ三角形](https://yukicoder.me/problems/no/1292)

それぞれの辺 s<sub>i</sub> について、対称移動で各辺がどこに行くかを手書きで図示すれば、規則性があることはすぐ分かる. 各移動を重心を上下左右のどちらかに動かすものだと考えれば、各移動ごとの x, y 座標が決まるので、後はセットを使えば簡単に解ける.

```python
S = input()

x, y = 0, 0
t = {(0, 0)}

s0 = [(-1, 0), (0, -1), (1, 0), (-1, 0), (0, 1), (1, 0)]
s1 = [(-1, 0), (0, 1), (1, 0), (-1, 0), (0, -1), (1, 0)]

for ch in S:
    if ch == 'a':
        if y % 2 == 0:
            dx, dy = s0[x % 6]
        else:
            dx, dy = s1[x % 6]
    elif ch == 'b':
        if y % 2 == 0:
            dx, dy = s0[(x + 4) % 6]
        else:
            dx, dy = s1[(x + 4) % 6]
    elif ch == 'c':
        if y % 2 == 0:
            dx, dy = s0[(x + 2) % 6]
        else:
            dx, dy = s1[(x + 2) % 6]
    x += dx
    y += dy
    t.add((x, y))
print(len(t))
```

## [C 1293 2種類の道路](https://yukicoder.me/problems/no/1293)

都市 i から行けるところを考える. 自動車専用道路を通って、都市 i から行ける都市の集合S(都市 i 自身も含む)に含まれる都市を j とすると、都市 j から歩行者専用道路通っていける都市(都市 j 自身も含む)の集合の和集合が、都市 i から行ける都市の集合となる. また当然ながら同じ集合Sに含まれる都市から行ける都市は全て同じである. つまり各集合S毎に、集合Sに含まれる都市の数だけ処理をするので *O*(*N*) となり解ける.

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

N, D, W = map(int, readline().split())

car = [-1] * N
for _ in range(D):
    a, b = map(lambda x: int(x) - 1, readline().split())
    unite(car, a, b)

walking = [-1] * N
for _ in range(W):
    c, d = map(lambda x: int(x) - 1, readline().split())
    unite(walking, c, d)

xs = [i for i in range(N) if car[i] < 0]
cs = {}
ss = {}
for x in xs:
    cs[x] = 0
    ss[x] = set()

for i in range(N):
    a = find(car, i)
    b = find(walking, i)
    if b in ss[a]:
        continue
    ss[a].add(b)
    cs[a] -= walking[b]

result = 0
for x in xs:
    result -= car[x] * (cs[x] - 1)
print(result)
```
