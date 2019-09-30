# AtCoder Grand Contest 037 参戦記

# A - Dividing a String

52分で突破. そもそも山でバスに間に合わず、蒲田駅についたのが21時5分って感じで、15分以上遅れてスタート. 問題文をちゃんと見ていなく、最初はすべての分割文字がユニークだと勘違いして、なんて難しい問題だって唸ってた. 正しく題意を理解してサラッと書いたのが以下. ダメでも良いやで流して AC. ~~正直なところ、最後と最後の一個手前が違うことを確認してないので、意地悪なテストケースを用意されると転けそうな気がする.~~ 後から、分割文字は1文字→1文字、1文字→2文字、2文字→1文字の3パターンしか無いのかーって思った.

```python
s = input()
t = ''
k = 0
prev = ''
for i in range(len(s)):
  t += s[i]
  if prev != t:
    k += 1
    prev = t
    t = ''
print(k)
```

追記: 最後と最後の1個手前が同じ場合だが、

* 最後が 2個 1個 1個(例: aa a a) のパターンは 3個 1個(aaa a) にすれば大丈夫
* 最後が 1個 1個 1個(例: b a a) のパターンは 1個 2個(b aa) にすれば大丈夫

ということで大丈夫だった. 文字列の分割結果を取得できるように書き直したのが以下.

```python
def divide_string(s):
    result = []
    prev, t = '', ''
    for i in range(len(s)):
        t += s[i]
        if prev != t:
            result.append(t)
            prev, t = t, ''
    if t != '':
        if len(result) == 1 or len(result[-2]) == 1:
            result[-1] = result[-1] + t
        else:
            result[-2] = result[-2] + t
    return result


S = input()

print(len(divide_string(S)))
```

# C - Numbers on a Circle

敗退. B はもう問題文を見ただけで無理って思ったので飛ばしてこっちをやってた. 問題を見た感想は、「えー、どの順でやればいいの. 総当りしたら組合せ爆発しない?」って気分だった. とりあえず順方向だと最小どころかそもそもAからBにたどり着ける気がしないので、逆向きにやっていくことにし、回数最小ってことはできる限り小さくしない(小さいと引ける数が減って最小にならない)のがいいのかなと、一番大きいのを順に置き換えていくことを計画. 一番大きいのをどんどん取り出すために順序付きキューを使って実装してみた. TLE にはなるものの WA はでず、方針はこれで合ってるかと思ったものの、処理量を減らす方法が全く思いつかずタイムアップ.

```python
def main():
  import sys
  from heapq import heappush, heappop, heapify
  _int = int
  n = _int(input())
  a = [_int(e) for e in input().split()]
  b = [_int(e) for e in input().split()]
  result = 0
  finished = 0
  hq = [(-b[i], i) for i in range(n)]
  heapify(hq)
  while True:
    _, i = heappop(hq)
    if a[i] == b[i]:
      finished += 1
      continue
    if i == 0:
      b[i] -= b[n - 1] + b[1]
    elif i == n - 1:
      b[i] -= b[n - 2] + b[0]
    else:
      b[i] -= b[i - 1] + b[i + 1]
    if a[i] > b[i]:
      print(-1)
      sys.exit()
    result += 1
    if a[i] == b[i]:
      finished += 1
      if finished == n:
        print(result)
        sys.exit()
    else:
      heappush(hq, (-b[i], i))
main()
```

解説 PDF を読んで、一気に減らして大丈夫なのか、本当にこれで最小になるのかと思いつつ、実装したら AC. 未だに本当に最小回数なんだろうかって思っている自分がいる.

```python
def main():
  import sys
  _int = int
  n = _int(input())
  a = [_int(e) for e in input().split()]
  b = [_int(e) for e in input().split()]
  result = 0
  q = [i for i in range(n) if b[i] != a[i]]
  while len(q) != 0:
    nq = []
    c = 0
    for i in q:
      if i == 0 or i == n - 1:
        j = b[(n + i - 1) % n] + b[(n + i + 1) % n]
      else:
        j = b[i - 1] + b[i + 1]
      if j > b[i] - a[i]:
        nq.append(i)
        continue
      c += 1
      k = (b[i] - a[i]) // j
      result += k
      b[i] -= j * k
      if a[i] != b[i]:
        nq.append(i)
    if c == 0 and len(nq) != 0:
      print(-1)
      sys.exit()
    q = nq
  print(result)
main()
```
