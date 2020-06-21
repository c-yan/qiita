# yukicoder contest 253 参戦記

## [A 1081 和の和](https://yukicoder.me/problems/no/1081)

結論的に &sum;<sup>N</sup><sub>i=1</sub> <sub>N-1</sub>C<sub>i-1</sub>A<sub>i</sub> になる. N≤100 なのでパスカルの三角形でも解けるだろうけど、めんどくさいのでいつもの mcomb を貼った.

```python
N = int(input())
A = list(map(int, input().split()))

m = 1000000007

fac = [0] * N
fac[0] = 1
for i in range(N - 1):
    fac[i + 1] = fac[i] * (i + 1) % m


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], m - 2, m) * pow(fac[k], m - 2, m) % m


result = 0
for i in range(N):
    result += mcomb(N - 1, i) * A[i]
    result %= m
print(result)
```

追記: パスカルの三角形版.

```python
N = int(input())
A = list(map(int, input().split()))

m = 1000000007

c = [[0] * N for _ in range(N)]
c[0][0] = 1
for i in range(1, N):
    c[i][0] = 1
    for j in range(1, i + 1):
        c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % m

result = 0
for i in range(N):
    result += c[N - 1][i] * A[i]
    result %= m
print(result)
```

## [B 1082 XORのXOR](https://yukicoder.me/problems/no/1082)

並べ替えた A<sub>i</sub> を元にすると、X = A<sub>1</sub> xor A<sub>2</sub> xor A<sub>2</sub> xor A<sub>3</sub> xor ... xor A<sub>N - 1</sub> xor A<sub>N - 1</sub> xor A<sub>N</sub> となる. A<sub>1</sub> xor A<sub>1</sub> = 1 なので、X = A<sub>1</sub> xor A<sub>N</sub> となる. 結果として任意の i != j の i, j について A<sub>i</sub> xor A<sub>j</sub> の最大値が答えとなる.

```python
N = int(input())
A = list(map(int, input().split()))

result = 0
for i in range(N - 1):
    for j in range(i + 1, N):
        result = max(result, A[i] ^ A[j])
print(result)
```

## [C 1083 余りの余り](https://yukicoder.me/problems/no/1083)

K を i で割ったとして、j > i である j については K % i % j = K % i である. なので A を大きい順にソートして、A<sub>N</sub> で割るまでの総当りをすれば良い. 前進するだけなので計算量は O(2<sup>*N-1*</sup>) となり解ける.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

A.sort(reverse=True)


def f(k, i):
    if i == N:
        return k
    result = -1
    for j in range(i, N):
        result = max(result, f(k % A[j], j + 1))
    return result


print(f(K, 0))
```

## [D 1084 積の積](https://yukicoder.me/problems/no/1084)

結構いいところまで行っていた(テストケースの3/4がAC)が、解けずに終了.

f(l,r) が 10<sup>9</sup> 以上とならない l, r を求めるのは尺取り法を使うというのはすぐ分かる. f(l,r) が 10<sup>9</sup> 未満のとき、l≤x≤r である x について f(x,r) も当然 10<sup>9</sup> 未満だが、これをループで解に畳み込んでいくと、最大 N=10<sup>5</sup> の O(*N*<sup>2</sup>) になってしまうので TLE 必至. つまり、尺取り法のアキュムレータとは別にもう一つアキュムレータが必要となる.

例として、1,2,3 に4を追加することを考える. もう一つのアキュムレータを b とおくと4を追加する前は b=(1×2×3)×(2×3)×(3) となる. これを b=(1×2×3×4)×(2×3×4)×(3×4)×(4) に変えることになる. つまり変更操作は b=b×4<sup>4</sup> となる. 要するに「追加する数<sup>追加後の尺取りの長さ</sup>」を掛けることになる.

次に左端の1を削除することを考える. これは b=(1×2×3×4)×(2×3×4)×(3×4)×(4) を b=(2×3×4)×(3×4)×(4) に変更する操作で、1×2×3×4 を無くす操作となる. ちょうど尺取り法のアキュムレータが 1×2×3×4 で一致しているので、尺取り法のアキュムレータで割ればいい. といっても 10<sup>9</sup>+7 で割った余りなのでフェルマーの小定理を使うことになる. 尺取り法のアキュムレータを a とすると b=b×a<sup>10<sup>9</sup>+7-2</sup> となる.

以上が分かっていれば、O(log<i>N</i>) の冪乗関数を使って、O(<i>N</i>log<i>N</i>) で解が求めれる.

```python
N = int(input())
A = list(map(int, input().split()))

m = 1000000007
l = 0
a = 1
b = 1
result = 1
for r in range(N):
    a *= A[r]
    b *= pow(A[r], r - l + 1, m)
    b %= m
    while a >= 1000000000:
        b *= pow(a, m - 2, m)
        a //= A[l]
        l += 1
    result *= b
    result %= m
print(result)
```

## [E 1085 桁和の桁和](https://yukicoder.me/problems/no/1085)

まったく着手できず.

桁和の桁和が0になるのは全ての桁が0のときだけ. 9で余りを取ってしまうと0が発生してしまうが、9を超えたら9を引くようにすればその問題が回避できる. あとは一桁づつ DP で処理すればいい.

```python
T = input()
D = int(input())

m = 1000000007


def update(t, nt, v):
    for i in range(10):
        ni = i + v
        if ni > 9:
            ni -= 9
        nt[ni] += t[i]
        nt[ni] %= m


t = [0] * 10
t[0] = 1
for c in T:
    nt = [0] * 10
    if c != '?':
        update(t, nt, int(c))
    else:
        for i in range(10):
            update(t, nt, i)
    t = nt
print(t[D])
```
