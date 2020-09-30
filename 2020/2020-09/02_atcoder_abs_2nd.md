# AtCoder Beginners Selection 体験記 (2回目)

[AtCoder Beginners Selection 体験記](https://qiita.com/c-yan/items/cf1c6ee6f06c0df9773b) のコードが Python 2 でもう動かないので、どれくらいレベルアップしたかを確認する意味でもう一度解いてみた.

## [PracticeA - Welcome to AtCoder](https://atcoder.jp/contests/abs/tasks/practice_1)

8分→2分. スペース区切りの文字列を % で生成していたのが、print にカンマ区切りで渡せばスペース区切りで出るよと分かってそこが変わった.

```python
a = int(raw_input())
b, c = [int(e) for e in raw_input().split()]
s = raw_input()
print "%d %s" % (a + b + c, s)
```

↓

```python
a = int(input())
b, c = map(int, input().split())
s = input()

print(a + b + c, s)
```

## [ABC086A - Product](https://atcoder.jp/contests/abs/tasks/abc086_a)

2分→1分. Odd と Even の順序が逆転したw.

```python
a, b = [int(e) for e in raw_input().split()]
if a * b % 2 == 0:
  print 'Even'
else:
  print 'Odd'
```

↓

```python
a, b = map(int, input().split())

if a * b % 2 == 1:
    print('Odd')
else:
    print('Even')
```

## [ABC081A - Placing Marbles](https://atcoder.jp/contests/abs/tasks/abc081_a)

4分→30秒. 当時は count を知らなかったようだ.

```python
print len([c for c in raw_input() if c == '1'])
```

↓

```python
s = input()

print(s.count('1'))
```

## [ABC081B - Shift only](https://atcoder.jp/contests/abs/tasks/abc081_b)

9分→3分半. open(0).read() による一発読み込み登場. 1個づつ処理するのと、全部1回割れるのかのチェックを繰り返すのと、方針が大きく異なった.

```python
n = int(raw_input())
a = [int(e) for e in raw_input().split()]
i = 0
while True:
  if any(e % 2 == 1 for e in a):
    break
  i += 1
  a = [e / 2 for e in a]
print i
```

↓

```python
N, *A = map(int, open(0).read().split())

result = float('inf')
for a in A:
    t = 0
    while a % 2 == 0:
        t += 1
        a //= 2
    result = min(result, t)
print(result)
```

## [ABC087B - Coins](https://atcoder.jp/contests/abs/tasks/abc087_b)

4分→2分. 総当り、まあ大差ないですね.

```python
a = int(raw_input())
b = int(raw_input())
c = int(raw_input())
x = int(raw_input())
result = 0
for i in range(a + 1):
  for j in range(b + 1):
    for k in range(c + 1):
      if i * 500 + j * 100 + k * 50 == x:
        result += 1
print result
```

↓

```python
A, B, C, X = map(int, open(0).read().split())

result = 0
for a in range(A + 1):
    for b in range(B + 1):
        for c in range(C + 1):
            if X == 500 * a + 100 * b + 50 * c:
                result += 1
print(result)
```

## [ABC083B - Some Sums](https://atcoder.jp/contests/abs/tasks/abc083_b)

6分→3分. 各桁の和を直接求めるか、文字列に変換して求めるか、方針が異なった.

```python
n, a, b = [int(e) for e in raw_input().split()]
result = 0
for i in range(1, n + 1):
  if a <= sum(int(e) for e in str(i)) <= b:
    result += i
print result
```

↓

```python
N, A, B = map(int, input().split())

result = 0
for i in range(1, N + 1):
    c = 0
    t = i
    while t != 0:
        c += t % 10
        t //= 10
    if A <= c <= B:
        result += i
print(result)
```

## [ABC088B - Card Game for Two](https://atcoder.jp/contests/abs/tasks/abc088_b)

7分→1分半. 方針はほぼ同じだが、sort の reverse オプションを当時は知らなかったようだ.

```python
n = int(raw_input())
a = [int(e) for e in raw_input().split()]
a.sort()
a.reverse()
print sum(a[::2]) - sum(a[1::2])
```

↓

```python
N, *a = map(int, open(0).read().split())

a.sort(reverse=True)
print(sum(a[::2]) - sum(a[1::2]))
```

## [ABC085B - Kagami Mochi](https://atcoder.jp/contests/abs/tasks/abc085_b)

3分→1分半. これは覚えてた.

```python
n = int(raw_input())
d = [int(raw_input()) for i in range(n)]
print len(set(d))
```

↓

```python
N, *d = map(int, open(0).read().split())

print(len(set(d)))
```

## [ABC085C - Otoshidama](https://atcoder.jp/contests/abs/tasks/abc085_c)

7分→3分. まあ、総当たりなので方針としては大差ない. print の使い方だけ変わった.

```python
import sys
n, y = [int(e) for e in raw_input().split()]
for i in range(n + 1):
  for j in range(n + 1 - i):
    k = n - i - j
    if 10000 * i + 5000 * j + 1000 * k == y:
      print "%d %d %d" % (i, j, k)
      sys.exit()
print "-1 -1 -1"
```

↓

```python
N, Y = map(int, input().split())

for i in range(N + 1):
    for j in range(N + 1 - i):
        k = N - i - j
        if 10000 * i + 5000 * j + 1000 * k == Y:
            print(i, j, k)
            exit()
print(-1, -1, -1)
```

## [ABC049C - 白昼夢 / Daydream](https://atcoder.jp/contests/abs/tasks/arc065_a)

1時間くらい→12分. 反転すればいいというのは覚えていた. しかし、前回は *O*(*N*<sup>2</sup>) のアルゴリズムで順方向でねじ伏せていたのかw. 1223ms → 41ms と爆速化した.

```python
import sys
s = raw_input()
ts = ['']
while True:
  nts= []
  for t in ts:
    for w in ['dreamer', 'eraser', 'dream', 'erase']:
      if s == t + w:
        print 'YES'
        sys.exit()
      if s.startswith(t + w):
        nts.append(t + w)
  if len(nts) == 0:
    print 'NO'
    sys.exit()
  ts = nts
```

↓

```python
S = input()

S = S[::-1]
candidates = [s[::-1] for s in ['dream', 'dreamer', 'erase', 'eraser']]

i = 0
while i < len(S):
    for c in candidates:
        if S.startswith(c, i):
            i += len(c)
            break
    else:
        print('NO')
        exit()
print('YES')
```

## [ABC086C - Traveling](https://atcoder.jp/contests/abs/tasks/arc089_a)

40分くらい→8分半. バッファド IO に切り替えたので 337ms → 131ms. やってることはだいたい同じだけど、最初に入力をまとめて処理をするかしないかが違うせいでコードはだいぶ変わった.

```python
import sys
n = int(raw_input())
data = [map(int, raw_input().split()) for i in range(n)]
t = 0
x = 0
y = 0
for d in data:
  duration = d[0] - t
  distance = abs(x - d[1]) + abs(y - d[2])
  if (distance > duration) or ((duration - distance) % 2 == 1):
    print 'No'
    sys.exit()
  t = d[0]
  x = d[1]
  y = d[2]
print 'Yes'
```

↓

```python
from sys import stdin
readline = stdin.readline

N = int(readline())

pt, px, py = 0, 0, 0
for _ in range(N):
    t, x, y = map(int, readline().split())
    d = abs(x - px) + abs(y - py)
    if d > t - pt:
        print('No')
        break
    if (t - pt - d) % 2 == 1:
        print('No')
        break
    pt, px, py = t, x, y
else:
    print('Yes')
```
