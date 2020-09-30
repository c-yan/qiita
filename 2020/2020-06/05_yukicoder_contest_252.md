# yukicoder contest 252 参戦記

## [A 1076 寿司打](https://yukicoder.me/problems/no/1076)

モンテカルロ法は駄目だったので、真面目に数学しました. 求める期待値を n とすると n = p * (1 + n) + (1 - p) * 0 となるので n = p / (1 - p) となります.

```python
p = float(input())

print(p / (1 - p))
```

## [B 1077 Noelちゃんと星々4](https://yukicoder.me/problems/no/1077)

「広義単調増加 競プロ」で検索すると、POJ-3666 : Making the Grade のコードがでてくるので、広義単調減少列側のコードをコメントアウトして投稿すると解けました(酷).

追記: ちゃんと解きました. 移動後の Y<sub>i-1</sub> が x の時にそれまでの移動させた距離の総和を dp[i - 1][x] とすると、dp[i][x] は min(dp[i - 1][0], dp[i - 1][1], ..., dp[i - 1][x]) + abs(Y<sub>i</sub> - x) となる.

```python
N = int(input())
Y = list(map(int, input().split()))

dp = [[0] * (10 ** 4 + 1) for _ in range(N)]

for i in range(N):
    t = float('inf')
    for j in range(10 ** 4 + 1):
        t = min(t, dp[i - 1][j])
        dp[i][j] = t + abs(Y[i] - j)

print(min(dp[N - 1]))
```

これだと Python では TLE になり PyPy でしか通らない. ところで2次元で書いているが、実際はオーバーラップするところがないので、1次元でも動く. また、Y<sub>i</sub> の移動先の座標は Y<sub>1</sub>, ..., Y<sub>N</sub> のどれかの座標のみとして問題ない. 以上を考慮して書くと、Python でも通るコードになる.

```python
N = int(input())
Y = list(map(int, input().split()))

a = sorted(set(Y))
dp = [0] * len(a)

for i in range(N):
    t = float('inf')
    for j in range(len(a)):
        t = min(t, dp[j])
        dp[j] = t + abs(Y[i] - a[j])

print(min(dp))
```
