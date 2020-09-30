# yukicoder contest 243 参戦記

## [A 1020 Reverse](https://yukicoder.me/problems/no/1020)

手で実際にやってみると K番目からN番目までが順に並んだ後に1番目からK-1番目が順に並んだり、逆に並んだりすることがわかります. 1番目からK-1番目の並びが順になるか逆順になるかは当然操作回数が奇数か偶数かに依存します. よって以下のようなコードで解けました.


```python
N, K = map(int, input().split())
S = input()

if (N - K) % 2 == 0:
    print(S[K-1:] + S[:K-1][::-1])
else:
    print(S[K-1:] + S[:K-1])
```

## [B 1021 Children in Classrooms](https://yukicoder.me/problems/no/1021)

ポインタを移動しながら配列を書き換えて、ポインタの情報をもとに結果を出力することも考えましたが、ややこしくて頭の中がこんがらがったので deque でストレートに操作を実行することにしました. これなら何も難しいことはなく、以下のようなコードで解けました.

```python
from collections import deque

N, M = map(int, input().split())
a = list(map(int, input().split()))
S = input()

q = deque(a)
for c in S:
    if c == 'L':
        t = q.popleft()
        q[0] += t
        q.append(0)
    elif c == 'R':
        t = q.pop()
        q[-1] += t
        q.appendleft(0)
print(*q)
```

追記: ポインタを動かす方式でも解いてみた. ややこしかった.

```python
N, M = map(int, input().split())
a = list(map(int, input().split()))
S = input()

p = 0
for c in S:
    if c == 'L':
        p = max(p - 1, -N + 1)
        if p < 0:
            a[-p] += a[-p - 1]
            a[-p - 1] = 0
    elif c == 'R':
        p = min(p + 1, N - 1)
        if p > 0:
            a[N - 1 - p] += a[N - p]
            a[N - p] = 0

if p > 0:
    print(*([0] * p + a)[:N])
elif p < 0:
    print(*(a + [0] * -p)[-p:N - p])
else:
    print(*a)
```
