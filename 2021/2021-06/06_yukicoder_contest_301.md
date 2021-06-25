# yukicoder contest 301 参戦記

## [A 1556 Power Equality](https://yukicoder.me/problems/no/1556)

Python の多倍長整数でも流石に10<sup>9</sup>乗は出来ないので真面目に考える. A=B はまず思いつく. それ以外にあるかだが、ぱっと思いつかなかったので A と B を1から1000まで回して確認したところ2と4が出て来たので、後はこいつだけだろうと思って提出したら AC.

```python
A, B = sorted(map(int, input().split()))

if A == B:
    print('Yes')
elif A == 2 and B == 4:
    print('Yes')
else:
    print('No')
```

## [B 1557 Binary Variable](https://yukicoder.me/problems/no/1557)

直感が右端でソートと言ったのでそうした. ちゃんと分かりたい人は、蟻本の P.43 からの区間スケジューリングの部分を読む.

```python
from sys import stdin

readline = stdin.readline

N, M = map(int, readline().split())
LR = [list(map(int, readline().split())) for _ in range(M)]

result = N
t = -1
for L, R in sorted(LR, key=lambda x: x[1]):
    if L <= t <= R:
        continue
    result -= 1
    t = R
print(result)
```
