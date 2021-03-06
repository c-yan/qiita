# AtCoder Beginner Contest 154 参戦記

## [ABC154A - Remaining Balls](https://atcoder.jp/contests/abc154/tasks/abc154_a)

2分半で突破. 変数が5つもあることに一瞬戸惑ったけど、書くだけだった.

```python
S, T = input().split()
A, B = map(int, input().split())
U = input()

if U == S:
    A -= 1
else:
    B -= 1
print(A, B)
```

## [ABC154B - I miss you...](https://atcoder.jp/contests/abc154/tasks/abc154_b)

1分で突破. 書くだけ.

```python
S = input()

print('x' * len(S))
```

## [ABC154C - Distinct or Not](https://atcoder.jp/contests/abc154/tasks/abc154_c)

2分で突破. 書くだけ. [ABC063B - Varied](https://atcoder.jp/contests/abc063/tasks/abc063_b) を思い出した.

```python
N = int(input())
A = list(map(int, input().split()))

if len(set(A)) == N:
    print('YES')
else:
    print('NO')
```

## [ABC154D - Dice in Line](https://atcoder.jp/contests/abc154/tasks/abc154_d)

10分で突破. リスト p から、平均値のリスト m を作って、幅 K の Sliding Window で最大値を求めるだけ.

```python
N, K = map(int, input().split())
p = list(map(int, input().split()))

m = [(e + 1) / 2 for e in p]

t = sum(m[0:K])
result = t
for i in range(N - K):
    t -= m[i]
    t += m[i + K]
    if t > result:
        result = t
print(result)
```

## [ABC154E - Almost Everywhere Zero](https://atcoder.jp/contests/abc154/tasks/abc154_e)

敗退. 前に解けなくて放置したアレと同じ問題だなあと思ったが、そのときに解いておかなかったのが運の尽きだった…….

追記: 桁DP すれば良いわけですが、リストの次数が上がるととたんに遅くなる Python なので、次元を上げずに変数を分けて書いてみました.

```python
N = input()
K = int(input())

a = 1
b = [0] * (K + 1)
b[0] = 1
b[1] = int(N[0]) - 1

for c in N[1:]:
    t = int(c)
    for i in range(K - 1, -1, -1):
        b[i + 1] += b[i] * 9
    if t != 0:
        if a + 1 <= K:
            b[a + 1] += t - 1
        if a <= K:
            b[a] += 1
        a += 1

if a == K:
    print(b[K] + 1)
else:
    print(b[K])
```
