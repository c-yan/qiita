# AtCoder Beginner Contest 218 参戦記

解けた問題数に不満はないが、解くのが遅いなあという感想.

## [ABC218A - Weather Forecast](https://atcoder.jp/contests/abc218/tasks/abc218_a)

1分半で突破. 書くだけ.

```python
N = int(input())
S = input()

if S[N - 1] == 'o':
    print('Yes')
else:
    print('No')
```

## [ABC218B - qwerty](https://atcoder.jp/contests/abc218/tasks/abc218_b)

3分で突破. ASCII コードと文字の変換ができれば余裕でしょう.

```python
P = map(int, open(0).read().split())

print(*(chr(ord('a') + p - 1) for p in P), sep='')
```

## [ABC218D - Rectangles](https://atcoder.jp/contests/abc218/tasks/abc218_d)

29分半で突破. (x0, y0) と (x0, y1) に呼応する点は (x1, y0), (x1, y1) なので、ある x0 に対して存在する y の集合と x1 に対して存在する y の集合の積集合の大きさを n とすると、<sub>n</sub>C<sub>2</sub> の組み合わせが作れる. 後はすべての x0 と x1 の組み合わせに対してそれを求めて合計すれば答えが出る. 回答時は無駄に座標圧縮をして時間を溶かしてしまった.

```python
N = int(input())
xy = [tuple(map(int, input().split())) for _ in range(N)]

d = {}
for x, y in xy:
    d.setdefault(x, set())
    d[x].add(y)
t = list(d.values())

result = 0
for i in range(len(t)):
    for j in range(i + 1, len(t)):
        n = len(t[i] & t[j])
        result += n * (n - 1) // 2
print(result)
```

## [ABC218C - Shapes](https://atcoder.jp/contests/abc218/tasks/abc218_c)

26分半で突破. ぱっと見面倒くさいなあと思ったが、それほどでもなかった. 先頭と末尾の空行を削除するトリム関数と右90度回転する回転関数を作って、SとTをトリムして、SとTを回転してトリムする. これで平行移動については考えなくてよくなったので、Tを回転しながらSと一致するかを4回判定する、ただそれだけだった.

```python
N = int(input())
S = [input() for _ in range(N)]
T = [input() for _ in range(N)]


def trim(a):
    while a[0] == '.' * len(a[0]):
        a = a[1:]
    while a[-1] == '.' * len(a[-1]):
        a = a[:-1]
    return a


def rotate(a):
    r = ['' for _ in range(len(a[0]))]
    for i in range(len(a[0])):
        for j in range(len(a) - 1, -1, -1):
            r[i] += a[j][i]
    return r


S = trim(S)
T = trim(T)
S = rotate(S)
T = rotate(T)
S = trim(S)
T = trim(T)

for i in range(4):
    T = rotate(T)
    if len(S) != len(T):
        continue
    if any(S[i] != T[i] for i in range(len(S))):
        continue
    print('Yes')
    break
else:
    print('No')
```

## [ABC218E - Destruction](https://atcoder.jp/contests/abc218/tasks/abc218_e)

17分半で突破. アルゴリズムを思いつくのに時間がかかり過ぎだった. この手の問題ではよくあるが、取り除くものを考えるのではなく、0から繋いでいくように考えると解ける. まずC<sub>i</sub>が負のものについては全て繋ぐ. 取り除く理由がまったくない. 後はC<sub>i</sub>が小さい方から順に必要があれば繋いでいく. 必要性の判断は辺の両端の頂点が既に別の辺経由で繋がっているかで判断できる. 判断は Union Find で簡単に実装できる.

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

N, M = map(int, readline().split())
ABC = [list(map(int, readline().split())) for _ in range(M)]

parent = [-1] * (N + 1)

result = 0
for a, b, c in sorted(ABC, key=lambda x: x[2]):
    if c > 0 and find(parent, a) == find(parent, b):
        result += c
        continue
    unite(parent, a, b)
print(result)
```

## [ABC218F - Blocked Roads](https://atcoder.jp/contests/abc218/tasks/abc218_f)

頂点1からNまで最短では多くてもたかだかN-1本の辺しか通らないので、そのN-1本についてだけ BFS で最短路計算しても TLE しないだろうと割とすぐ解き方は分かったのだが、残り10分では実装できないなと思って諦めた.
