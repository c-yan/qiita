# AtCoder Beginners Contest 過去問チャレンジ2

## ABC129D - Lamp

特に難しいことはなくて、縦方向と横方向で別々に照らされる範囲のテーブルを作って、最後にそれぞれのテーブルの値の和 -1 (現在位置が2回カウントされる分を引く) の最大値を求めればいい. ただこういう配列を延々と嘗める処理は Python では絶望的に遅く TLE の嵐. まあ、PyPy で余裕の AC なんですが. それでも悔しくて Python で通すために最適化. 3回スキャンすると時間がかかるので、横方向だけテーブルを埋めて、縦方向をスキャンしてる時にオンザフライで最大値を求めていくようにしたらなんとか AC.

その後、他人の回答を見ていたら、配列の部分列に代入をする場合、スライスに同じ長さのリストを代入するのが速いと分かった. 毎回リストを生成する部分が重そうな気がしたが、その部分はC言語で書かれた機械語で行われるので Python で頑張るより速いというオチのようである. `yokoi[start:j] = [t] * t` がそれを使っている部分である. この問題以外でこの知識を使うことはあるだろうかw.

```python
from sys import stdin
def main():
  from builtins import range
  readline = stdin.readline
  h, w = map(int, readline().split())
  s = [readline().rstrip('\r\n') + '#' for _ in range(h)]
  s.append('#' * w)
  ft = [[i] * i for i in range(100)]
  yoko = [[0] * w for _ in range(h)]
  for i in range(h):
    start = -1
    si = s[i]
    yokoi = yoko[i]
    for j in range(w + 1):
      if si[j] == '#':
        if start != -1:
          t = j - start
          if t < 100:
            yokoi[start:j] = ft[t]
          else:
            yokoi[start:j] = [t] * t
          start = -1
      else:
        if start == -1:
          start = j
  result = 0
  for i in range(w):
    start = -1
    for j in range(h + 1):
      if s[j][i] == '#':
        if start != -1:
          t = yoko_max + j - start - 1
          if t > result:
            result = t
          start = -1
      else:
        yji = yoko[j][i]
        if start == -1:
          start = j
          yoko_max = yji
        else:
          if yji > yoko_max:
            yoko_max = yji
  print(result)
main()
```

## ABC128D - equeue

K 回の操作の全組み合わせ. 入れたものを再度取り出してもしょうがないので、ガーッと左から連続的に取って、ガーッと連続的に右から取って、手持ちの一番低いほうから順に0未満を筒に詰める(左でも、右でもどっちでも良い、違いはない)といった感じである. 何も考えなくても TLE が出ないので、楽ちん.

```python
n, k = map(int, input().split())
v = list(map(int, input().split()))
m = min(n, k)
result = 0
for i in range(m + 1):
  for j in range(i + 1):
    t = v[:j]
    t.extend(v[n - (i - j):])
    t.sort(reverse = True)
    l = k - i
    while len(t) > 0 and l > 0 and t[-1] < 0:
      t.pop()
      l -= 1
    result = max(result, sum(t))
print(result)
```

## ABC127D - Integer Cards

真面目に書き換えるのはめんどくさいので、書き換え後の値を大きい方から順に並べて、その数を書き換え数分追加して、追加数が n を超えたら、ソートして、大きい方から n 個とればいいかなって.

```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
bc = [list(map(int, input().split())) for _ in range(m)]
bc.sort(key = lambda x: x[1], reverse = True)
t = 0
for b, c in bc:
  a.extend([c] * b)
  t += b
  if t > n:
    break
a.sort(reverse = True)
print(sum(a[:n]))
```

## ABC084D - 2017-like Number

まず素数判定用のテーブルを作る. エラトステネスの篩.
その次に 2～10<sup>5</sup> までの累積和のテーブルを作る.
後は 2～r までの累積和から、2～l までの累積和を引けば答えが簡単に出る.

```python
from sys import stdin
def main():
  from math import sqrt
  readline = stdin.readline
  p = [True] * (10 ** 5 + 1)
  p[0] = False
  p[1] = False
  n = int(sqrt(10 ** 5)) + 1
  for j in range(4, 10 ** 5 + 1, 2):
    p[j] = False
  for i in range(3, n + 1, 2):
    if p[i]:
      for j in range(i + i, 10 ** 5 + 1, i):
        p[j] = False
  c = [0] * (10 ** 5 + 2)
  for i in range(2, 10 ** 5 + 2):
    c[i] = c[i - 1]
    if p[i - 2] and p[(i - 1) // 2]:
      c[i] += 1
  q = int(readline())
  for _ in range(q):
    l, r = map(int, readline().split())
    print(c[r + 2] - c[l + 1])
main()
```

