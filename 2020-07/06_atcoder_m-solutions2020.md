# AtCoder M-SOLUTIONS プロコンオープン 2020 参戦記

## [m-solutions2020A - Kyu in AtCoder](https://atcoder.jp/contests/m-solutions2020/tasks/m_solutions2020_a)

4分で突破. 書くだけ……だったが、4分もかかってしまっていると、さすがに素直に200で割って処理すべきだったと反省.

```python
X = int(input())

if 400 <= X <= 599:
    print(8)
elif 600 <= X <= 799:
    print(7)
elif 800 <= X <= 999:
    print(6)
elif 1000 <= X <= 1199:
    print(5)
elif 1200 <= X <= 1399:
    print(4)
elif 1400 <= X <= 1599:
    print(3)
elif 1600 <= X <= 1799:
    print(2)
elif X >= 1800:
    print(1)
```

## [m-solutions2020B - Magic 2](https://atcoder.jp/contests/m-solutions2020/tasks/m_solutions2020_b)

3分で突破. 2倍なら素直にループを回しても大丈夫だろうと、素直に書き下ろした.

```python
A, B, C = map(int, input().split())
K = int(input())

while A >= B:
    K -= 1
    B *= 2

while B >= C:
    K -= 1
    C *= 2

if K >= 0:
    print('Yes')
else:
    print('No')
```

## [m-solutions2020C - Marks](https://atcoder.jp/contests/m-solutions2020/tasks/m_solutions2020_c)

13分半で突破. TLE1. 固定窓だと思った私がアホでした. なまじ TLE になるものの計算は出来てしまうのが裏目になってしまう Python だった.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

result = []
for i in range(K, N):
    if A[i] > A[i - K]:
        result.append('Yes')
    else:
        result.append('No')
print(*result, sep='\n')
```

## [m-solutions2020D - Road to Millionaire](https://atcoder.jp/contests/m-solutions2020/tasks/m_solutions2020_d)

18分半で突破. DP だろうけど、どう DP 回せばいいんだろうなとそこそこ悩んだ.

```python
N = int(input())
A = list(map(int, input().split()))

t = [-1] * (N + 1)
t[0] = 1000
for i in range(N):
    k = t[i] // A[i]
    y = t[i] % A[i]
    for j in range(i, N):
        t[j + 1] = max(t[j + 1], k * A[j] + y)
print(t[N])
```

## [m-solutions2020E - M's Solution](https://atcoder.jp/contests/m-solutions2020/tasks/m_solutions2020_e)

突破できず. 順位表を見て F の方が簡単なことに途中で気づいたが手遅れ. 正しい答えが出るナイーブなコードは書けたが、そこから計算量を減らせず.
