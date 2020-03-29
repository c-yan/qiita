# AtCoder Beginners Contest 過去問チャレンジ5

## [ABC134D - Preparing Boxes](https://atcoder.jp/contests/abc134/tasks/abc134_d)

i = N から i を減らしながら順に確定していけば、特に難しいことはない.

```python
N = int(input())
a = list(map(int, input().split()))

t = [0] * N
for i in range(N - 1, -1, -1):
    t[i] = (sum(t[2 * (i + 1) - 1::i + 1]) % 2) ^ a[i]

print(sum(t))
print(*[i + 1 for i in range(N) if t[i] == 1])
```

## [ABC080D - Recording](https://atcoder.jp/contests/abc080/tasks/abc080_d)

ぱっと見では imos 法一発に見えるのだが、S - 0.5 があるので、何も考えずに imos 法をすると、同じチャンネルの連続する録画が 0.5 の間だけ2台になってしまう罠.

解決法として一番簡単なのは、imos 法をやめてしまうこと. `+= 1` ではなく `= 1` であればダブっても大丈夫だし、処理時間的にも実はぜんぜん大丈夫なのだった.

```python
N, C = map(int, input().split())

tt = [[0] * (10 ** 5 + 1) for _ in range(C)]
for _ in range(N):
    s, t, c = map(int, input().split())
    for i in range(s - 1, t):
        tt[c - 1][i] = 1

ct = [0] * (10 ** 5 + 1)
for i in range(C):
    for j in range(10 ** 5 + 1):
        ct[j] += tt[i][j]

print(max(ct))
```

imos 法でやるのも難しくはないのだけど、エレガントに書こうと思うと意外と難しい. ソートして、連続してたら動きを変えるのが一番簡単か?

```python
from operator import itemgetter

N, C = map(int, input().split())
stc = [list(map(int, input().split())) for _ in range(N)]
stc.sort(key=itemgetter(2, 0))

cs = [0] * (10 ** 5 + 1)
pt, pc = -1, -1
for s, t, c in stc:
    if pt == s and pc == c:
        cs[s] += 1
    else:
        cs[s - 1] += 1
    cs[t] -= 1
    pt, pc = t, c

for i in range(1, 10 ** 5 + 1):
    cs[i] += cs[i - 1]

print(max(cs))
```

ちなみに理論上はコケうるけど、そうそうヒットしないだろうとチャンネルをスルーしていたらしっかりとスナイプされました. 出題者、さすがやでえ…….

## [ABC121D - XOR World](https://atcoder.jp/contests/abc121/tasks/abc121_d)

`f(A,B)` は `f(0,A-1) xor f(0,B)` と同じなので、`g(N)=f(0,N)` である、`g` が書ければ良い. 2進数の各桁ごとに1の数をカウントして、偶数か奇数かで処理ができる.

```python
def h(A, n):
    if A == -1:
        return 0
    return A // (2 * n) * n + max(A % (2 * n) - (n - 1), 0)


def g(A):
    result = 0
    for i in range(48):
        t = 1 << i
        if h(A, t) % 2 == 1:
            result += t
    return result


def f(A, B):
    return g(A - 1) ^ g(B)


A, B = map(int, input().split())

print(f(A, B))
```

実のところ任意の偶数の `n` について `n xor (n + 1)` は `1` なので、各桁ごとにカウントする必要はない.

```python
def g(A):
    t = ((A + 1) // 2) % 2  # 任意の偶数 n について n ^ (n + 1) == 1
    if A % 2 == 0:
        return A ^ t
    return t


def f(A, B):
    return g(A - 1) ^ g(B)


A, B = map(int, input().split())

print(f(A, B))
```

## [ABC117D - XXOR](https://atcoder.jp/contests/abc117/tasks/abc117_d)

Aの各ビット毎にビットの立っている数と立っていない数を集計し、立っている数のほうが多ければそのビットは0、立っていない数のほうが多ければそのビットは1のXを作ればいいだけ. ただ X には K 以下という条件がついているので、K を超えてしまったらビットを立てるのは諦める.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

bcs = [0] * 41
for i in range(N):
    a = A[i]
    for j in range(41):
        if a & (1 << j) != 0:
            bcs[j] += 1

X = 0
for i in range(40, -1, -1):
    if bcs[i] >= N - bcs[i]:
        continue
    t = 1 << i
    if X + t <= K:
        X += t

result = 0
for i in range(N):
    result += X ^ A[i]
print(result)
```

## [ABC094D - Binomial Coefficients](https://atcoder.jp/contests/abc094/tasks/arc095_b)

a<sub>i</sub> は最大の数. a<sub>j</sub> はその次に大きい数ではあるけど <sub>a<sub>i</sub></sub>C<sub>a<sub>j</sub></sub> == <sub>a<sub>i</sub></sub>C<sub>a<sub>i</sub>-a<sub>j</sub></sub> なことに注意して次数を探す.

```python
n = int(input())
a = list(map(int, input().split()))

a.sort()
x = a[-1]
b = [(min(e, x - e), e) for e in a[:-1]]
b.sort()
print(x, b[-1][1])
```

## [ABC098D - Xor Sum 2](https://atcoder.jp/contests/abc098/tasks/arc098_b)

加算は単調増加だけど、XOR は 1 xor 1 が 0 なので単調増加ではない. ところで 0 xor N == 0 + N なので「XOR <= 加算」となる. つまり一度でも XOR の結果が加算の結果未満になると、そこからどれだけ r の数字を増やしても永久に同じになることはない. また、ある l < r が同じ値の場合、l + 1 と r の組み合わせでも同じ値になる. ここまで来るとしゃくとり法でいいことが分かる.

```python
N = int(input())
A = list(map(int, input().split()))

result = 0
r = 0
xs = A[0]
cs = A[0]
for l in range(N):
    while r < N - 1 and xs ^ A[r + 1] == cs + A[r + 1]:
        r += 1
        xs ^= A[r]
        cs += A[r]
    result += r - l + 1
    xs ^= A[l]
    cs -= A[l]
print(result)
```