## ABC054D - Mixing Experiment

DP で総当りするだけです(終). といいつつ、時系列無しテーブルで解いたので、これは本当に DP なのか? その回のものを2回使わないようにループを降順に回します.

```python
INF = float('inf')
n, ma, mb = map(int, input().split())
cmax = n * 10
t = [[INF] * (cmax + 1) for _ in range(cmax + 1)]
for _ in range(n):
  a, b, c = map(int, input().split())
  for aa in range(cmax, 0, -1):
    for bb in range(cmax, 0, -1):
      if t[aa][bb] == INF:
        continue
      t[aa + a][bb + b] = min(t[aa + a][bb + b], t[aa][bb] + c)
  t[a][b] = min(t[a][b], c)
result = INF
for a in range(1, cmax):
  for b in range(1, cmax):
    if a * mb == b * ma:
      result = min(result, t[a][b])
if result == INF:
  result = -1
print(result)
```

## ABC049D - 連結 / Connectivity

道路の連結を計算し、鉄道の連結を計算し、都市 i の道路グループID、鉄道グループIDの2要素のタプルを列挙して集計して、集計結果を都市ごとに出力すれば OK. 出題者の想定通りに Union Find を使えば、根のノード番号がグループ ID になり、簡単かつ高速に解ける. 自分は [素集合データ構造](https://ja.wikipedia.org/wiki/%E7%B4%A0%E9%9B%86%E5%90%88%E3%83%87%E3%83%BC%E3%82%BF%E6%A7%8B%E9%80%A0) を見ながら Union Find を書きました. path compression(経路圧縮)は入れないと速度的にむりーだが、こんな軽くて簡単なものを入れないやつの頭はおかしい.

```python
from sys import stdin

def main():
  def find(parent, i):
    t = parent[i]
    if t == -1:
      return i
    t = find(parent, t)
    parent[i] = t
    return t

  def unite(parent, i, j):
    i = find(parent, i)
    j = find(parent, j)
    if i == j:
      return
    parent[i] = j

  from builtins import int, map, range
  readline = stdin.readline

  n, k, l = map(int, readline().split())

  roads = [-1] * n
  rails = [-1] * n

  for i in range(k):
    p, q = map(int, readline().split())
    unite(roads, p - 1, q - 1)

  for i in range(l):
    p, q = map(int, readline().split())
    unite(rails, p - 1, q - 1)

  d = {}
  for i in range(n):
    t = (find(roads, i), find(rails, i))
    if t in d:
      d[t] += 1
    else:
      d[t] = 1

  print(*[d[(find(roads, i), find(rails, i))] for i in range(n)])

main()
```

と書いているが、この問題を解く前は Union Find なぞ知らなかったので、普通の探索で最初は書いてて TLE を出しまくったのだ. Union Find 版で AC をゲットした後も未練たらしく探索版を修正していたらなんか AC がゲットできてしまったw. よって、この問題においては Union Find は必須ではない.

```python
from sys import stdin

def create_links(n, k):
  links = [[] for _ in range(n)]
  for i in range(k):
    p, q = map(int, stdin.readline().split())
    links[p - 1].append(q - 1)
    links[q - 1].append(p - 1)
  return links

def create_groups(n, links):
  groups = list(range(n))
  for i in range(n):
    if groups[i] != i:
      continue
    q = [i]
    while q:
      j = q.pop()
      for k in links[j]:
        if groups[k] != i:
          groups[k] = i
          q.append(k)
  return groups

n, k, l = map(int, input().split())
road_links = create_links(n, k)
rail_links = create_links(n, l)
road_groups = create_groups(n, road_links)
rail_groups = create_groups(n, rail_links)

d = {}
for i in range(n):
  t = (road_groups[i], rail_groups[i])
  if t in d:
    d[t] += 1
  else:
    d[t] = 1

print(*[d[(road_groups[i], rail_groups[i])] for i in range(n)])
```
