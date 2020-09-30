# yukicoder contest 262 参戦記

寝坊して40分遅れくらいで参加したら、ジャッジがちょうど止まってて、なんかやたらと Clar が多くて、D問題が不備行きになるし、いやなんかすごい回だった(笑).

## [A 1175 Simultaneous Equations](https://yukicoder.me/problems/no/1175)

普通に中学生の数学なので、係数を揃えて引いて x か y を消して……って感じですよね.

```python
a, b, c, d, e, f = map(int, input().split())

y = (c - f * a / d) / (b - e * a / d)
x = c / a - (b / a) * y
print(x, y)
```

## [B 1176 少ない質問](https://yukicoder.me/problems/no/1176)

だいたい底を2にするのが最適なんだろうけど、そうじゃないやつがあることは A=9 などすぐ思いつく. ただまあ、考慮するべき底はそんなに無いであろうと直感的に思うので、適当に小さい数を試してAC.

```python
from math import ceil, log

A = int(input())

result = float('inf')
for i in range(2, 100):
    result = min(result, i * ceil(log(A, i)))
print(result)
```

## [C 1177 余りは?](https://yukicoder.me/problems/no/1177)

解けず. 難易度詐欺. ★1.5から★2.5に変化した.

## [D 1178 Can you draw a Circle?](https://yukicoder.me/problems/no/1178)

解けず. 式は必ず円(半径は正の数)をなすといいつつ、a と b が違ったら楕円じゃないのかとクルクルした.

## [E 1179 Quadratic Equation](https://yukicoder.me/problems/no/1179)

二次方程式の解の公式は中学生の数学なので、その通りに解くだけって感じですね.

```python
a, b, c = map(int, input().split())

t = b * b - 4 * a * c
if t < 0:
    print('imaginary')
elif t == 0:
    print(-b / (2 * a))
else:
    t = t ** 0.5
    result = [(-b - t) / (2 * a), (-b + t) / (2 * a)]
    result.sort()
    print(*result)
```
