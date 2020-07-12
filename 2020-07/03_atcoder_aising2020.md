# AtCoder エイシング プログラミング コンテスト 2020 参戦記

むちゃくちゃ眠かったので、直前に20分仮眠して頑張った.

## [aising2020A - Number of Multiples](https://atcoder.jp/contests/aising2020/tasks/aising2020_a)

1分半で突破. 問題の条件の通り書くだけ. 時間がかかるので、*O*(1) で解こうとかそういう事は考えない.

```python
L, R, d = map(int, input().split())

result = 0
for i in range(L, R + 1):
    if i % d == 0:
        result += 1
print(result)
```

## [aising2020B - An Odd Problem](https://atcoder.jp/contests/aising2020/tasks/aising2020_b)

2分で突破. 問題の条件の通り書くだけ.

```python
N = int(input())
a = list(map(int, input().split()))

result = 0
for i in range(N):
    if (i + 1) % 2 == 1 and a[i] % 2 == 1:
        result += 1
print(result)
```

追記: 偶数番目なんてスライスで飛ばせばよかったんや…….

```python
N, *a = map(int, open(0).read().split())

print(sum(1 for e in a[::2] if e % 2 == 1))
```

## [aising2020C - XYZ Triplets](https://atcoder.jp/contests/aising2020/tasks/aising2020_c)

6分半で突破. N≤10<sup>4</sup> だから3重ループでも通ると思った. 実際、通った.

```python
N = int(input())

result = [0] * (N + 1)
for x in range(1, 101):
    for y in range(1, 101):
        for z in range(1, 101):
            n = x * x + y * y + z * z + x * y + y * z + z * x
            if n > N:
                break
            result[n] += 1

print(*result[1:], sep='\n')
```

## [aising2020D - Anything Goes to Zero](https://atcoder.jp/contests/aising2020/tasks/aising2020_d)

突破できず. A = B * C のとき A % m = (B % m) * (C % m) % m を使って大きな数の余りは出せるなあと思って、それを使って組んだけど、TLE は出るし、RE も出るしでよく分からないことに. 最初の1回目の処理は事前計算で *O*(1) に出来るけど、2回目以降はどうするんだろ.

追記: *f*(*n*) の返す値の最大値は log<sub>2</sub>(*n*) なので 2<sup>2×10<sup>5</sup></sup> → 2×10<sup>5</sup> → 17.6 → 4.1 → 2.0 → 1 → 0 とあっという間に収束する. なので最初の1回だけなんとかすればよかった. TLE を見て *f*(*n*) の高速化に走ったのは間抜けだった.

RE が出ていたのはもともとビットが1つしか立ってなかったとき、その唯一立っているビットが反転されると *popcount* が0になることに気づかず0の除算で出ていた. TLE が出ていたのは log<sub>2</sub>(*X*) すら 2×10<sup>5</sup> なことを意識せずに *O*(<i>N</i>log<i>X</i>) で書いていたため.

着想は合っていたものの、ところどころ抜けがあって解けなかった感じ. 残念.

```python
def popcount(n):
    return bin(n).count("1")


def f(n):
    if n == 0:
        return 0
    return 1 + f(n % popcount(n))


N = int(input())
X = input()

c = X.count('1')
t1 = [0] * N
t1[0] = 1 % (c + 1)
for i in range(1, N):
    t1[i] = (t1[i - 1] << 1) % (c + 1)
x1 = 0
for i in range(N):
    if X[i] == '0':
        continue
    x1 += t1[N - 1 - i]

if c - 1 != 0:
    t2 = [0] * N
    t2[0] = 1 % (c - 1)
    for i in range(1, N):
        t2[i] = (t2[i - 1] << 1) % (c - 1)
    x2 = 0
    for i in range(N):
        if X[i] == '0':
            continue
        x2 += t2[N - 1 - i]

result = []
for i in range(N):
    if X[i] == '0':
        n = (x1 + t1[N - 1 - i]) % (c + 1)
    elif X[i] == '1':
        if c - 1 == 0:
            result.append(0)
            continue
        n = (x2 - t2[N - 1 - i]) % (c - 1)
    result.append(1 + f(n))
print(*result, sep='\n')
```
