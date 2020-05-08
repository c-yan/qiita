# yukicoder contest 247 参戦記

## [A 1046 Fruits Rush](https://yukicoder.me/problems/no/1046)

正の新鮮さの果物があるのであれば、正の新鮮さの果物だけ大きい方から順にK個採用する(K個無い場合は全部). 0以下の果物しか無いのであれb、一番0に近いやつを1つだけ採用する.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

A.sort(reverse=True)
print(A[0] + sum([a for a in A[1:] if a > 0][:K - 1]))
```

## [B 1047 Zero (Novice) ](https://yukicoder.me/problems/no/1047)

分からないので、多めにループを回して0になるかどうか確認するだけのコードを書いたら AC した(酷い). A, B が大きいので、10<sup>5</sup> 回したら TLE します.

```python
A, B = map(int, input().split())

N = 0
for i in range(1, 10000):
    N = A * N + B
    if N == 0:
        print(i)
        exit()
print(-1)
```

## [C 1048 Zero (Advanced) ](https://yukicoder.me/problems/no/1048)

K回の操作後の N は L * K <= N <= R * K になる. R * K 以下の最大の M の倍数が A 以上であれば N = 0 となるような操作列が存在することになる. A が L * K、B が R * K のときの [ABC165A - We Love Golf](https://atcoder.jp/contests/abc165/tasks/abc165_a) を O(1) で解く問題です(笑).


```python
L, R, M, K = map(int, input().split())

if R * K // M * M >= L * K:
    print('Yes')
else:
    print('No')
```
