# yukicoder contest 278 参戦記

## [A 1335 1337](https://yukicoder.me/problems/no/1335)

書くだけ.

```python
A = input()
S = input()

result = ''
for c in S:
    if c in '0123456789':
        result += A[int(c)]
    else:
        result += c
print(result)
```

## [B 1336 Union on Blackboard](https://yukicoder.me/problems/no/1336)

問題文を見た瞬間に★1.5!?ってなった. 2 3 0 や 2 3 1 を手計算してみたところ、どの順序でも同じ数値になったので、順番に関係ないのかと踏んで前から順に処理するコードを書いて AC.

```python
T = int(input())

m = 1000000007

for _ in range(T):
    N = int(input())
    A = list(map(int, input().split()))
    t = 0
    for a in A:
        t = t * a + (t + a)
        t %= m
    print(t)
```

## [C 1337 Fair Otoshidama](https://yukicoder.me/problems/no/1337)

最初に100円も持っているんだったら数字は自由に変えれそうなので、結局 X + Y + Z を3人で分けて同じ数にできるかだけだよなということでこうなった.

```python
X, Y, Z = map(int, input().split())

if (X + Y + Z) % 3 == 0:
    print('Yes')
else:
    print('No')
```

## [D 1338 Giant Class](https://yukicoder.me/problems/no/1338)

長さ W の配列を初期値 H に初期化して、Y - 1 の最小値を取り、毎回減った数を H * W から引いていけば答えが求まる……と思ったら W≦10<sup>9</sup> で死んだ(爆). 辞書で記録するようにして AC.

```python
from sys import stdin

readline = stdin.readline

H, W, Q = map(int, readline().split())

c = H * W
t = {}
result = []
for _ in range(Q):
    Y, X = map(int, readline().split())
    t.setdefault(X - 1, H)
    p = t[X - 1]
    t[X - 1] = min(t[X - 1], Y - 1)
    c -= p - t[X - 1]
    result.append(c)
print(*result, sep='\n')
```
