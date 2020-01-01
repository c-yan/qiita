# AtCoder Beginner Contest 136 参戦記

## ABC136A - Transfer

3分で突破. さらサラッと書いて、入力例3で -2 になって、グハってなって書き直して終わり.

```python
a, b, c = [int(e) for e in input().split()]
if a - b > c:
  print(0)
else:
  print(c - (a - b))
```

## ABC136B - Uneven Numbers

6分で突破. 1からNまで全部試すという身も蓋もないナイーブなコードを書いて、実行時間制限に引っかからないだろうかと不安になったけど大丈夫だったのでOK.

```python
n = int(input())
result = 0
for i in range(1, n + 1):
  if len(str(i)) % 2 == 1:
    result += 1
print(result)
```

## ABC136C - Build Stairs

9分半で突破. 端から -1 するというオプションを念頭に試していけば良いんかなとさらサラッと書いたら入力例で WA. これは右からやれば良いんじゃないかと思って変えてみたら、入力例が通ったので、そのまま提出してみたら AC. 右からにすればいくというのはある程度確信があったが、かといってなんでうまくいくか説明しろと言われると困る. 完全に暗黙知である.

```python
import sys
n = int(input())
h = [int(e) for e in input().split()]
for i in range(n - 2, 0, -1):
  if h[i] > h[i + 1]:
    h[i] -= 1
  if h[i] > h[i + 1]:
    print('No')
    sys.exit()
print('Yes')
```

## ABC136D - Gathering Children

敗退. どう考えても 10^100 回動きをシミュレートするのは TLE. ただ RL のところに辿り着いてこの2つを往復運動するだけなのはすぐ分かるので、後は L を右に持つ R にたどり着くまでの移動回数が偶数か奇数かで R か L に振り分けるだけである. とはいえ、ナイーブに RL にたどり着くまでを最大 10^5 シミュレートするのは TLE になるに決まっている. 過去に計算済のマスについて、RL までの移動回数を記録しておけば、計算済のマスまでの移動回数 + 記憶してある移動回数で一気にシミュレーション回数が減るので行けるかなと思ったが PyPy で TLE が 6/21. だったらもう RL を検索して見つけて、Rの左がRである間と、Lの右がLである間の移動回数を事前計算しておけばいくだろうと踏んだが PyPy で TLE 3/21. タイムアップ直前に R の左側に R がn(偶数個)あったら、R と L に 2 / n、n(奇数個)あったら R に 2 // n, L に 2 // n + 1 とかバッチで処理すればいいじゃんと気づいたが時既に遅し.

```python
s = input()
n = len(s)
result = [0 for _ in range(n)]
t = [None for _ in range(n)]
prev = ''
for i in range(n):
  if t[i] is not None:
    continue
  if prev == 'R' and s[i] == 'L':
    j = i
    while t[j] == 'L' and j < n:
      t[j] = (j - (i - 1), i - 1)
      j += 1
    j = i - 1
    while t[j] == 'R' and j > 0:
      t[j] = (j - (i - 1), i - 1)
      j -= 1
  prev = s[i]
for i in range(n):
  p = i
  if s[p] == 'R':
    p += 1
    prev = 'R'
  else:
    p -= 1
    prev = 'L'
  count = 1
  while True:
    if t[p] is not None:
      fp = t[p][0]
      fcount = count + t[p][1]
      t[i] = (fp, fcount)
      if fcount % 2 == 0:
        result[fp] += 1
      else:
        result[fp - 1] += 1
      break
    if s[p] == 'R':
      p += 1
      prev = 'R'
    else:
      if prev == 'R':
        t[i] = (p, count)
        if count % 2 == 0:
          result[p] += 1
        else:
          result[p - 1] += 1
        break
      p -= 1
      prev = 'L'
    count += 1
print(' '.join(map(str, result)))
```

追記: というわけでタイムアップ直前に思いついた着想で書いてみたら15分で書けた挙げ句、PyPy 使わずに一発 AC で、もっと早く思いつけよとしか言いようがなかった.

```python
s = input()
n = len(s)
result = [0 for _ in range(n)]
start = 0
c = 'R'
for i in range(1, n):
  if s[i] != c:
    if c == 'R':
      result[i - 1] += (i - start) // 2 + (i - start) % 2
      result[i] += (i - start) // 2
      c = 'L'
    else:
      result[start] += (i - start) // 2 + (i - start) % 2
      result[start - 1] += (i - start) // 2
      c = 'R'
    start = i
result[start] += (n - start) // 2 + (n - start) % 2
result[start - 1] += (n - start) // 2
print(*result)
```
