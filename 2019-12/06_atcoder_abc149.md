# AtCoder Beginner Contest 149 参戦記

## ABC149A - Strings

1分で突破. 書くだけ

```python
S, T = input().split()

print(T + S)
```

## ABC149B - Greedy Takahashi

6分で突破. 書くだけ. 最初文章の意味がわかってなくて、どっちもK枚減らすのかと思ったけど、入出力例を試して正しい意味に気づいた.

```python
A, B, K = map(int, input().split())

a = max(A - K, 0)
K -= A - a
b = max(B - K, 0)
print(*[a, b])
```

## ABC149C - Next Prime

7分で突破. ABC084D - 2017-like Number で書いたエラトステネスの篩を持ってきて書くだけ.

```python
from math import sqrt

N = 10 ** 6

p = [True] * (N + 1)
p[0] = False
p[1] = False
n = int(sqrt(N) + 1)
for j in range(4, N + 1, 2):
  p[j] = False
for i in range(3, n + 1, 2):
  if p[i]:
    for j in range(i + i, N + 1, i):
      p[j] = False


X = int(input())
for i in range(X, N + 1):
    if p[i]:
        print(i)
        break
```

## ABC149D - Prediction and Restriction

44分で突破. WA1. 取り敢えず勝つ手を出す前提で話を進め、K個前の手と同じ手になってしまった場合はあいこにするという実装にしたら WA が出て、「DP しないといけないのかあ!? でも突破人数的にそれはないか(腐った推論)」とだいぶ長時間悩んで、負けの手のほうがいい可能性があることに気づいて AC. そのせいで順位が1400番台になってしまったので、unrated になって助かった感.

```python
N, K = map(int, input().split())
R, S, P = map(int, input().split())
T = input()

d = {'r': P, 's': R, 'p': S}
result = 0
wins = [False] * K
for i in range(len(T)):
    if not wins[i % K] or T[i] != T[i - K]:
        result += d[T[i]]
        wins[i % K] = True
    else:
        wins[i % K] = False
print(result)
```

## ABC149E - Handshake

突破できず. オーダーを切り下げる方法が思いつかず TLE いっぱいのまま終了.

追記: すべての組み合わせを生成するのは計算量が 10<sup>10</sup> になってしまうから無理なので、生成する組み合わせをなんとか M までに抑えようと思っても、M も 10<sup>10</sup> で変わらなく詰み. なので発想を逆転して、組み合わせの数が M を超える一回で発生する幸福度の閾値を二分探索で調べる. これは累積和を使うと  O(*n* log *n*) で計算できる.

これが求まってしまえば、左手をベースにループを回し、右手はどこまで下げれるかはさっき作った累積和で分かり、もう一個右手のパワーの累積和を作っておけば O(*n*) で最終的な幸福度の合計値が求まる. ただし、組み合わせの数がちょうど M とは限らないので、M を超えている分だけさっき求めた一回で発生する幸福度の閾値を差し引く必要がある.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))

A.sort(reverse=True)
LB = A[-1] * 2  # 一回の握手で上がる幸福度の下限は、左手も右手もパワーが一番低い人を握った場合
UB = A[0] * 2   # 一回の握手で上がる幸福度の上限は、左手も右手もパワーが一番高い人を握った場合

# cgs[i] は パワーがi以上のゲストの数
cgs = [0] * (UB + 1)
for a in A:
    cgs[a] += 1
for i in range(UB, 0, -1):
    cgs[i - 1] += cgs[i]


# 組み合わせの数がM以上になる一回で発生する幸福度の閾値を二分探索する
def is_ok(n):
    m = 0
    for a in A:
        m += cgs[max(n - a, 0)]
    return m >= M


ok = LB
ng = UB + 1
while ng - ok > 1:
    m = (ok + ng) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m

# cps[i] は A[0]～A[i] の累積和
cps = A[:]
for i in range(1, N):
    cps[i] += cps[i - 1]

result = 0
for a in A:
    t = cgs[max(ok - a, 0)]
    if t != 0:
        result += a * t + cps[t - 1]
        M -= t
result += ok * M
print(result)
```
