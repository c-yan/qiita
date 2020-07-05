# AtCoder Beginner Contest 173 参戦記

直前まで寝ていて眠い、眠い. そのせいか A, B 問題解くのが遅かったし、しょうもない WA も出してしまった. まあ、体調管理も競技のうちなので、自分が悪い. 結果は青パフォだったのでヨシ!

## [ABC173A - Payment](https://atcoder.jp/contests/abc173/tasks/abc173_a)

2分半で突破. 書くだけ.

```python
N = int(input())

if N % 1000 == 0:
    print(0)
else:
    print(1000 - N % 1000)
```

## [ABC173B - Judge Status Summary](https://atcoder.jp/contests/abc173/tasks/abc173_b)

4分で突破. 書くだけ.

```python
N = int(input())
S = [input() for _ in range(N)]

d = {'AC': 0, 'WA': 0, 'TLE': 0, 'RE': 0}
for s in S:
    d[s] += 1
print('AC x', d['AC'])
print('WA x', d['WA'])
print('TLE x', d['TLE'])
print('RE x', d['RE'])
```

## [ABC173C - H and V](https://atcoder.jp/contests/abc173/tasks/abc173_c)

6分半で突破. 制約を見てビット全探索だと一目. 最近ビット全探索多くね? (まあ、今回は二重ループだけど)

```python
from itertools import product

H, W, K = map(int, input().split())
c = [input() for _ in range(H)]

result = 0
for a in product([True, False], repeat=H):
    for b in product([True, False], repeat=W):
        t = 0
        for y in range(H):
            if a[y]:
                continue
            for x in range(W):
                if b[x]:
                    continue
                if c[y][x] == '#':
                    t += 1
        if t == K:
            result += 1
print(result)
```

## [ABC173D - Chat in a Circle](https://atcoder.jp/contests/abc173/tasks/abc173_d)

14分で突破. WA1. N - 1 を N とかくチョンボ. 一つでもサンプルを試していればくらわなかったので猛省.

手でやってみると A を大きい順にソートしたものを a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>N</sub> とした場合、a<sub>1</sub> + a<sub>2</sub> + a<sub>2</sub> + a<sub>3</sub> + a<sub>3</sub> + ... となりそうだなと分かったので、それで提出したら AC した.

```python
N = int(input())
A = list(map(int, input().split()))

A.sort(reverse=True)
result = 0
for i in range(N - 1):
    result += A[(i + 1) // 2]
print(result)
```

## [ABC173E - Multiplication 4](https://atcoder.jp/contests/abc173/tasks/abc173_e)

41分で突破. WA1. プラスルートの判別が間違っていた.

まず個数を見てプラスになるか、0になるか、マイナスになるかを考える. プラスの場合は絶対値が大きくなるように、マイナスの場合は絶対値が小さくなるように積算していけば良い. プラスの場合、マイナスの値は2個づつしか使えないことに注意.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

m = 1000000007

a = [e for e in A if e > 0]
b = [e for e in A if e < 0]
c = [e for e in A if e == 0]
# プラスルート
if len(a) >= K - (min(K, len(b)) // 2) * 2:
    a.sort(reverse=True)
    b.sort()
    result = 1
    i = 0
    j = 0
    k = 0
    while k < K:
        if k < K - 1 and i < len(a) - 1 and j < len(b) - 1:
            x = a[i] * a[i + 1]
            y = b[j] * b[j + 1]
            if y >= x:
                result *= y
                result %= m
                j += 2
                k += 2
            else:
                result *= a[i]
                result %= m
                i += 1
                k += 1
        elif k < K - 1 and j < len(b) - 1:
            y = b[j] * b[j + 1]
            result *= y
            result %= m
            j += 2
            k += 2
        elif i < len(a):
            result *= a[i]
            result %= m
            i += 1
            k += 1
        elif j < len(b):
            result *= b[j]
            result %= m
            j += 1
            k += 1
    print(result)
# 0 ルート
elif len(c) != 0:
    print(0)
# マイナスルート
else:
    a.sort()
    b.sort(reverse=True)
    result = 1
    i = 0
    j = 0
    k = 0
    while k < K:
        if i < len(a) and j < len(b):
            if a[i] <= -b[i]:
                result *= a[i]
                result %= m
                i += 1
                k += 1
            else:
                result *= b[j]
                result %= m
                j += 1
                k += 1
        elif i < len(a):
            result *= a[i]
            result %= m
            i += 1
            k += 1
        elif j < len(b):
            result *= b[j]
            result %= m
            j += 1
            k += 1
    print(result)
```

## [ABC173F - Intervals on Tree](https://atcoder.jp/contests/abc173/tasks/abc173_f)

解けず. *O*(*N*<sup>2</sup>) の解法しか思いつかなかった.
