# AtCoder Beginner Contest 206 参戦記

開始から30秒くらい、始まっていることに気づかずに無駄にした. 今回も無事爆死して、やっぱりウマ娘にかまけて精進を怠った3ヶ月は裏切らないなあと思いました.

## [ABC206A - Maxi-Buying](https://atcoder.jp/contests/abc206/tasks/abc206_a)

3分で突破. 書くだけ.

```python
N = int(input())

t = N * 108 // 100
if t < 206:
    print('Yay!')
elif t == 206:
    print('so-so')
elif t > 206:
    print(':(')
```

## [ABC206B - Savings](https://atcoder.jp/contests/abc206/tasks/abc206_b)

1分半で突破. 1からnまでの合計が n(n+1)/2 なのが分かっていれば、10<sup>9</sup> には 10<sup>5</sup> 未満で到達することが分かり、シングルループで素直に積算していっても大丈夫だと分かる.

```python
N = int(input())

c = 0
for i in range(1, 10 ** 9):
    c += i
    if c < N:
        continue
    print(i)
    break
```

## [ABC206C - Swappable](https://atcoder.jp/contests/abc206/tasks/abc206_c)

9分で突破. 間違って A<sub>i</sub>=A<sub>j</sub> を数え上げて、あれ数が合わないとかやって時間を無駄にした……. 2重ループだと間に合わないことは明らか. 個数を数え上げればシングルループになると気づくのは難しくない,

```python
from collections import Counter

N, *A = map(int, open(0).read().split())

t = 0
for c in Counter(A).values():
    t += c * (N - c)
print(t // 2)
```

## [ABC206D - KAIBUNsyo](https://atcoder.jp/contests/abc206/tasks/abc206_d)

46分半で突破、WA2. 過去に似たような問題絶対やってるよなあ. ざっくり言えば、各疎になっているグループ毎に、グループ内の数字の種類数-1を積算したものが答え. 入出力例はグループが複数のものがなく、WA を出してからうんうん唸ってようやく気づいた.

```
6
1 3 2 4 1 5
```

を入力して

```
3
```

が出力できれば OK ですね.

グループ判定は Union Find 以外思いつかなかった.

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


setrecursionlimit(10 ** 6)

N, *A = map(int, open(0).read().split())

ma = max(A)

parent = [-1] * (ma + 1)
for i in range(N // 2):
    unite(parent, A[i], A[N - 1 - i])

result = 0
for i in range(ma + 1):
    if parent[i] > 0:
        continue
    result += -parent[i] - 1
print(result)
```
