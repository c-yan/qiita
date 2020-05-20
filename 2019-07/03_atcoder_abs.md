# AtCoder Beginners Selection 体験記

AtCoder Beginner Contest に一度挑戦してみたいなーと思って、練習のために AtCoder Beginners Selection を解いてみた. 言語は、性能が辛ければ Go で、そうでなければ Python 3 にしようと思って提出フォームを見たら、選択肢に Python 2 があって、print にカッコつけるのダルいなと思って Python 2 になった(ダメ人間).

## PracticeA - Welcome to AtCoder

8分で完了. どっかの記事で Python で AtCoder するときは入力を `input()` で受け取るというのを見ていて、使うの初めてながら使ってみたら `SyntaxError: unexpected EOF while parsing` が出て ??? ってなりながら `raw_input()` に変えて突破. 問題自体は簡単すぎて特に何も言うことはない.

```python
a = int(raw_input())
b, c = [int(e) for e in raw_input().split()]
s = raw_input()
print "%d %s" % (a + b + c, s)
```

## ABC086A - Product

2分で完了. 簡単すぎて特に何も言うことはない.

```python
a, b = [int(e) for e in raw_input().split()]
if a * b % 2 == 0:
  print 'Even'
else:
  print 'Odd'
```
## ABC081A - Placing Marbles

4分で完了. 簡単すぎて特に何も言うことはない.

```python
print len([c for c in raw_input() if c == '1'])
```

## ABC081B - Shift only

9分で完了. `any` の使い方を思い出せば特に難しいことはない.

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
## ABC087B - Coins

4分で完了. まあ、総当たりでいいだろうで、終わりではある. 0..n なので `range` に `+ 1` するのを忘れなければよいだけ.

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
## ABC083B - Some Sums

6分で完了. 各桁の和は一回文字列に変換して、各桁毎に数値に戻せばいいやで、後は総当りすれば OK. `a <= x <= y` みたいに書けるのは知ってたけど、初めて書いたかも. 普通の言語では `a <= x and x <= y` になるのにねー.

```python
n, a, b = [int(e) for e in raw_input().split()]
result = 0
for i in range(1, n + 1):
  if a <= sum(int(e) for e in str(i)) <= b:
    result += i
print result
```
## ABC088B - Card Game for Two

7分で完了. 最後の行に `print` を書き忘れて初の WA を食らう(しょぼーん). 要するに大きい方から順に並べて、Alice が偶数個目、Bob が奇数個目を取ると思えば、Python の強みであるスライス処理一発で終わるので、ちょー楽ちん.

```python
n = int(raw_input())
a = [int(e) for e in raw_input().split()]
a.sort()
a.reverse()
print sum(a[::2]) - sum(a[1::2])
```
## ABC085B - Kagami Mochi

3分で完了. 同じ直径のものは積めないってことは、要するに直径の unique を取ればよいわけで、Python は set に突っ込めば一発でそれを取れるので、後は set の要素数を取ってお終い.

```python
n = int(raw_input())
d = [int(raw_input()) for i in range(n)]
print len(set(d))
```

## ABC085C - Otoshidama

7分で完了. 最初　`k = n + 1 - i - j` になっていて WA. 総当りするだけなので特に難しいことはない.

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
## ABC049C - 白昼夢 / Daydream

1時間くらいで完了. 最初は再帰関数で書いて RE を食らい、あー `maximum recursion depth exceeded` かーと言いながらループ版に書き直し. これが TLE を食らいマジどーしよとなったけど、`s.find(t + w) == 0` って *O*(*n*<sup>2</sup>) じゃねと思い、*O*(*n*) な `s.startswith(t + w)` ならどうだろうと試してみたら通った.

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

## ABC086C - Traveling

40分くらいで完了. 到着した後の残り時間が偶数なら良いんでしょとさらさらっと書き上げて出したら WA. 延々考えてどう考えてもあってるやんけーってなった後に、前の問題と違って Yes / No が大文字小文字混じりであることに気づいた orz. しょうもねえミスすぎた.

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
