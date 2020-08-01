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

追記: A<sup>b<sup>c</sup></sup> は A<sup>b×c</sup> というのは初歩中の初歩の数学である. ここで A<sup>P!</sup> は A<sup>P×(P-1)×(P-2)!</sup>=A<sup>P×(P-2)!×(P-1)</sup>=A<sup>P×(P-2)!<sup>P-1</sup></sup> と書き換えることが出来る. フェルマーの小定理より A<sup>P×(P-2)!</sup> が P と互いに素であれば A<sup>P!</sup>=1 (mod P) となる. ところでPは合成数ではなく素数なので、結局は A と P が互いに素なのかだけ判定すれば良い.

なお、問題文に書かれている通り Python では TLE になるので、以下のコードは PyPy で提出する必要がある.

```python
def make_prime_table(n):
    sieve = list(range(n + 1))
    sieve[0] = -1
    sieve[1] = -1
    for i in range(2, int(n ** 0.5) + 1):
        if sieve[i] != i:
            continue
        for j in range(i * i, n + 1, i):
            if sieve[j] == j:
                sieve[j] = i
    return sieve


readline = open(0).readline

prime_table = make_prime_table(5 * 10 ** 6)

T = int(readline())
result = []
for _ in range(T):
    A, P = map(int, readline().split())
    if prime_table[P] != P:
        result.append(-1)
    else:
        if A % P == 0:
            result.append(0)
        else:
            result.append(1)
print(*result, sep='\n')
```

追々記: A<sup>P-1<sup>P×(P-2)!</sup></sup>に組み替えたほうが分かりやすかった. 1<sup>P×(P-2)!</sup> か 0<sup>P×(P-2)!</sup> になるので、1か0になると一目である.

追々々記: Python でも通った.

```python
def make_prime_table(n):
    sieve = [True] * (n + 1)
    sieve[0] = False
    sieve[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if not sieve[i]:
            continue
        for j in range(i * i, n + 1, i):
            sieve[j] = False
    return sieve


def main():
    readline = open(0).readline

    prime_table = make_prime_table(5 * 10 ** 6)

    T = int(readline())
    result = []
    for _ in range(T):
        A, P = map(int, readline().split())
        if not prime_table[P]:
            result.append(-1)
        else:
            if A % P == 0:
                result.append(0)
            else:
                result.append(1)
    print(*result, sep='\n')


main()
```

## [C 1141 田グリッド](https://yukicoder.me/problems/no/1141)

累積積で簡単に解けるやーと書いた後に、A<sub>i,j</sub> が0の時に何が起こるかを考えて無くてハマった.

0を除いた累積積を、全部と各行と各列に対して計算する. 上からr<sub>i</sub>行目、または左からc<sub>i</sub>列目を塗りつぶしてもまだ0が残っていたら、そのクエリの答えは0. 0が残っていなかったら、事前に計算した累積積を使って *O*(1) でクエリの答えを計算すればいい.

```python
readline = open(0).readline

H, W = map(int, readline().split())
A = [list(map(int, readline().split())) for _ in range(H)]
Q = int(readline())

m = 1000000007

total = 1
rows = [1] * H
cols = [1] * W
total0 = 0
rows0 = [0] * H
cols0 = [0] * W
for i in range(H):
    for j in range(W):
        x = A[i][j]
        if x == 0:
            total0 += 1
            rows0[i] += 1
            cols0[j] += 1
        else:
            total *= x
            total %= m
            rows[i] *= x
            rows[i] %= m
            cols[j] *= x
            cols[j] %= m

result = []
for _ in range(Q):
    r, c = map(lambda x: int(x) - 1, readline().split())
    x = A[r][c]
    t = total0 - rows0[r] - cols0[c]
    if x == 0:
        t += 1
    if t > 0:
        result.append(0)
        continue
    t = total * pow(rows[r], -1, m) % m * pow(cols[c], -1, m) % m
    if x != 0:
        t *= x
        t %= m
    result.append(t)
print(*result, sep='\n')
```
