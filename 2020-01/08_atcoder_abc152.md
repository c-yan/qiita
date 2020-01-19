# AtCoder Beginner Contest 152 参戦記

## ABC152A - AC or WA

1分半で突破. 書くだけ.

```python
N, M = map(int, input().split())

if N == M:
    print('Yes')
else:
    print('No')
```

## ABC152B - Comparing Strings

2分で突破. 書くだけ.

```python
a, b = map(int, input().split())

if a < b:
    print(str(a) * b)
else:
    print(str(b) * a)
```

## ABC152C - Low Elements

5分で突破. 流石に二重ループは TLE なので、現時点の最小値を持ち回す必要あり. 題意を理解するのに少し時間を使った.

```python
N = int(input())
P = list(map(int, input().split()))

result = 0
m = P[0]
for i in range(N):
    if P[i] <= m:
        result += 1
        m = P[i]
print(result)
```

## ABC152D - Handstand 2

26分で突破. N回ループ回しても大丈夫だと見切れればさして難しくはない. 先頭と末尾の組み合わせで個数を集計すれば一発.

```python
N = int(input())

t = [[0] * 10 for _ in range(10)]
for i in range(1, N + 1):
    s = str(i)
    t[int(s[0])][int(s[-1])] += 1

result = 0
for i in range(1, 10):
    for j in range(1, 10):
        result += t[i][j] * t[j][i]
print(result)
```

## ABC152E - Flatten

敗退. 最小公倍数を求めて集計するナイーブな実装では TLE だった.
