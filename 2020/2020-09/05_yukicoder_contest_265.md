# yukicoder contest 265 参戦記

## [A 1223 I hate Golf](https://yukicoder.me/problems/no/1223)

[ABC177A - Don't be late](https://atcoder.jp/contests/abc177/tasks/abc177_a) かなと思ったが、座標がマイナスの可能性があった. まあ、abs つけるだけだよねと思いつつ、ABC177 のときにはやらなかった = 付け忘れをやらかすなどした.

```python
N, K, T = map(int, input().split())

if K * T >= abs(N):
    print('Yes')
else:
    print('No')
```

## [B 1224 I hate Sqrt Inequality](https://yukicoder.me/problems/no/1224)

a,b≤10<sup>18</sup> を見て、素因数分解して共通の素数で割りたいけど、エラトステネスの篩が使えない値域じゃん、どうすればいいんだと悩んでいた. はい、GCDで割ればいいだけですね orz. 割り切れるということは、a×10<sup>N</sup>%b=0 ということなので、aとbのGCDで割ったbが2<sup>N</sup>×5<sup>M</sup>になっていればOK.

```python
from math import gcd

a, b = map(int, input().split())

t = gcd(a, b)
b //= t
while b % 2 == 0:
    b //= 2
while b % 5 == 0:
    b //= 5

if b == 1:
    print('No')
else:
    print('Yes')
```

## [C 1225 I hate I hate Matrix Construction](https://yukicoder.me/problems/no/1225)

typo で自爆しまくった(笑). まず S にも T にも2がある場合は、もう論理和が0になることはありえないので、1と2しかないことが保証され、2が指定されたところ以外に1を置く必要はない. S か T のどちらかだけに2がある場合、無い方は全部論理和が1になること確定. 2がある方は、1の数だけ適当にどこかに1を置けば良い. S にも T にも2がない場合には、1の数だけ1を置いていくことになるが、1個置くごとに S, T のノルマを一つづつ消せるので、それぞれの1の数の大きい方の数だけ置けば良い.

```python
N = int(input())
S = list(map(int, input().split()))
T = list(map(int, input().split()))

m = [[0] * N for _ in range(N)]

s1 = S.count(1)
t1 = T.count(1)
s2 = S.count(2)
t2 = T.count(2)

if s2 == 0 and t2 == 0:
    print(max(s1, t1))
elif s2 > 0 and t2 > 0:
    print(s2 * N + t2 * N - s2 * t2)
elif s2 == 0:
    print(t1 + t2 * N)
elif t2 == 0:
    print(s1 + s2 * N)
```
