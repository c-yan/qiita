# yukicoder contest 259 参戦記

## [A 1139 Slime Race](https://yukicoder.me/problems/no/1139)

「衝突時間を順番に割り出して処理していかないといけない、すごい難しい!」と思わせておいて、速度が引き継がれるので、衝突がないとしても同じだった. 引っ掛けられた、やられた.

速度の合計でDを割って切り上げたのが答え.

```python
N, D = map(int, input().split())
x = list(map(int, input().split()))
v = list(map(int, input().split()))

t = sum(v)
print((D + t - 1) // t)
```

## [B 1140 EXPotentiaLLL!](https://yukicoder.me/problems/no/1140)

解けず. フェルマーの小定理が絡んでいるのかなあと悩んだけど、数式変換思いつかず.

## [C 1141 田グリッド](https://yukicoder.me/problems/no/1141)

累積積で簡単に解けるやーと書いた後に、A<sub>i,j</sub> が0の時に何が起こるかを考えて無くてハマった.

0を除いた累積積を、全部と各行と各列に対して計算する. 上からr<sub>i</sub>行目、または左からc<sub>i</sub>列目を塗りつぶしてもまだ0が残っていたら、そのクエリの答えは0. 0が残っていなかったら、事前に計算した累積積を使って *O*(1) でクエリの答えを計算すればいい.

```python
H, W = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(H)]
Q = int(input())

m = 1000000007

total = 1
rows = [1] * H
cols = [1] * W
totalm = 0
rowsm = [0] * H
colsm = [0] * W
for i in range(H):
    for j in range(W):
        x = A[i][j]
        if x == 0:
            totalm += 1
            rowsm[i] += 1
            colsm[j] += 1
        else:
            total *= x
            total %= m
            rows[i] *= x
            rows[i] %= m
            cols[j] *= x
            cols[j] %= m

result = []
for _ in range(Q):
    r, c = map(lambda x: int(x) - 1, input().split())
    x = A[r][c]
    t = 0
    if x == 0:
        t = 1
    if totalm - rowsm[r] - colsm[c] + t > 0:
        result.append(0)
    else:
        if x == 0:
            result.append(total * pow(rows[r], -1, m) % m * pow(cols[c], -1, m) % m)
        else:
            result.append(total * pow(rows[r], -1, m) % m * pow(cols[c], -1, m) % m * x % m)
print(*result, sep='\n')
```
