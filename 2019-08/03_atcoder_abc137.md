# AtCoder Beginner Contest 137 参戦記

## A - +-x

2分半で突破. 書くだけ.

```python
a, b = map(int, input().split())
print(max(a + b, a- b, a * b))
```

## B - One Clue

4分で突破. 座標 X が右端のパターンと、左端のパターンを考えて、後は -k とか + k が突き抜けていないかを気にするだけ.

```python
k, x = map(int, input().split())
l = max(-1000000, x - k + 1)
r = min(1000000, x + k - 1)
for i in range(l, r + 1):
  print(i)
```
## C - Green Bin

26分半もかかってしまった上に、TLE×2, WA×1. WA は一回 `//` で書いた後、`>> 1` にしようかと思いつつ、結局 `//` に戻した時に誤って `/` と書いたという凡ミス、泣く.
各文字列をアルファベティカルオーダーでソートすれば良いのは1秒で分かるので、後は2重 `for` で `==` な場合だけ `+= 1` するナイーブなコードを書いたが、PyPy でも TLE.
しょうがないので dict で各アナグラムの数を素直に数え上げて、<sub>n</sub>C<sub>2</sub> を計算して合計して回答.

```python
n = int(input())
s = [input() for _ in range(n)]
for i in range(n):
  s[i] = ''.join(sorted(s[i]))
t = {}
for i in range(n):
  if s[i] in t:
    t[s[i]] += 1
  else:
    t[s[i]] = 1
print(sum(t[k] * (t[k] -1) // 2 for k in t))
```

## D - Summer Vacation

敗退. 見た瞬間にナップサックだ、DP だ、予習しててよかったと言いながら、EDP のときのコードを持ってきて打ち込んで WA を食らってから、入出力例を読んでナップサックじゃないことに気づく orz. その後は、思いつきで TLE だらけになるコードを書き上げて時間切れになったけど、TLE にならなかったら WA になるような気もする.

考え方としては

* M - 1 日は、A<sub>i</sub> = 1 の中で、B<sub>i</sub> が最大の日雇いアルバイトをやる.
* M - 2 日は、M -1 日のときの残りと、A<sub>i</sub> = 2 の中で、B<sub>i</sub> が最大の日雇いアルバイトをやる.
* ...

でいいはずなので、その通りにコードを組んでみたが、TLE だらけ. 毎回候補となる日雇いアルバイト全部を嘗めて最大の B<sub>i</sub> を探索しているのがまずいんだろうなあと思って、B の値の最大をキャッシュするようにしたら PyPy3 で通った. (Python3 は TLE 2個)

```python
n, m = map(int, input().split())
d1 = {}
for _ in range(n):
  a, b = map(int, input().split())
  if a in d1:
    d1[a].append(b)
  else:
    d1[a] = [b]
result = 0
d2 = {}
maxb = -1
for i in range(1, m + 1):
  if i in d1:
    for j in d1[i]:
      if j not in d2:
        d2[j] = 0
      d2[j] += 1
      if j > maxb:
        maxb = j
  else:
    if len(d2) == 0:
      continue
  result += maxb
  if d2[maxb] == 1:
    del d2[maxb]
    if len(d2) == 0:
      maxb = 0
    else:
      maxb = max(d2.keys())
  else:
    d2[maxb] -= 1
print(result)
```

過去、main 関数を作ってコードをその中に入れ呼ぶようにすると、変数がローカル変数になるため、1割～2割程度高速化していたのだが、今回は不思議なことに遅くなってしまった. 要研究. あと、Python3 では AC なのに、PyPy3 では WA になるのを初めてみた. PyPy3 の秘孔をついてしまったか.
