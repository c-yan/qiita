# AtCoder Beginner Contest 165 参戦記

突然10分遅れになって驚いた. なんかラッキーでC問題、D問題が解けた感がある. まあ、解けた問題を逃してしまったと地団駄することも多いのでたまには良いでしょう.

## [ABC165A - We Love Golf](https://atcoder.jp/contests/abc165/tasks/abc165_a)

2分半で突破. O(1) で書けるんだろうなと思いつつも、ループを回すチキン.

```python
K = int(input())
A, B = map(int, input().split())

for i in range(A, B + 1):
    if i % K == 0:
        print('OK')
        exit()
print('NG')
```

追記: O(1) でも書いてみた.

```python
K = int(input())
A, B = map(int, input().split())

if (B // K) * K >= A:
    print('OK')
else:
    print('NG')
```

## [ABC165B - 1%](https://atcoder.jp/contests/abc165/tasks/abc165_b)

4分で突破. X≤10<sup>18</sup> を見て、B問題なのに TLE ありうる!? と思ったけど、コードテストで 10<sup>18</sup> を食わせても全然平気だったので、なーんだと思いつつ AC.

```python
X = int(input())

t = 100
result = 0
while t >= X:
    t += t // 100
    result += 1
print(result)
```

## [ABC165C - Many Requirements](https://atcoder.jp/contests/abc165/tasks/abc165_c)

23分半で突破. 問題見た瞬間に頭が真っ白になったけど、なんとか解けた. AC した瞬間にこれ現在順位すごいんじゃねと思ったけど、みんな先にD問題を解いててそこまででもなかった(笑).

まあ、総当たりをすれば良いのかなあと思ったけど、妥当でないものも含めて生成すると O(10<sup>10</sup>) でダメそう. 1≤M≤10 に注目して、M - 1 は0～9なので文字列で表現できるじゃんと、文字列で妥当なものだけを全組生成することに. 後は総当たりで. TLE するかなと思ったけど、せずに AC.

```python
N, M, Q = map(int, input().split())
abcd = [list(map(int, input().split())) for _ in range(Q)]

m = M - 1


def f(s):
    if len(s) == N:
        return [s]
    result = []
    if s == '':
        r = 0
    else:
        r = int(s[-1])
    for i in range(r, m + 1):
        result.extend(f(s + str(i)))
    return result


result = 0
V = f('')
for s in V:
    A = [int(c) + 1 for c in s]
    t = 0
    for a, b, c, d in abcd:
        if A[b - 1] - A[a - 1] == c:
            t += d
    result = max(result, t)
print(result)
```

追記: 頑張って自力で全組生成する必要なかった模様. Python は偉大.

```python
from itertools import combinations_with_replacement

N, M, Q = map(int, input().split())
abcd = [list(map(int, input().split())) for _ in range(Q)]

result = 0
for A in combinations_with_replacement(range(1, M + 1), N):
    t = 0
    for a, b, c, d in abcd:
        if A[b - 1] - A[a - 1] == c:
            t += d
    result = max(result, t)
print(result)
```

## [ABC165D - Floor Function](https://atcoder.jp/contests/abc165/tasks/abc165_d)

20分で突破. Ax/B が増えるタイミングに最大値があるはずと思ったので、Ax/B = n の式を解くと x = Bn/A となるので、n を1づつインクリメントした x を N になるまで回してみた. ただこれは A < B のときにアレなので、その場合用に A まで回してみた. というちゃらんぽらんな合体で出してみたら AC が来て自分でもびっくりした. つまり、問題をあまり理解してないのに解けてしまった…….

D問題が解けた瞬間は900番台だったので青パフォの夢を見たが、E問題が解けなくて1500位手前まで後退してしまったのは残念.

```python
from math import floor

A, B, N = map(int, input().split())


def f(x):
    return floor(A * x / B) - A * floor(x / B)


result = 0
for x in range(min(A + 1, N + 1)):
    result = max(result, f(x))

if B > A:
    for i in range(10 ** 20):
        x = (i * B + A - 1) // A
        if x > N:
            break
        result = max(result, f(x))

print(result)
```

追記: 我ながらよく通ったなあ(汗). しかし、D問題とC問題の難易度が完全に入れ替わっている.

```python
from math import floor

A, B, N = map(int, input().split())

x = min(B - 1, N)
print(floor(A * x / B) - A * floor(x / B))
```

## [ABC165E - Rotation Matching](https://atcoder.jp/contests/abc165/tasks/abc165_e)

敗退.

追記: 条件としては a, b の差が違うことなのだが、循環しているので差が違っていても同じに鳴ることがあるので注意(例えば N = 4 の時の、(1, 4) と (2, 3) は3差と1差で違うように見えるが、4の次は1なので、どちらも1差). 結論的に言うと距離は 1, 2, ..., M にすれば良い. この場合は M×2+1≤N の条件から循環していてもユニークとなる. 次に組み合わせをどう取っていくかだが、まず最長の1とM+1を取る. 当然その内側には距離M-1のものは入らないのでその隣にM+2と2M+1で取る. その次は距離M-2なので1とM+1の間に入るので、2とMで取る. その次は距離M-3なので、M+3と2Mで取れば良くてといった感じで、2箇所に分けてマトリョーシカ人形のように内側に詰めていけば良い.

```python
N, M = map(int, input().split())

for i in range(1, M + 1):
    if i % 2 == 1:
        j = (i - 1) // 2
        print(1 + j, M + 1 - j)
    else:
        j = (i - 2) // 2
        print(M + 2 + j, 2 * M + 1 - j)
```
