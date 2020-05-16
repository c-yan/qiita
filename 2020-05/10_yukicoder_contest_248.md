# yukicoder contest 248 参戦記

## [A 1052 電子機器X](https://yukicoder.me/problems/no/1052)

29分で突破. N=10,11 について K=1～8 まで手で求めたらようやく分かりました(笑). N が偶数のときは、Kが奇数のときは偶数、Kが偶数のときは奇数しかでてこないので、最大が N / 2 になります. N が奇数のときは、偶数奇数両方とも出てくるので最大 N になります.

```python
N, K = map(int, input().split())

if N % 2 == 0:
    print(min(K + 1, N // 2))
else:
    print(min(K + 1, N))
```

## [B 1053 ゲーミング棒](https://yukicoder.me/problems/no/1053)

突破できず. WA 2個まで AC した後、3つに分かれているやつが一つでもあれば無理というのが抜けたまま、他に条件を満たすことが出来るパターンがあるのかと悩んで終了(笑). 条件を満たせることが出来るパターンは以下の2つ.

1. 色が最初から分かれていない (回数は0回)
2. 2つに分かれているのが1つだけあり、それが先頭と末尾 (回数は1回)

```python
N = int(input())
A = list(map(int, input().split()))

d = {}
prev = -1
for a in A:
    if prev != a:
        d.setdefault(a, 0)
        d[a] += 1
    prev = a

if all(v == 1 for v in d.values()):
    print(0)
    exit()

if any(v >= 3 for v in d.values()):
    print(-1)
    exit()

if len([v for v in d.values() if v == 2]) == 1 and A[0] == A[-1]:
    print(1)
else:
    print(-1)
```

## [C 1054 Union add query](https://yukicoder.me/problems/no/1054)

突破できず. [ABC138D - Ki](https://atcoder.jp/contests/abc138/tasks/abc138_d) みたいにまとめて足せるようにするんだろうなあとは思ったが、残り時間的に実装は無理で…….

追記: 解説どおりに Union Find 木を改造して実装. unite するときに、傘下に加わる親の値から、親の値を引くというのが言われてみればなるほどだった. 新しい親の分だけ数字が増えるのどうしようとか悩んでいた自分が間抜けすぎる. 解説では経路圧縮を抜いても大丈夫だと書かれていたが、Python は遅いし、経路圧縮を残すのがそれほど難しいとも思わなかったので、残して実装した. AC はしたが、割と時間ギリギリだったので残して正解だったのではなかろうか.

改造内容であるが find に親情報だけではなく、値の合計の情報を渡すようにした. そして、find は親の番号だけではなく、親にたどり着く途中の親の値の合計も返すようにした. 経路圧縮するときは、この途中の合計を自分の合計に足し込む必要がある. unite 時は前述の通り、傘下に入る親の値の合計から親の値の合計を引いて相殺する. 各ノードの値は、そのノードの値の合計と親の値の合計となる(経路圧縮の効果により).

```python
from sys import setrecursionlimit
from sys import stdin


def find(parent, amount, i):
    t = parent[i]
    if t < 0:
        return i, 0
    t, a = find(parent, amount, t)
    parent[i] = t
    amount[i] += a
    return t, amount[i]


def unite(parent, amount, i, j):
    i, _ = find(parent, amount, i)
    j, _ = find(parent, amount, j)
    if i == j:
        return
    parent[j] += parent[i]
    parent[i] = j
    amount[i] -= amount[j]


setrecursionlimit(10 ** 6)
readline = stdin.readline

N, Q = map(int, readline().split())

parent = [-1] * (N + 1)
amount = [0] * (N + 1)

result = []
for _ in range(Q):
    T, A, B = map(int, readline().split())
    if T == 1:
        unite(parent, amount, A, B)
    elif T == 2:
        j, _ = find(parent, amount, A)
        amount[j] += B
    elif T == 3:
        j, a = find(parent, amount, A)
        result.append(amount[j] + a)
print('\n'.join(str(v) for v in result))
```
