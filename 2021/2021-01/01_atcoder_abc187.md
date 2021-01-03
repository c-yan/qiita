# AtCoder Beginner Contest 187 参戦記

## [ABC187A - Large Digits](https://atcoder.jp/contests/abc187/tasks/abc187_a)

2分で突破. 書くだけ.

```python
def S(n):
    return sum(int(c) for c in str(n))


A, B = map(int, input().split())

print(max(S(A), S(B)))
```

## [ABC187B - Gentle Pairs](https://atcoder.jp/contests/abc187/tasks/abc187_b)

4分半で突破. いつもどおり、二点を通る直線の方程式をググるところからスタート. 除算したらまずいかなってちょっと思ったけど、まあB問題だし大丈夫かなと腐った読みで出してセーフ.

```python
N = int(input())
xy = [tuple(map(int, input().split())) for _ in range(N)]

result = 0
for i in range(N):
    for j in range(i + 1, N):
        x1, y1 = xy[i]
        x2, y2 = xy[j]
        if x1 == x2:
            continue
        a = (y2 - y1) / (x2 - x1)
        if abs(a) <= 1:
            result += 1
print(result)
```

## [ABC187C - 1-SAT](https://atcoder.jp/contests/abc187/tasks/abc187_c)

5分半で突破. 制限を見ずに書いたあとで、こんなんだと流石に無理かなと一瞬思ったけど、|S<sub>i</sub>|≦10 だったので「あ、大丈夫だ」となって AC.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
S = [readline()[:-1] for _ in range(N)]

t = set()
for s in S:
    if s[0] == '!':
        if s[1:] in t:
            print(s[1:])
            break
        t.add(s)
    else:
        if '!' + s in t:
            print(s)
            break
        t.add(s)
else:
    print('satisfiable')
```

## [ABC187D - Choose Me](https://atcoder.jp/contests/abc187/tasks/abc187_d)

13分で突破. 一瞬全然わからなかったけど「Sum(A<sub>i</sub>) vs 0」で開始して、町<sub>j</sub>で演説をすると「Sum(A<sub>i</sub>) - A<sub>j</sub> vs A<sub>j</sub> + B<sub>j</sub>」になるから、移項すると「Sum(A<sub>i</sub>) vs 2A<sub>j</sub> + B<sub>j</sub>」と分かる. なので、「2A<sub>j</sub> + B<sub>j</sub>」な数列を作って降順ソートして、順番に足していけばいいと分かり、*O*(<i>N</i>log<i>N</i>) になったのでこれで解けた.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
AB = [tuple(map(int, readline().split())) for _ in range(N)]

s = sum(a for a, _ in AB)
t = [a * 2 + b for a, b in AB]
t.sort(reverse=True)

c = 0
for i in range(N):
    c += t[i]
    if c > s:
        print(i + 1)
        break
```

## [ABC187E - Through Path](https://atcoder.jp/contests/abc187/tasks/abc187_e)

突破できず. コンテスト終了から21分20秒で自力で解くことができた(96分半). 脳内で頂点 a<sub>e<sub>i</sub></sub> と頂点 b<sub>e<sub>i</sub></sub> が隣接していない問題に書き換えてしまって自爆した. 計算量を落とす方法は [ABC138D - Ki](https://atcoder.jp/contests/abc138/tasks/abc138_d) と同じで親ノードに足し込んで、最後に合算すればいいよねと思った. それと辻褄が合うように考えると、辿り元が親の場合には根の頂点に +x、ブロックする頂点に -x、辿り元が子の場合には辿り元の頂点に +x すればよい. 後はどっちが親かについては根を勝手に決めて、そこからトラバースして親情報を配列に入れておけば *O*(1) で処理できる. どの頂点を根にしても問題ないので、頂点1を根にして計算した.

```python
from sys import stdin
from collections import deque

readline = stdin.readline

N = int(readline())
ab = [tuple(map(lambda x: int(x) - 1, readline().split())) for _ in range(N - 1)]

links = [[] for _ in range(N)]
for a, b in ab:
    links[a].append(b)
    links[b].append(a)

parent = [-1] * N
parent[0] = 0
q = deque([0])
while q:
    i = q.popleft()
    for j in links[i]:
        if parent[j] != -1:
            continue
        parent[j] = i
        q.append(j)

c = [0] * N
Q = int(readline())
for _ in range(Q):
    t, e, x = map(int, readline().split())
    a, b = ab[e - 1]
    if t == 2:
        a, b = b, a
    if a == parent[b]:
        c[0] += x
        c[b] -= x
    else:
        c[a] += x

q = deque([0])
while q:
    i = q.popleft()
    for j in links[i]:
        if j == parent[i]:
            continue
        c[j] += c[i]
        q.append(j)

print(*c, sep='\n')
```

追記: 親情報じゃなくて深さ情報を持っている人が多いですね.

```python
from sys import stdin
from collections import deque

readline = stdin.readline

N = int(readline())
ab = [tuple(map(lambda x: int(x) - 1, readline().split())) for _ in range(N - 1)]

links = [[] for _ in range(N)]
for a, b in ab:
    links[a].append(b)
    links[b].append(a)

depth = [-1] * N
depth[0] = 0
q = deque([0])
while q:
    i = q.popleft()
    for j in links[i]:
        if depth[j] != -1:
            continue
        depth[j] = depth[i] + 1
        q.append(j)

c = [0] * N
Q = int(readline())
for _ in range(Q):
    t, e, x = map(int, readline().split())
    a, b = ab[e - 1]
    if t == 2:
        a, b = b, a
    if depth[a] < depth[b]:
        c[0] += x
        c[b] -= x
    else:
        c[a] += x

q = deque([0])
while q:
    i = q.popleft()
    for j in links[i]:
        if depth[j] < depth[i]:
            continue
        c[j] += c[i]
        q.append(j)

print(*c, sep='\n')
```
