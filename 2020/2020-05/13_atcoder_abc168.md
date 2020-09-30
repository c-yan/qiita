# AtCoder Beginner Contest 168 参戦記

700番切ったの初めてなのでイヤッッホォォォオオォオウ!! 入水!!!

## [ABC168A - ∴ (Therefore)](https://atcoder.jp/contests/abc168/tasks/abc168_a)

2分で突破. 書くだけ.

```python
N = int(input())

if N % 10 in [2, 4, 5, 7, 9]:
    print('hon')
elif N % 10 in [0, 1, 6, 8]:
    print('pon')
elif N % 10 in [3]:
    print('bon')
```

## [ABC168B - ... (Triple Dots)](https://atcoder.jp/contests/abc168/tasks/abc168_b)

2分で突破. 書くだけ.

```python
K = int(input())
S = input()

if len(S) <= K:
    print(S)
else:
    print(S[:K] + '...')
```

## [ABC168C - : (Colon)](https://atcoder.jp/contests/abc168/tasks/abc168_c)

9分半で突破. 分でも時針が動くことを忘れているアホが発生. 数学は苦手だけど流石にこのレベルならなんとか…….

```python
from math import pi, sin, cos, sqrt

A, B, H, M = map(int, input().split())

x1 = A * cos(2 * pi * (H + M / 60) / 12)
y1 = A * sin(2 * pi * (H + M / 60) / 12)
x2 = B * cos(2 * pi * M / 60)
y2 = B * sin(2 * pi * M / 60)

print(sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)))
```

## [ABC168D - .. (Double Dots)](https://atcoder.jp/contests/abc168/tasks/abc168_d)

9分半で突破. 問題としては入り口から順に幅優先探索するだけで簡単なのだが、'No' の場合は無いんだと心を強く持って提出するのがきつかった.

```python
from collections import deque

N, M = map(int, input().split())
AB = [map(int, input().split()) for _ in range(M)]

links = [[] for _ in range(N + 1)]
for a, b in AB:
    links[a].append(b)
    links[b].append(a)

result = [-1] * (N + 1)
q = deque([1])
while q:
    i = q.popleft()
    for j in links[i]:
        if result[j] == -1:
            result[j] = i
            q.append(j)
print('Yes')
print('\n'.join(str(i) for i in result[2:]))
```

## [ABC168E - ∙ (Bullet)](https://atcoder.jp/contests/abc168/tasks/abc168_e)

解けそうで解けなかった. 多分 A<sub>i</sub> か B<sub>i</sub> のどちらかが0の時の場合が詰めきれてない.

追記: (A<sub>i</sub>, B<sub>i</sub>) = (0, 0) の特別扱いの仕方が間違ってた. (0, 0) はすべての魚と仲が悪いので後から足し込まないと駄目だった.

b/a と -a/b の仲が悪いのは分かるが、残念ながら Python には有理数がないので (a, b) のタプルで代用する. ところで (1, 2) と (-4, 2) は仲が悪いがそれがぱっとわからないので、a, b それぞれを gcd(a, b) で割って通分しておく. こうすると (1, 2) と (-2, 1) になるので、(a, b) と仲が悪いのが (-b, a), (b, -a) だけに絞れる. また (-b, a), (b, -a) は (-a, -b) とも仲が悪いので、(a, b) と (-a, -b) はグループとなる. あとは存在する (a, b) 毎に場合の数は 2^((a, b) の個数 + (-a, -b) の個数) + 2^((-b, a) の個数 + (b, -a) の個数) - 1 (最後の -1 は無いがダブっている分) となるので乗算していき、最後に (0, 0) の個数を足し、一個も選ばなかった分を1つ差っ引けば答えになる.

理屈は簡単だけど、実装はカロリー高くてめんどくさいです…….

```python
from math import gcd

N = int(input())
AB = [map(int, input().split()) for _ in range(N)]

t = []
d = {}
d[0] = {}
d[0][0] = 0
for a, b in AB:
    i = gcd(a, b)
    if i != 0:
        a //= i
        b //= i
    t.append((a, b))
    d.setdefault(a, {})
    d[a].setdefault(b, 0)
    d[a][b] += 1

used = set()
result = 1
for a, b in t:
    if (a, b) in used:
        continue
    used.add((a, b))
    if a == 0 and b == 0:
        continue
    i = d[a][b]
    j, k, l = 0, 0, 0
    if -a in d and -b in d[-a]:
        j = d[-a][-b]
        used.add((-a, -b))
    if -b in d and a in d[-b]:
        k = d[-b][a]
        used.add((-b, a))
    if b in d and -a in d[b]:
        l = d[b][-a]
        used.add((b, -a))
    result *= pow(2, i + j, 1000000007) + pow(2, k + l, 1000000007) - 1
    result %= 1000000007
result += d[0][0] - 1
result %= 1000000007
print(result)
```
