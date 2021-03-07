# yukicoder contest 285 参戦記

★2.5とはいえども4つもあればどれかは解けるんじゃないかと思って考えていたら意識が飛んで、起きたら25時過ぎだった(笑).

## [A 1415 100の倍数かつ正整数(1)](https://yukicoder.me/problems/no/1415)

書くだけ……といいつつ、最初はちゃんと問題文を読んでいなくて、`print(abs(N) // 100)` で出して WA を食らった(馬鹿). まあ、100で割れば答えが出ますよね.

```python
N = int(input())

if N < 0:
    print(0)
else:
    print(N // 100)
```

## [B 1416 ショッピングモール](https://yukicoder.me/problems/no/1416)

一目で店員数が多い店から入れていけばいいことは分かるはず.

```python
from math import log2

n, *A = map(int, open(0).read().split())

A.sort(reverse=True)

result = 0
for i in range(n):
    result += int(log2(i + 1)) * A[i]
print(result)
```

## [F 1420 国勢調査 (Easy) ](https://yukicoder.me/problems/no/1420)

コンテスト後に解いた. 連立一次方程式と同じで、関係する未知数のどれかが1個分かればドミノ倒しのように全部定まるので、まず Union Find で関係しているグループを求める. グループが求めれたらどれか一つを0として、ドミノ倒しで全部求めれば良い. 求まったら最後に矛盾したものがなかったかをチェックして、矛盾があったら -1 を、無かったら求まった答えを出力する.

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

parent = [-1] * N
d = {i: {} for i in range(N)}
vs = []
for _ in range(M):
    A, B = map(lambda x: int(x) - 1, readline().split())
    Y = int(readline())
    vs.append((A, B, Y))
    d[A][B] = Y
    d[B][A] = Y
    unite(parent, A, B)

result = [-1] * N
q = [i for i in range(N) if parent[i] < 0]
for i in q:
    result[i] = 0
while q:
    a = q.pop()
    for b in d[a]:
        if result[b] != -1:
            continue
        result[b] = result[a] ^ d[a][b]
        q.append(b)

for A, B, Y in vs:
    if result[A] ^ result[B] == Y:
        continue
    print(-1)
    exit()
print(*result, sep='\n')
```
