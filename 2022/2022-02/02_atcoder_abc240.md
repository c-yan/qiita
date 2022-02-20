# AtCoder Beginner Contest 240 参戦記

## [ABC240A - Edge Checker](https://atcoder.jp/contests/abc240/tasks/abc240_a)

4分で突破. 書くだけ.

```python
a, b = map(int, input().split())

if (a + 1 == b) or (a == 1 and b == 10):
    print('Yes')
else:
    print('No')
```

## [ABC240B - Count Distinct Integers](https://atcoder.jp/contests/abc240/tasks/abc240_b)

1分半で突破. 書くだけ. というか、種類数を数える問題何度目だ. 頻出にもほどがある.

```python
N, *a = map(int, open(0).read().split())

print(len(set(a)))
```

## [ABC240C - Jumping Takahashi](https://atcoder.jp/contests/abc240/tasks/abc240_c)

4分で突破. C問題で DP 出るのかあと思った.

```python
from sys import stdin

readline = stdin.readline

N, X = map(int, readline().split())
t = set([0])
for _ in range(N):
    a, b = map(int, readline().split())
    u = set()
    for x in t:
        u.add(x + a)
        u.add(x + b)
    t = u

if X in t:
    print('Yes')
else:
    print('No')
```

## [ABC240D - Strange Balls](https://atcoder.jp/contests/abc240/tasks/abc240_d)

9分で突破. 数値と幾つ連続で並んでいるかの2つ組のリストを管理すればいいだけですね.

```python
N, *a = map(int, open(0).read().split())

q = []
c = 0
result = []
for x in a:
    if len(q) == 0 or q[-1][0] != x:
        q.append((x, 1))
        c += 1
    else:
        n = q[-1][1]
        if n == x - 1:
            q.pop()
            c -= n
        else:
            q[-1] = (x, n + 1)
            c += 1
    result.append(c)
print(*result, sep='\n')
```
