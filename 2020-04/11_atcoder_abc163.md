# AtCoder Beginner Contest 163 参戦記

ここ最近は上位20%近辺の常連だったが、今回は上位38.4%の位置ということで、unrated で良かった…….

## [ABC163A - Circle Pond](https://atcoder.jp/contests/abc163/tasks/abc163_a)

4分で突破. 書くだけだったけど、問題文が開始から2分以上見えなかったので時間がかかった. 円周の長さの公式くらい覚えてますよね?

```python
from math import pi

R = int(input())

print(2 * pi * R)
```

## [ABC163B - Homework](https://atcoder.jp/contests/abc163/tasks/abc163_b)

1分半で突破. 書くだけ. 宿題の所要日数を単純に合計して、夏休みの日数と比較するだけ.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))

a = sum(A)

if a > N:
    print(-1)
else:
    print(N - a)
```

追記: これでよかったな.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))

print(max(N - sum(A), -1))
```

## [ABC163C - management](https://atcoder.jp/contests/abc163/tasks/abc163_c)

7分半で突破. ディクショナリで直接の部下一覧リストを作ったけど、これ無駄に頑張りすぎてるなと今気づいた. 素直にリストに人数だけ足し込むほうが楽じゃん…….

```python
N = int(input())
A = list(map(int, input().split()))

d = {}

for i in range(N - 1):
    if A[i] in d:
        d[A[i]].append(i + 2)
    else:
        d[A[i]] = [i + 2]

for i in range(1, N + 1):
    if i in d:
        print(len(d[i]))
    else:
        print(0)
```

追記: これでよかった.

```python
N = int(input())
A = list(map(int, input().split()))

result = [0] * N
for a in A:
    result[a - 1] += 1
print('\n'.join(map(str, result)))
```

## [ABC163D - Sum of Large Numbers](https://atcoder.jp/contests/abc163/tasks/abc163_d)

まさかの敗退. 0 が邪魔すぎた.
