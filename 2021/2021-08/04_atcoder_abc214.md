# AtCoder Beginner Contest 214 参戦記

過去最悪の順位をかろうじて更新しなくてすんだ程度の大爆死……….

## [ABC214A - New Generation ABC](https://atcoder.jp/contests/abc214/tasks/abc214_a)

1分半で突破. 書くだけ.

```python
N = int(input())

if N <= 125:
    print(4)
elif N <= 211:
    print(6)
else:
    print(8)
```

## [ABC214B - How many?](https://atcoder.jp/contests/abc214/tasks/abc214_b)

3分半で突破. 素直に3重ループしても *O*(10<sup>6</sup>) 程度で TLE しないので、素直にやれば良い.

```python
S, T = map(int, input().split())

result = 0
for a in range(101):
    for b in range(101):
        for c in range(101):
            if a + b + c > S:
                break
            if a * b * c > T:
                break
            result += 1
print(result)
```

## [ABC214C - Distribution](https://atcoder.jp/contests/abc214/tasks/abc214_c)

51分半で突破. TLE4、WA2. i番目のすぬけ君が初めて宝石をもらう時刻が最小となるスタート地点は1からNのどれか. 受け渡しリレーを2周すれば全てのすぬけくんに可能性があるスタート地点が必ず全て試されるので、2周回せば良い. 冷静に考えれば難しくなくて絶望.

```python
N = int(input())
S = list(map(int, input().split()))
T = list(map(int, input().split()))

result = T[:]
for _ in range(2):
    for i in range(N):
        j = (i + 1) % N
        x = result[i] + S[i]
        if result[j] > x:
            result[j] = x
print(*result, sep='\n')
```

## [ABC214D - Sum of Maximum Weights](https://atcoder.jp/contests/abc214/tasks/abc214_d)

突破できず. 重みの小さい頂点から順に繋いでいくと言われると「ああぁーーーっっっ」ってなりますね. 過去に類題解いているのに何故気づかない. C問題で爆死したのもやらかしだけど、こっちが解けなかったことのほうがやらかしな気がしてきた.

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

N = int(readline())
uvw = [tuple(map(int, readline().split())) for _ in range(N - 1)]

result = 0
parent = [-1] * (N + 1)
for u, v, w in sorted(uvw, key=lambda x: x[2]):
    x = -parent[find(parent, u)]
    y = -parent[find(parent, v)]
    result += x * y * w
    unite(parent, x, y)
print(result)
```
