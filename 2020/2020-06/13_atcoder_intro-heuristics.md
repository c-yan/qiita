# AtCoder Introduction to Heuristics Contest 参戦記

[Introduction to Heuristics Contest](https://atcoder.jp/contests/intro-heuristics) に参加しました. マラソンコンテストは [Chokudai Contest 004](https://atcoder.jp/contests/chokudai004) 以来の2回目の参加でした. 結果は83275739点で、1731人中497位でした. 最終コードは以下で PyPy で提出しました.

```python
from time import time
from random import random

limit_secs = 2
start_time = time()

D = int(input())
c = list(map(int, input().split()))
s = [list(map(int, input().split())) for _ in range(D)]

def calc_score():
    score = 0
    S = 0
    last = [-1] * 26
    for d in range(D):
        S += s[d][t[d]]
        last[t[d]] = d
        for i in range(26):
            S -= c[i] * (d - last[i])
        score += max(10 ** 6 + S, 0)
    return score

def solution():
    return [i % 26 for i in range(D)]

t = solution()
score = calc_score()
while time() - start_time + 0.14 < limit_secs:
    d = int(random() * D)
    q = int(random() * 26)
    old = t[d]
    t[d] = q
    new_score = calc_score()
    if new_score < score:
        t[d] = old
    else:
        score = new_score
print('\n'.join(str(e + 1) for e in t))
```

## 感想、反省点など

- [Chokudai Contest 004](https://atcoder.jp/contests/chokudai004) の時は得点計算はせず、局所探索法は用いてなかったので、マラソンコンテストの一般的な戦略というものが知れてよかったなと思った.
- 流石に初期解が酷くないかと思う(笑). ラウンドロビンにしたってせめて26種類作って一番いいやつを使うとかなかったのか(笑).→初期解26種類を試したけど得点変わらず!! やっぱ、生兵法は駄目だ、解説を待とう.
- 局所探索法などを使うんだとすると、やはり速度が得点に結びついてしまうので、Python より書くのに時間がかかってしまうとしても C++ で書くべきだった気がする.
- B問題と、C問題の「次のステップ」に良いことが書かれていたが、コンテスト中は全く読んでいなかった、反省. とはいえ、生兵法してもしょうがないので、今後出てくる解説で典型的な戦術を学んでから再考しよう.

## タイムライン

以下は大まかなタイムラインです.

- [21:00] 寝てた(ぉぃ).
- [21:08] 起きて、アァァ! となって急いで https://atcoder.jp/ にアクセスした.
- [21:12] Aの問題文を読んで、訳分かんねえと思いつつ、とりあえず以下を出して、13639点をゲット. 入力は日数しか見ていない(笑).

```python
    D = int(input())
    c = list(map(int, input().split()))
    s = [list(map(int, input().split())) for _ in range(D)]

    for i in range(D):
        print(i % 26 + 1)
```

- [21:32] B問題の入力例の答えがなかなか合わない. しょうがないので 0-indexed を諦めて、1-indexed にしたら合ったので、提出したら通った.

```python
D = int(input())
c = [None] + list(map(int, input().split()))
s = [None] + [[None] + list(map(int, input().split())) for _ in range(D)]
t = [None] + [int(input()) for _ in range(D)]

S = 0
last = [0] * 27
score = 0
for d in range(1, D + 1):
    S += s[d][t[d]]
    last[t[d]] = d
    for i in range(1, 27):
        S -= c[i] * (d - last[i])
    score += max(10 ** 6 + S, 0)
    print(S)
```

- [21:44] C問題が書けたので提出したら TLE して ??? となる.

```python
def calc_satisfaction():
    S = 0
    last = [0] * 27
    for d in range(1, D + 1):
        S += s[d][t[d]]
        last[t[d]] = d
        for i in range(1, 27):
            S -= c[i] * (d - last[i])
    return S

D = int(input())
c = [None] + list(map(int, input().split()))
s = [None] + [[None] + list(map(int, input().split())) for _ in range(D)]
t = [None] + [int(input()) for _ in range(D)]
M = int(input())
for _ in range(M):
    d, q = map(int, input().split())
    old = t[d]
    t[d] = q
    print(calc_satisfaction())
```

- [21:49] B問題を 0-indexed に書き直して再投して AC. `last = [0] * 26` が間違っていた.

```python
D = int(input())
c = list(map(int, input().split()))
s = [list(map(int, input().split())) for _ in range(D)]
t = [int(input()) - 1 for _ in range(D)]

S = 0
last = [-1] * 26
score = 0
for d in range(D):
    S += s[d][t[d]]
    last[t[d]] = d
    for i in range(26):
        S -= c[i] * (d - last[i])
    score += max(10 ** 6 + S, 0)
    print(S)
```

- [21:49] ほぼ同時にC問題を言語を PyPy に変えただけ(つまり 1-indexed のまま) で再投してまた TLE. ここで初めて M≤10<sup>5</sup> の制限を見て、まあいいかとA問題に戻る.
- [22:02] C問題で学んだ内容を踏まえ、1.5秒になるまでランダムで改善するコードに書き換えて、A問題に再投. 0点. score の更新が漏れていることに気づいていない.

```python
from time import time
from random import random

start_time = time()

D = int(input())
c = list(map(int, input().split()))
s = [list(map(int, input().split())) for _ in range(D)]

def calc_score():
    S = 0
    last = [-1] * 26
    score = 0
    for d in range(D):
        S += s[d][t[d]]
        last[t[d]] = d
        for i in range(26):
            S -= c[i] * (d - last[i])
        score += max(10 ** 6 + S, 0)
    return score

def solution():
    return [i % 26 for i in range(D)]

t = solution()
score = calc_score()
while time() - start_time < 1.5:
    d = int(random() * D)
    q = int(random() * 26)
    old = t[d]
    t[d] = q
    new_score = calc_score()
    if new_score < score:
        t[d] = old

for e in t:
    print(e + 1)
```

- [22:14] 悩んで、B問題で正しいことが分かっている満足度を評価基準に変える. その際にスコアの更新が漏れていたことに気づいたが、更新が間違っていてまた0点(爆). あと `while time() - start_time < limit_secs * 0.9:` に変更し、よりギリギリまで改善するようにした.

```python
    if new_S < S:
        t[d] = old
        S = new_S
```

- [22:20] 途中で誤った答えを作っているのだろうかと、最低限 valid にはしようと出力を書き換えたが、そもそも 0 は invalid です(爆). よって、また0点.

```python
for e in t:
    if 0 <= e <= 25:
        print(e + 1)
    else:
        print(0)
```

- [22:25] ようやくスコアの更新が間違っていることに気づき、9634428点をゲット! 順位的には下から20%くらいの位置に. 1位の75%くらいのスコアをとってもこれかよと思ったが、よく見たら一桁間違っていて 7.5% だった(爆).
- [22:30] 言語を PyPy に変更したら一気に 75379880点!! トップの60%くらいの得点になって、順位も上から40%に入った.
- [22:39] 改善を満足度ではなくスコアで評価するように戻した. 82463631点.
- [22:44] `while time() - start_time + 0.15 < limit_secs:` にして、時間ギリギリまで考えるように変更. 81865683点(下がった).
- [22:49] `while time() - start_time + 0.14 < limit_secs:` にして83275738点. これが最高点かつ最終コードとなり、同じコードを後2回投稿したが低い点数しかでなかった.
