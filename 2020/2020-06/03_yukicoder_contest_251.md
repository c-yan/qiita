# yukicoder contest 251 参戦記

## [A 1070 Missing a space](https://yukicoder.me/problems/no/1070)

0を先頭にできないのでその分だけ差っ引けばいい.

```python
C = input()

print(len(C) - 1 - C.count('0'))
```

## [B 1071 ベホマラー](https://yukicoder.me/problems/no/1071)

まずベホマラーのほうが消費 MP が少ない場合は全部ベホマラーで良い. また、ベホイミ N 回の消費 MP よりベホマラーの消費 MP の方が多い場合は全部ベホイミで良い. 問題はその中間なわけですが、例えばベホマラーの消費メモリがベホイミの2倍であれば、一番さいだい HP が高い人以外をベホマラーで回復し、終わったら残り一人の回復が終わってない人をベホイミで回復する. ベホマラーの消費メモリがベホイミの 1.1 倍でも 1.9 倍でも同じ. 2.1 倍ならさいだい HP が一番目と二番目に高い人以外をベホマラーで回復し、終わったら残り二人の回復が終わってない人をベホイミで回復する. つまり ベホマラーの消費メモリのベホイミに対する倍率の ceil になる. 初期 HP が1であることには注意.

```python
from math import ceil

N, K, X, Y = map(int, input().split())
A = list(map(int, input().split()))

B = [((a - 1) + K - 1) // K for a in A]
B.sort(reverse=True)

if Y <= X:
    print(Y * B[0])
    exit()
t = ceil(Y / X) - 1
if t < N:
    print(Y * B[t] + X * sum(B[i] - B[t] for i in range(t)))
else:
    print(X * sum(B[i] for i in range(N)))
```

## [C 1072 A Nice XOR Pair](https://yukicoder.me/problems/no/1072)

A のそれぞれの個数を数えて、A と A xor X のそれぞれの個数をかけ合わせたものを合計する. A or X のときに A との組になってダブルカウントになってしまうので最後に2で割ればいい. が、X = 0 のときだけが特殊で A xor 0 = A で自分自身と組み合わせることになるのでその場合だけ <sub>Aの個数</sub>C<sub>2</sub> を合計する.

```python
N, X = map(int, input().split())
A = [int(input()) for _ in range(N)]

d = {}
for a in A:
    d.setdefault(a, 0)
    d[a] += 1

result = 0
if X == 0:
    for v in d.values():
        result += v * (v - 1) // 2
    print(result)
else:
    for k in d:
        if k ^ X in d:
            result += d[k] * d[k ^ X]
    print(result // 2)
```

## [D 1073 無限すごろく](https://yukicoder.me/problems/no/1073)

敗退. ナイーブなのは書けますけどね. N≤10<sup>18</sup> は無理ですよね.
