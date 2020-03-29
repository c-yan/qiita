# AtCoder Beginners Contest 過去問チャレンジ2

## [ABC129D - Lamp](https://atcoder.jp/contests/abc129/tasks/abc129_d)

特に難しいことはなくて、縦方向と横方向で別々に照らされる範囲のテーブルを作って、最後にそれぞれのテーブルの値の和 -1 (現在位置が2回カウントされる分を引く) の最大値を求めればいい. ただこういう配列を延々と嘗める処理は Python では絶望的に遅く TLE の嵐. まあ、PyPy で余裕の AC なんですが. それでも悔しくて Python で通すために最適化. 3回スキャンすると時間がかかるので、横方向だけテーブルを埋めて、縦方向をスキャンしてる時にオンザフライで最大値を求めていくようにしたらなんとか AC.

その後、他人の回答を見ていたら、配列の部分列に代入をする場合、スライスに同じ長さのリストを代入するのが速いと分かった. 毎回リストを生成する部分が重そうな気がしたが、その部分はC言語で書かれた機械語で行われるので Python で頑張るより速いというオチのようである. `yokoi[start:j] = [t] * t` がそれを使っている部分である. この問題以外でこの知識を使うことはあるだろうかw.

```python
def main():
    from sys import stdin
    from builtins import range

    readline = stdin.readline

    H, W = map(int, readline().split())
    S = [readline()[:-1] + '#' for _ in range(H)]

    S.append('#' * W)
    ft = [[i] * i for i in range(100)]
    yoko = [[0] * W for _ in range(H)]
    for i in range(H):
        start = -1
        si = S[i]
        yokoi = yoko[i]
        for j in range(W + 1):
            if si[j] == '#':
                if start != -1:
                    t = j - start
                    if t < 100:
                        yokoi[start:j] = ft[t]
                    else:
                        yokoi[start:j] = [t] * t
                    start = -1
            else:
                if start == -1:
                    start = j

    result = 0
    for i in range(W):
        start = -1
        for j in range(H + 1):
            if S[j][i] == '#':
                if start != -1:
                    t = yoko_max + j - start - 1
                    if t > result:
                        result = t
                    start = -1
            else:
                yji = yoko[j][i]
                if start == -1:
                    start = j
                    yoko_max = yji
                else:
                    if yji > yoko_max:
                        yoko_max = yji
    print(result)


main()
```

## [ABC128D - equeue](https://atcoder.jp/contests/abc128/tasks/abc128_d)

K 回の操作の全組み合わせ. 入れたものを再度取り出してもしょうがないので、ガーッと左から連続的に取って、ガーッと連続的に右から取って、手持ちの一番低いほうから順に0未満を筒に詰める(左でも、右でもどっちでも良い、違いはない)といった感じである. 何も考えなくても TLE が出ないので、楽ちん.

```python
N, K = map(int, input().split())
V = list(map(int, input().split()))

m = min(N, K)
result = 0
for i in range(m + 1):
    for j in range(i + 1):
        t = V[:j]
        t.extend(V[N - (i - j):])
        t.sort(reverse=True)
        l = K - i
        while len(t) > 0 and l > 0 and t[-1] < 0:
            t.pop()
            l -= 1
        result = max(result, sum(t))
print(result)
```

## [ABC127D - Integer Cards](https://atcoder.jp/contests/abc127/tasks/abc127_d)

真面目に書き換えるのはめんどくさいので、書き換え後の値を大きい方から順に並べて、その数を書き換え数分追加して、追加数が n を超えたら、ソートして、大きい方から n 個とればいいかなって.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))
BC = [list(map(int, input().split())) for _ in range(M)]

BC.sort(key=lambda x: x[1], reverse=True)
t = 0
for B, C in BC:
    A.extend([C] * B)
    t += B
    if t > N:
        break
A.sort(reverse=True)
print(sum(A[:N]))
```

## [ABC084D - 2017-like Number](https://atcoder.jp/contests/abc084/tasks/abc084_d)

まず素数判定用のテーブルを作る. エラトステネスの篩. その次に 2～10<sup>5</sup> までの累積和のテーブルを作る. 後は 2～r までの累積和から、2～l までの累積和を引けば答えが簡単に出る.

```python
from math import sqrt

max_lr = 10 ** 5

