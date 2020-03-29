# AtCoder Beginners Contest 過去問チャレンジ7

## [ABC046D - AtCoDeerくんと変なじゃんけん](https://atcoder.jp/contests/abc046/tasks/arc062_b)

TopCoDeerくん と全く同じ手を出せば得点は0点になるので、最大得点は0点以上となる. とりあえず TopCoDeerくん と全く同じ手を出すことにして、パーを出した回数と、グーを出した回数を数えて、条件を満たす数だけ、一番最後に出したグーからパーに変えていけば最高得点になる.

```python
s = input()

print(max((s.count('g') * 2 - len(s)) // 2, 0))
```

## [ABC097D - Equals](https://atcoder.jp/contests/abc097/tasks/arc097_b)

1と2がスワップできて、2と3がスワップできれば、1と3もスワップできる. つまり同じユニオンならスワップ可能. Union Find 木だ. 同じユニオン内なら思う通りに並べることができるかについては証明できないけど、まあバブルソートができない時があるなんて聞いたこと無いからおそらく大丈夫だろう. 直感的には端から順に並べていけばいいはず. なので、すべての p<sub>i</sub> について、同じユニオン内に i があるかを確認して、あった数の合計が答えになる.

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


N, M = map(int, input().split())
p = list(map(int, input().split()))

parent = [-1] * N
for _ in range(M):
    x, y = map(int, input().split())
    unite(parent, x - 1, y - 1)

result = 0
for i in range(N):
    if find(parent, i) == find(parent, p[i] - 1):
        result += 1
print(result)
```

## [ABC043D - アンバランス](https://atcoder.jp/contests/abc043/tasks/arc059_b)

すべての部分文字列について過半数が同じ文字かを確認すると O(N<sup>2</sup>) になるので TLE 必至である. ところで、過半数が同じ文字であるならば、同じ文字が2連続か、間を1個空けて同じ文字が出現する. これは実際に過半数が同じ文字を実際に作ってみればすぐ分かる. これが存在するかは O(N) で確認できるので、解くことができる.

```python
s = input()

for i in range(len(s) - 1):
    if s[i] == s[i + 1]:
        print(*[i + 1, i + 2])
        exit()
for i in range(len(s) - 2):
    if s[i] == s[i + 2]:
        print(*[i + 1, i + 3])
        exit()
print(*[-1, -1])
```

## [ABC105D - Candy Distribution](https://atcoder.jp/contests/abc105/tasks/abc105_d)

ナイーブに書くと O(N<sup>3</sup>)、累積和で緩和しても O(N<sup>2</sup>) で TLE. ではどうするか? まず「合計が M の倍数」ではなく、「M で割った余りの合計を M で割った余りが0である」を数え上げることとする. 次に A<sub>1</sub>, A<sub>1</sub> + A<sub>2</sub>, ..., A<sub>1</sub> + A<sub>2</sub>...A<sub>N</sub> のMで割ったあまりの数を数え上げる. t<sub>m</sub> を余り m の出現数とした場合 l = 1, r = 1..N のときのかぞえあげるべき個数は t<sub>0</sub> となる. 次に l = 2, r = 1..N の数え上げるべき個数だが、それぞれから A<sub>1</sub> を M で割った数が減るわけだから、t<sub>A<sub>1</sub>%M</sub> となると一瞬思うが、l=1, r=1 は A<sub>1</sub> だけだったのでこれは省かないといけなく t<sub>A<sub>1</sub>%M</sub> - 1 となるし、l = 3 以降の計算のときからもこの1つは省かれていないといけない. 後は l = N までループすれば答えが求まる.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))

t = {}
c = 0
for a in A:
    c += a
    c %= M
    if c in t:
        t[c] += 1
    else:
        t[c] = 1

result = 0
if 0 in t:
    result += t[0]
c = 0
for a in A:
    c += a
    c %= M
    t[c] -= 1
    result += t[c]
print(result)
```

## [ABC064D - Insertion](https://atcoder.jp/contests/abc064/tasks/abc064_d)

求めるのは辞書順最小のものなので、'(' は可能な限り左に挿入したいし、')' は可能な限り右に挿入したいとなる. なので、'(' は左端に、')' は右端にしか挿入しないとする. 後はいくつづつ挿入するかなのだが、文字列の長さはたかだか100までなので、実際に変形しても TLE にはならない. 対になっているカッコを消しながら、左端か右端に文字列が空になるまでカッコを追加していくだけ.

```python
N = int(input())
S = input()

result = S
while True:
    while True:
        t = S.replace('()', '')
        if t == S:
            break
        S = t
    if S == '':
        break
    elif S[0] == ')':
        S = '(' + S
        result = '(' + result
    elif S[0] == '(':
        S = S + ')'
        result = result + ')'
print(result)
```

## [ABC088D - Grid Repainting](https://atcoder.jp/contests/abc088/tasks/abc088_d)

幅優先探索で (1, 1) から (H, W) への最短路を見出して、最短路で通らなかった全ての白いマスを全て黒に変えるのが最大スコアである. 答えは白いマス目数 - 最短路のマス目数となる.

```python
H, W = map(int, input().split())
s = [input() for _ in range(H)]

dp = [[2501] * W for _ in range(H)]
dp[0][0] = 1

q = [(0, 0)]
while q:
    nq = []
    while q:
        y, x = q.pop()
        t = dp[y][x] + 1
        if y < H - 1:
            if s[y + 1][x] == '.' and dp[y + 1][x] > t:
                dp[y + 1][x] = t
                nq.append((y + 1, x))
        if y > 0:
            if s[y - 1][x] == '.' and dp[y - 1][x] > t:
                dp[y - 1][x] = t
                nq.append((y - 1, x))
        if x < W - 1:
            if s[y][x + 1] == '.' and dp[y][x + 1] > t:
                dp[y][x + 1] = t
                nq.append((y, x + 1))
        if x > 0:
            if s[y][x - 1] == '.' and dp[y][x - 1] > t:
                dp[y][x - 1] = t
                nq.append((y, x - 1))
    q = nq

if dp[H - 1][W - 1] == 2501:
    print(-1)
else:
    print(sum(s[y].count('.') for y in range(H)) - dp[H - 1][W - 1])
```
