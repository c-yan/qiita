# AtCoder 第三回 アルゴリズム実技検定 参戦記

無料だし、受けてほしいと chokudai さんがあーだこーだーで言っていたので、過去二回とは違い本物に参加. 結果は70点の中級. 第一回が64点、第二回が76点なので、ちょうど中間. 過去二回同様に早朝にやっていたのだが、睡眠時間が3時間だったので、途中から眠くて仕方がなかった. やたら WA が多かったのはそのせいかもしれないなあとは思うが、かと言ってスッキリした頭でも解けて後一問だろうから、別に良いかなって感じ.

* [AtCoder 第二回 アルゴリズム実技検定 バーチャル参戦記](https://qiita.com/c-yan/items/8b1ddcb4e8fa4d3443ed)
* [AtCoder 第一回 アルゴリズム実技検定 バーチャル参戦記](https://qiita.com/c-yan/items/578f3234de4113cb3a6b)

## [past202005A - ケース・センシティブ](https://atcoder.jp/contests/past202005-open/tasks/past202005_a)

2分で突破. 書くだけですね.

```python
s = input()
t = input()

if s == t:
    print('same')
elif s.lower() == t.lower():
    print('case-insensitive')
else:
    print('different')
```

## [past202005B - ダイナミック・スコアリング](https://atcoder.jp/contests/past202005-open/tasks/past202005_b)

10分半で突破. B問題なのにもう結構難しい!? 最初、yukicoder 式の早く解いた人が得点が高いタイプだと勘違いしたせいで無駄に時間がかかっている.

```python
N, M, Q = map(int, input().split())

score = [N] * (M + 1)
answered = [[False] * (M + 1) for _ in range(N + 1)]
for _ in range(Q):
    s = list(map(int, input().split()))
    if s[0] == 1:
        t = 0
        for i in range(1, M + 1):
            if answered[s[1]][i]:
                t += score[i]
        print(t)
    elif s[0] == 2:
        score[s[2]] -= 1
        answered[s[1]][s[2]] = True
```

## [past202005C - 等比数列](https://atcoder.jp/contests/past202005-open/tasks/past202005_c)

13分半で突破. TLE2、WA1. C問題なのにナイーブに書くと TLE!? 慌てて Go 言語用の *O*(log<i>N</i>) 実装の modpow を持ってきて、Python に書き直したらバグらせて WA を喰らいつつも AC.

解説 PDF の参考実装、意味もなく setrecursionlimit してるし、sys.stdin.readline じゃなくて sys.stdin.buffer.readline を使っているのもよくわからないし、`a * r ** (n -1)` って最大 10<sup>9 + 9<sup>29</sup></sup> だけど良いんだろうか、イマイチって思っちゃう. *O*(log<i>N</i>) の階乗を書くのを回避したいってことかな.

```python
A, R, N = map(int, input().split())

result = A
a = R
b = N - 1
while b != 0 and result <= 1000000000:
    if b & 1 != 0:
        result *= a
    a *= a
    b >>= 1

if result > 1000000000:
    print('large')
else:
    print(result)
```

## [past202005D - 電光掲示板](https://atcoder.jp/contests/past202005-open/tasks/past202005_d)

9分半で突破. 一文字づつ切り出して、マッチングするだけだからさほど難しくなかった.

```python
N = int(input())
s = [input() for _ in range(5)]

a = [
    '.###..#..###.###.#.#.###.###.###.###.###.',
    '.#.#.##....#...#.#.#.#...#.....#.#.#.#.#.',
    '.#.#..#..###.###.###.###.###...#.###.###.',
    '.#.#..#..#.....#...#...#.#.#...#.#.#...#.',
    '.###.###.###.###...#.###.###...#.###.###.'
]

result = []
for i in range(N):
    t = [s[j][i * 4:(i + 1) * 4] for j in range(5)]
    for j in range(10):
        for k in range(5):
            if t[k] != a[k][j * 4:(j + 1) * 4]:
                break
        else:
            result.append(j)
            break
print(''.join(str(i) for i in result))
```

## [past202005E - スプリンクラー](https://atcoder.jp/contests/past202005-open/tasks/past202005_e)

8分半で突破. 問題文を読んでも起動中のスプリンクラーの色を変えた場合周りの色が変わるのか分からんなあと思いつつ、WA になったら変えればいいかと考えて提出して AC. 後から考えると、起動中のスプリンクラーが隣接した場合に困るから、実際は起動して停止して次のクエリってことなんだろうな. links はもう書くの何度目だって感じで、悩むところは何もなし.

```python
N, M, Q = map(int, input().split())

links = [[] for _ in range(N + 1)]
for _ in range(M):
    u, v = map(int, input().split())
    links[u].append(v)
    links[v].append(u)
c = [0] + list(map(int, input().split()))

for _ in range(Q):
    s = list(map(int, input().split()))
    if s[0] == 1:
        x = s[1]
        print(c[x])
        for y in links[x]:
            c[y] = c[x]
    if s[0] == 2:
        x, y = s[1:]
        print(c[x])
        c[x] = y
```

## [past202005G - グリッド金移動](https://atcoder.jp/contests/past202005-open/tasks/past202005_g)

40分半?で突破. WA2. 到達不能のときに -1 を出力するのを忘れて無限大を出力していた. 他にも金将の動きであると分かっていなくて、6種類ある移動のうち上下左右の4種類しか実装してなくて、入力例が通らずに悩んで時間を無駄に使った. まあ、これも書くのが何度目だろうっていう感じの幅優先探索なので、それほど難しくはない. −200≤x<sub>i</sub>,y<sub>i</sub>,X,Y≤200 だからといって、それを空間にしてしまうと、端にある障害物の隣を本来すり抜けることが可能なのに通行不能になってしまうので、上下左右に1つづつ広げてやらないといけないことには注意.

```python
from collections import deque

INF = float('inf')

N, X, Y = map(int, input().split())

t = [['.'] * 403 for _ in range(403)]
for _ in range(N):
    x, y = map(int, input().split())
    t[y + 201][x + 201] = '#'

d = [[INF] * 403 for _ in range(403)]
d[0 + 201][0 + 201] = 0
q = deque([(0, 0)])
while q:
    y, x = q.popleft()
    i, j = y + 201, x + 201
    if i + 1 < 403 and j - 1 >= 0 and t[i + 1][j - 1] != '#' and d[i + 1][j - 1] > d[i][j] + 1:
        d[i + 1][j - 1] = d[i][j] + 1
        q.append((y + 1, x - 1))
    if i + 1 < 403 and j + 1 < 403 and t[i + 1][j + 1] != '#' and d[i + 1][j + 1] > d[i][j] + 1:
        d[i + 1][j + 1] = d[i][j] + 1
        q.append((y + 1, x + 1))
    if i - 1 >= 0 and t[i - 1][j] != '#' and d[i - 1][j] > d[i][j] + 1:
        d[i - 1][j] = d[i][j] + 1
        q.append((y - 1, x))
    if i + 1 < 403 and t[i + 1][j] != '#' and d[i + 1][j] > d[i][j] + 1:
        d[i + 1][j] = d[i][j] + 1
        q.append((y + 1, x))
    if j - 1 >= 0 and t[i][j - 1] != '#' and d[i][j - 1] > d[i][j] + 1:
        d[i][j - 1] = d[i][j] + 1
        q.append((y, x - 1))
    if j + 1 < 403 and t[i][j + 1] != '#' and d[i][j + 1] > d[i][j] + 1:
        d[i][j + 1] = d[i][j] + 1
        q.append((y, x + 1))
if d[Y + 201][X + 201] == INF:
    print(-1)
else:
    print(d[Y + 201][X + 201])
```

## [past202005F - 回文行列](https://atcoder.jp/contests/past202005-open/tasks/past202005_f)

40分半?で突破. WA4. 行列の各行から文字を取って回文を作る問題なのに、行列内の全ての文字を使って回文を作る問題だと勘違いして大惨事. set の積集合が分かっていれば解けると思います.

```python
N = int(input())
a = [input() for _ in range(N)]

t = ''
for i in range(N // 2):
    u = list(set(a[i]) & set(a[N - 1 - i]))
    if len(u) == 0:
        print(-1)
        exit()
    t += u[0]

if N % 2 == 0:
    print(t + t[::-1])
else:
    print(t + a[N // 2][0] + t[::-1])
```

## [past202005J - 回転寿司](https://atcoder.jp/contests/past202005-open/tasks/past202005_j)

27分半?で突破. [ABC134E - Sequence Decomposing](https://atcoder.jp/contests/abc134/tasks/abc134_e) を思い出した. 広義単調減少列になるので食べる子供を二分探索できる.

```python
N, M = map(int, input().split())
a = list(map(int, input().split()))

eater = [-1] * M
eater[0] = 1
t = [-1] * N
t[0] = a[0]
for i in range(1, M):
    ng = -1
    ok = N
    while abs(ng - ok) > 1:
        m = (ok + ng) // 2
        if a[i] > t[m]:
            ok = m
        else:
            ng = m
    if ok != N:
        t[ok] = a[i]
        eater[i] = ok + 1
print('\n'.join(str(i) for i in eater))
```

## [past202005I - 行列操作](https://atcoder.jp/contests/past202005-open/tasks/past202005_i)

68分で突破. WA2、TLE2、RE2. N≤10<sup>5</sup> なのを見ずに N×N のリストを作るアホ発生. 列の入れ替えやら、転置やらを実際にやると大惨事なので、読み替えテーブルで *O*(1) で処理するわけだが、行の入れ替えと列の入れ替えが転置のときに入れ替わるのを忘れてて無駄に時間を食った. 今回クエリ問題多くない?(3問目).

```python
from sys import stdin
readline = stdin.readline

N = int(readline())
Q = int(readline())

result = []
transposed = False
rows = list(range(N + 1))
cols = list(range(N + 1))
for _ in range(Q):
    Query = list(map(int, readline().split()))
    if Query[0] == 1:
        A, B = Query[1:]
        if not transposed:
            t = rows[A]
            rows[A] = rows[B]
            rows[B] = t
        else:
            t = cols[A]
            cols[A] = cols[B]
            cols[B] = t
    elif Query[0] == 2:
        A, B = Query[1:]
        if not transposed:
            t = cols[A]
            cols[A] = cols[B]
            cols[B] = t
        else:
            t = rows[A]
            rows[A] = rows[B]
            rows[B] = t
    elif Query[0] == 3:
        transposed = not transposed
    elif Query[0] == 4:
        A, B = Query[1:]
        y, x = A, B
        if transposed:
            y, x = x, y
        y = rows[y]
        x = cols[x]
        result.append(N * (y - 1) + x - 1)
print('\n'.join(str(i) for i in result))
```

## [past202005H - ハードル走](https://atcoder.jp/contests/past202005-open/tasks/past202005_h)

63分半?で突破. WA2、RE1. 今回最大のやらかし. 配る DP 一発の簡単な問題だと思ったら WA. なんでか本当に分からず、なんかしょうもないミスしてるんだろうから、Go 言語で書き直したら回避するかなと思って書いている途中で `T1 / 2` と書いた瞬間に「アレ、なんで `T1 * 0.5` とか書いてるんだ. 浮動小数点数になってるやんけ!!!」と気づいて AC.

```python
N, L = map(int, input().split())
x = set(map(int, input().split()))
T1, T2, T3 = map(int, input().split())

dp = [float('inf')] * (L + 4)
dp[0] = 0
for i in range(L):
    a = dp[i] + T1
    if i + 1 in x:
        a += T3
    if dp[i + 1] > a:
        dp[i + 1] = a

    a = dp[i] + T1 + T2
    if i + 2 in x:
        a += T3
    if dp[i + 2] > a:
        dp[i + 2] = a

    a = dp[i] + T1 + T2 * 3
    if i + 4 in x:
        a += T3
    if dp[i + 4] > a:
        dp[i + 4] = a

result = dp[L]
result = min(result, dp[L - 1] + T1 // 2 + T2 // 2)
result = min(result, dp[L - 2] + T1 // 2 + 3 * T2 // 2)
result = min(result, dp[L - 3] + T1 // 2 + 5 * T2 // 2)
print(result)
```

## [past202005K - コンテナの移動](https://atcoder.jp/contests/past202005-open/tasks/past202005_k)

残り20分だったので諦め. どうせ後2問解かないと上級にはならないしね. 下が何かだけを記録しておけば、*O*(1) で移動できるけど、今のどの机に載っているかが *O*(*N*) だよなあ、どうすればいいんだろうくらいは考えた. しかし、今回はクエリ問題が多いなあ(4問目).

## [past202005L - スーパーマーケット](https://atcoder.jp/contests/past202005-open/tasks/past202005_l)

残り20分だったので諦め. どうせ後2問解かないと上級にはならないしね. 1≤a<sub>i</sub>≤2 だから優先度付きキューを2つ持てば出来るか? いや移すのが *O*(*N*) だから無理か、平衡二分探索木かなくらいは考えた.