sieve = [True] * (max_lr + 1)
sieve[0] = False
sieve[1] = False
for i in range(2, int(sqrt(max_lr)) + 1):
    if not sieve[i]:
        continue
    for j in range(i * i, max_lr + 1, i):
        sieve[j] = False

cs = [0] * (max_lr + 1)
for i in range(3, max_lr + 1, 2):
    if sieve[i] and sieve[(i + 1) // 2]:
        cs[i] = 1
for i in range(1, max_lr + 1):
    cs[i] += cs[i - 1]

Q = int(input())
for _ in range(Q):
    l, r = map(int, input().split())
    print(cs[r] - cs[l - 1])
```

## [ABC054D - Mixing Experiment](https://atcoder.jp/contests/abc054/tasks/abc054_d)

DP で総当りするだけです(終). といいつつ、時系列無しテーブルで解いたので、これは本当に DP なのか? その回のものを2回使わないようにループを降順に回します.

```python
INF = float('inf')

N, Ma, Mb = map(int, input().split())

cmax = N * 10
t = [[INF] * (cmax + 1) for _ in range(cmax + 1)]
for _ in range(N):
    a, b, c = map(int, input().split())
    for aa in range(cmax, 0, -1):
        for bb in range(cmax, 0, -1):
            if t[aa][bb] == INF:
                continue
            t[aa + a][bb + b] = min(t[aa + a][bb + b], t[aa][bb] + c)
    t[a][b] = min(t[a][b], c)

result = INF
for a in range(1, cmax):
    for b in range(1, cmax):
        if a * Mb == b * Ma:
            result = min(result, t[a][b])
if result == INF:
    result = -1
print(result)
```

## [ABC049D - 連結 / Connectivity](https://atcoder.jp/contests/abc049/tasks/arc065_b)

道路の連結を計算し、鉄道の連結を計算し、都市 i の道路グループID、鉄道グループIDの2要素のタプルを列挙して集計して、集計結果を都市ごとに出力すれば OK. 出題者の想定通りに Union Find を使えば、根のノード番号がグループ ID になり、簡単かつ高速に解ける. 自分は [素集合データ構造](https://ja.wikipedia.org/wiki/%E7%B4%A0%E9%9B%86%E5%90%88%E3%83%87%E3%83%BC%E3%82%BF%E6%A7%8B%E9%80%A0) を見ながら Union Find を書きました. path compression(経路圧縮)は入れないと速度的にむりーだが、こんな軽くて簡単なものを入れないやつの頭はおかしい.

```python
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


N, K, L = map(int, input().split())

roads = [-1] * N
rails = [-1] * N

for i in range(K):
    p, q = map(int, input().split())
    unite(roads, p - 1, q - 1)

for i in range(L):
    r, s = map(int, input().split())
    unite(rails, r - 1, s - 1)

d = {}
for i in range(N):
    t = (find(roads, i), find(rails, i))
    if t in d:
        d[t] += 1
    else:
        d[t] = 1

print(*[d[(find(roads, i), find(rails, i))] for i in range(N)])
```

と書いているが、この問題を解く前は Union Find なぞ知らなかったので、普通の探索で最初は書いてて TLE を出しまくったのだ. Union Find 版で AC をゲットした後も未練たらしく探索版を修正していたらなんか AC がゲットできてしまったw. よって、この問題においては Union Find は必須ではない.

```python
def create_links(N, K):
    links = [[] for _ in range(N)]
    for _ in range(K):
        p, q = map(int, input().split())
        links[p - 1].append(q - 1)
        links[q - 1].append(p - 1)
    return links


def create_groups(N, links):
    groups = list(range(N))
    for i in range(N):
        if groups[i] != i:
            continue
        q = [i]
        while q:
            j = q.pop()
            for k in links[j]:
                if groups[k] != i:
                    groups[k] = i
                    q.append(k)
    return groups


N, K, L = map(int, input().split())

road_links = create_links(N, K)
rail_links = create_links(N, L)
road_groups = create_groups(N, road_links)
rail_groups = create_groups(N, rail_links)

d = {}
for i in range(N):
    t = (road_groups[i], rail_groups[i])
    if t in d:
        d[t] += 1
    else:
        d[t] = 1

print(*[d[(road_groups[i], rail_groups[i])] for i in range(N)])
```
