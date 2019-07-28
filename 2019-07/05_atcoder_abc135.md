# AtCoder Beginner Contest 135 参戦記

## A - Harmony

4分半で突破. 最初全然分からなくて、なんでこれがA問題なんだと恐慌に陥ったけど、平均値で良いのかと気づいて瞬殺.

```python
a, b = [int(e) for e in input().split()]
if (a + b) % 2 != 0:
  print('IMPOSSIBLE')
else:
  print((a + b) // 2)
```

## B - 0 or 1 Swap

5分で突破. 一瞬難しいかと思ったけど、数列が固定だったので、位置がおかしいのが2以下ならOKでいいのかと気づいた.

```python
n = int(input())
p = [int(e) for e in input().split()]
z = 0
for i in range(n):
  if p[i] != i + 1:
    z += 1
if z <= 2:
  print('YES')
else:
  print('NO')
```

## C - City Savers

14分で突破. 0番目の街は0番目の勇者しか関われないから、全力だけ倒せるだけ倒して、余力があったら倒せるだけ1番目の街のモンスターを倒して……って感じで、一人づつ倒せるだけ倒していけば良いのかなあ、分からないなあと思いつつ、その方針でコードを組んで流したら通ったので OK.

```python
n = int(input())
a = [int(e) for e in input().split()]
b = [int(e) for e in input().split()]
result = 0
for i in range(n):
  if a[i] > b[i]:
    result += b[i]
    a[i] -= b[i]
    b[i] = 0
  else:
    result += a[i]
    b[i] -= a[i]
    a[i] = 0
    if a[i + 1] > b[i]:
      result += b[i]
      a[i + 1] -= b[i]
      b[i] = 0
    else:
      result += a[i + 1]
      b[i] -= a[i + 1]
      a[i + 1] = 0
print(result)
```

## D - Digits Parade

敗退. 全組み合わせを試すのは TLE 必死だよなと思いつつ、問題を理解をするためだけにまずは全組み合わせのコードを書いてみた.

```python
s = input()
wl = len([c for c in s if c == '?'])
divisor = 10 ** 9 + 7
result = 0
wn = 0
ws = ('%%0%dd' % wl) % wn

def next():
  global wn
  global ws
  wn += 1
  ws = ('%%0%dd' % wl) % wn
  return len(ws) == wl

while True:
  cs = ''
  i = 0
  for c in s:
    if c == '?':
      cs += ws[i]
      i += 1
    else:
      cs += c
  if int(cs) % 13 == 5:
    result += 1
  if not next():
    break
print(result % divisor)
```

問題文についている入出力例すら突破できない速度で絶望. 入力例1の"??2??5"の下2桁を眺めていたら、(i * 10 + 5) % 13 と (((i * 10) % 13) + 5) % 13 って同じだなあと気づき、だったら下から順番に余り0..12のパターン数だけ積み上げていけば、組合せ爆発しないと気づき、コードを組み始めたが、完成する前にタイムアップ. コンテスト終了後も書き続けて、完成させたコードを投げたが、10/26 が TLE で通過できず. コードを眺めても O(n) で最内周は改善の余地が無い. パターン数が最後しか 10 ** 9 + 7 で割ってなく、多倍長計算で遅くなってるかなと思ったので、途中でも割ってみたら TLE は 6/26 にしか減らず途方に暮れる. Twitter の TL で PyPy3 って言ってる人がいたので、試してみたら……あっさり通過した orz.

```python
s = ''.join(reversed(input()))
multiplier = 1
divisor = 10 ** 9 + 7
p = [0 for i in range(13)]
if s[0] != '?':
  p[int(s[0])] = 1
else:
  for j in range(10):
    p[j] = 1
for i in range(1, len(s)):
  multiplier = (10 * multiplier) % 13
  np = [0 for j in range(13)]
  if s[i] != '?':
    r = int(s[i]) * multiplier
    for j in range(13):
      np[(j + r) % 13] = p[j]
  else:
    for j in range(10):
      r = j * multiplier
      for k in range(13):
        np[(k + r) % 13] += p[k]
  for j in range(13):
    p[j] = np[j] % divisor
print(p[5] % divisor)
```

アルゴリズムのオーダーさえちゃんとしていれば Python でもちゃんと通るんだなという印象を atcoder に持っていたのでちょっと残念な気分になった.

追記: Python 3 で カツカツに最適化してみたら TLE 1/26 まで行ったが、結局通らなかった. 残念.

```python
def main():
  s = ''.join(reversed(input()))
  multiplier = 1
  divisor = 10 ** 9 + 7
  list10 = list(range(10))
  list13 = list(range(13))
  p = [0 for i in list13]
  np = [0 for i in list13]
  if s[0] != '?':
    p[int(s[0])] = 1
  else:
    for j in list10:
      p[j] = 1
  for i in range(1, len(s)):
    multiplier = (10 * multiplier) % 13
    if s[i] != '?':
      r = int(s[i]) * multiplier
      for j in list13:
        np[(j + r) % 13] = p[j]
    else:
      r = 0
      for j in list10:
        for k in list13:
          np[(k + r) % 13] += p[k]
        r += multiplier
    for j in list13:
      p[j] = np[j] % divisor
      np[j] = 0
  print(p[5] % divisor)
main()
```

追々記: % 13 を表引きに置き換えてついに AC. 1番時間がかかっているテストケースが 1967 ms なので割とギリギリ. 最内周のループをアンロールすると10%くらい速くなるけどやりたくなさ(^^;

```python
def main():
  s = ''.join(reversed(input()))
  multiplier = 1
  divisor = 10 ** 9 + 7
  list10 = list(range(10))
  list13 = list(range(13))
  p = [0 for i in list13]
  np = [0 for i in list13]
  rt = [i % 13 for i in range(100)]
  if s[0] != '?':
    p[int(s[0])] = 1
  else:
    for j in range(10):
      p[j] = 1
  for i in range(1, len(s)):
    multiplier = (10 * multiplier) % 13
    if s[i] != '?':
      r = int(s[i]) * multiplier
      for j in list13:
        np[(j + r) % 13] = p[j]
    else:
      for j in range(10):
        r = multiplier * j % 13
        for k in list13:
          np[rt[k + r]] += p[k]
    for j in list13:
      p[j] = np[j] % divisor
      np[j] = 0
  print(p[5] % divisor)
main()
```
