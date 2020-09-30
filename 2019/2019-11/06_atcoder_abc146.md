# AtCoder Beginner Contest 146 参戦記

## [ABC146A - Can't Wait for Holiday](https://atcoder.jp/contests/abc146/tasks/abc146_a)

2分半で突破. 書くだけ.

```python
S = input()

if S == 'SUN':
    print(7)
elif S == 'MON':
    print(6)
elif S == 'TUE':
    print(5)
elif S == 'WED':
    print(4)
elif S == 'THU':
    print(3)
elif S == 'FRI':
    print(2)
elif S == 'SAT':
    print(1)
```

## [ABC146B - ROT N](https://atcoder.jp/contests/abc146/tasks/abc146_b)

4分半で突破. 書くだけ.

```python
N = int(input())
S = input()

print(''.join(chr((ord(c) - ord('A') + N) % 26 + ord('A')) for c in S))
```

## [ABC146C - Buy an Integer](https://atcoder.jp/contests/abc146/tasks/abc146_c)

16分で突破. 一目にぶたんで、最近やったなーと [ABC144E - Gluttony](https://atcoder.jp/contests/abc144/tasks/abc144_e) のソースコードをコピってきて、is_ok を書き直して、調整してポイ. 調整に手間取ったのでまだめぐる式が手についてないなあという感想.

```python
A, B, X = map(int, input().split())


def is_ok(N):
    return A * N + B * len(str(N)) <= X


ok = 0
ng = 10 ** 9 + 1
while ng - ok > 1:
    m = (ok + ng) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m
print(ok)
```

## [ABC146D - Coloring Edges on Tree](https://atcoder.jp/contests/abc146/tasks/abc146_d)

敗退. 幅優先探索というアルゴリズム方針はあっていたようなのだが……. 最初に書いたのは TLE を食らい、書き直したのはデータ構造が駄目で MLE を食らったので…….

うーん、77分もあってこれが通せないのは流石に情けない感.

```python
from sys import setrecursionlimit


def genid(a, b):
    if b < a:
        a, b = b, a
    return a * 100000 + b


def paint(currentNode, usedColor, parentNode, edges, colors):
    color = 1
    for childNode in edges[currentNode]:
        if childNode == parentNode:
            continue
        if color == usedColor:
            color += 1
        colors[genid(currentNode, childNode)] = color
        paint(childNode, color, currentNode, edges, colors)
        color += 1


setrecursionlimit(100000)

N = int(input())
ab = [list(map(int, input().split())) for _ in range(N - 1)]

edges = [[] for _ in range(N)]
for a, b in ab:
    edges[a - 1].append(b - 1)
    edges[b - 1].append(a - 1)

colors = {}
paint(0, -1, -1, edges, colors)

print(max(len(e) for e in edges))
for a, b in ab:
    print(colors[genid(a - 1, b - 1)])
```
