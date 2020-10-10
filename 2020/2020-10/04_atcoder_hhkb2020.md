# AtCoder HHKB プログラミングコンテスト 2020 参戦記

## [HHKB2020A - Keyboard](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_a)

2分で突破. 書くだけ.

```python
S = input()
T = input()

if S == 'Y':
    print(T.upper())
elif S == 'N':
    print(T)
```

## [HHKB2020B - Futon](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_b)

3分で突破. 書くだけ.

```python
H, W = map(int, input().split())
S = [input() for _ in range(H)]

result = 0
for h in range(H):
    for w in range(W - 1):
        if S[h][w] == '.' and S[h][w + 1] == '.':
            result += 1
for h in range(H - 1):
    for w in range(W):
        if S[h][w] == '.' and S[h + 1][w] == '.':
            result += 1
print(result)
```

## [HHKB2020C - Neq Min](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_c)

6分半で突破. 制限からして *O*(*N*) にしないとダメそうだなあと思いつつ、一目でその方法が見えなくて悩んだ記憶がある割にそれほど時間がかかってないのが不思議. 毎行0から考え直していたら*O*(1)になる気がしなかったので、前の行の結果は受け継ぐとして、単調増加だからと考えた辺りでわかった.

```python
N, *p = map(int, open(0).read().split())

result = []
t = 0
s = set()
for x in p:
    s.add(x)
    while t in s:
        t += 1
    result.append(t)
print(*result, sep='\n')
```

## [HHKB2020D - Squares](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_d)

30分くらい悩んだけど、端の処理が難しくてどうにも解けなかった. 問題文を読んだ感触でも、順位表でもEのほうが簡単そうだったのでそっちへ.

## [HHKB2020E - Lamps](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_e)

60分くらい悩んだけど、交差している部分の処理が難しくてどうにも解けなかった.
