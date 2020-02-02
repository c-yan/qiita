# yukicoder No.977 アリス仕掛けの摩天楼

まず、Alice 攻撃前の時点ですべての島が繋がっていたら Bob の勝ちです. Alice が爆破した橋を作り直せばいいのですから. 逆に島が3グループ以上になっていたら、Alice の勝ちです. Alice が何もしなくても、橋を1つ掛けるだけでは連結状態になりません. 勝負が分かれるのが、島が2グループに分かれていた場合で、Alice の爆破で3グループに変化すれば Alice の勝ちです. 橋が一本でしか繋がっていない島はその橋を爆破されると別グループになってしまうので、そういう島があるかをチェックすれば解答できます.

島が何グループかは Union Find で簡単にわかります. 島に何本橋がかかっているかは素直に集計すればよいです.

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

N = int(input())

parent = [-1] * N
edges = [0] * N
for _ in range(N - 1):
    u, v = map(int, input().split())
    unite(parent, u, v)
    edges[u] += 1
    edges[v] += 1

# Alice が橋を爆破する前のグループ数
groups = sum(1 for e in parent if e < 0)

# Alice が橋を1つ爆破した後のグループ数
if 1 in edges:
    groups += 1

# Bob が橋を1つ掛けた後のグループ数
if groups != 1:
    groups -= 1

if groups == 1:
    print('Bob')
else:
    print('Alice')
```
