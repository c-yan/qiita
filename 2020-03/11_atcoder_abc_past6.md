# AtCoder Beginners Contest 過去問チャレンジ6

## [ABC090D - Remainder Reminder](https://atcoder.jp/contests/abc090/tasks/arc091_b)

a を b で割った余りが K 以上ということは、b は K + 1 以上なのが確定. b で割った余りは 0..b-1 で、この1周期に出現する K 以上の個数は b - K となる. a は N 以下なので、周期数は N / b となる. 後は周期の余りについて考える必要があるが、ところで N は1から始まるので、周期は 1, 2, ..., b - 1, 0 の順となる. つまり N % b が n の場合、1, 2, ..., n となり、K = 0 と K = 1 の個数が同じとなることに注意.

```python
N, K = map(int, input().split())

result = 0
for b in range(K + 1, N + 1):
    result += (N // b) * (b - K) + max(N % b - max(K - 1, 0), 0)
print(result)
```

## [ABC115D - Christmas](https://atcoder.jp/contests/abc115/tasks/abc115_d)

レベル L バーガーの層数とそれに含まれるパティの枚数は N≤50 なので簡単に計算できる(し、ループを回して計算しなくても簡単に求める数式は導出できる). あとは X が尽きるまで再帰的に進めば良い. レベル L バーガーに遭遇して、残ってる X がレベル L バーガーより大きければ、X をその層数だけ減らして、カウンタにそのパティ数を追加し、小さければレベル L - 1 バーガの中に入っていけば良い.

```python
N, X = map(int, input().split())


def f(N, X):
    result = 0

    if X >= 1:
        X -= 1
    else:
        return result

    if X >= 2 ** ((N - 1) + 2) - 3:
        X -= 2 ** ((N - 1) + 2) - 3
        result += 2 ** ((N - 1) + 1) - 1
    else:
        return result + f(N - 1, X)

    if X >= 1:
        X -= 1
        result += 1
    else:
        return result

    if X >= 2 ** ((N - 1) + 2) - 3:
        X -= 2 ** ((N - 1) + 2) - 3
        result += 2 ** ((N - 1) + 1) - 1
    else:
        return result + f(N - 1, X)

    if X >= 1:
        X -= 1
    else:
        return result

    return result


print(f(N, X))
```

## [ABC053D - Card Eater](https://atcoder.jp/contests/abc053/tasks/arc068_b)

ダブっているカードが3枚以上あれば、どう3枚選んでもカードの種類数を減らさずダブリを2枚減らせる. ダブっているカードが2枚の場合、片方のダブっているカード2枚ともう片方のダブっているカード1枚でカードの種類数を減らさずダブリを0にできる. ダブっているカードが1枚の場合はダブっているカード2枚とその他のカード1枚で、カードの種類数は1減るがダブリを0にできる. 結局のところ答えはカードの種類数から、ダブり数が奇数の場合は1、偶数の場合は0を引いたものとなる.

```python
N = int(input())
A = list(map(int, input().split()))

t = len(set(A))
if (len(A) - t) % 2 == 0:
    print(t)
else:
    print(t - 1)
```

## [ABC072D - Derangement](https://atcoder.jp/contests/abc072/tasks/arc082_b)

p<sub>i</sub> = i の場合には、p<sub>i</sub> と p<sub>i+1</sub> でスワップし、その回数を数えればいいだけ. p<sub>N</sub> = N のときだけ、一つ前の p<sub>N-1</sub>とスワップ. p<sub>i</sub> は 1..N の順列なので、スワップすれば必ず p<sub>i</sub> != i になるし、当然 p<sub>i+1</sub> != i なので p<sub>i+1</sub> も必ず大丈夫になる. 1回で処理できる連続する p<sub>i</sub> = i をバラバラに2回で処理しないためにも、順番にやるのが最善なのは明らか.

```python
N = int(input())
p = list(map(int, input().split()))

result = 0
for i in range(N - 1):
    if p[i] != i + 1:
        continue
    result += 1
    p[i], p[i + 1] = p[i + 1], p[i]
if p[N - 1] == N:
    result += 1
print(result)
```

## [ABC040D - 道路の老朽化対策について](https://atcoder.jp/contests/abc040/tasks/abc040_d)

どう考えてもサイズ付き UnionFind を使うことは明白で、後は年でソートして、順次道路を追加しながら、国民が行き来できる都市の個数を確認していくだけ.

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
    parent[j] += parent[i]
    parent[i] = j


setrecursionlimit(10 ** 5)

N, M = map(int, input().split())
roads = []
for _ in range(M):
    a, b, y = map(int, input().split())
    roads.append((y, a - 1, b - 1))
Q = int(input())
citizen = []
for i in range(Q):
    v, w = map(int, input().split())
    citizen.append((w, v - 1, i))

parent = [-1] * N

roads.sort(reverse=True)

results = [None] * Q
t = 0
for c in sorted(citizen, reverse=True):
    while t < M and roads[t][0] > c[0]:
        unite(parent, find(parent, roads[t][1]), find(parent, roads[t][2]))
        t += 1
    results[c[2]] = -parent[find(parent, c[1])]
print(*results, sep='\n')
```

## [ABC129E - Sum Equals Xor](https://atcoder.jp/contests/abc129/tasks/abc129_e)

L = XXXX の時、条件を満たす組の数を n とする. L = 1XXXX の時に条件を満たす組の数は 0～1111 で条件を満たす組の数と 10000～1XXXX で条件を満たす組の数の合計となる. ところで YYYY + ZZZZ = YYYY xor ZZZZ だとすると、1YYYY + ZZZZ = 1YYYY xor ZZZZ は成立するし、YYYY + 1ZZZZ = YYYY xor 1ZZZZ も成立する. つまり 10000～1XXXX で条件を満たす組の数は 2 * n となる.

L = 1 のとき条件を満たす組の数は (0,0),(0,1),(1,0) で3つである. L = 11 のとき条件を満たす組の数は上述の通り 0～1 = 3 と 10～11 = 2 * 3 の合計となり 3 * 3 = 9 となる. 同様に L = 111 は 9 * 3 = 27 となり、L = 1111 は 3<sup>4</sup> = 81 となる. 結果として L = 1XXXX のとき条件を満たす組の数は 2 * n + 3<sup>4</sup> となる.

ここまでくれば、下の桁から一桁づつ計算して、答えを求めることができる.

```python
L = input()

result = 1
t = 1
for c in L[::-1]:
    if c == '1':
        result = result * 2 + t
        result %= 1000000007
    t *= 3
    t %= 1000000007
print(result)
```
