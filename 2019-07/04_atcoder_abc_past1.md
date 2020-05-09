# AtCoder Beginners Contest 過去問チャレンジ1

AtCoder Beginners Selection(以下 abs) を解いたので、AtCoder Beginners Contest (以下 abc)の準備は万端かと思いきや、abs には難易度 ABC 問題しかなく、abc には難易度 DEF の問題が出るようなので、過去問のD以上の問題にチャレンジ.

あと abs を解くときに input() が動かなくてひどい目にあって raw_input() に差し替えたが、Python 3 は input() で大丈夫らしいので、Python 2 から 3 に切り替えた. print に括弧を付けるのがめんどくさいなと思っていたが、map や range が list じゃないことのほうがめんどくさいかもしれない.

## [ABC133D - Rain Flows into Dams](https://atcoder.jp/contests/abc133/tasks/abc133_d)

山iに降った水の量をRiとすると

```
(R1+R2) / 2 = A1
(R2+R3) / 2 = A2
(R3+R4) / 2 = A3
(R4+R5) / 2 = A3
...
(RN+R1) / 2 = AN
```

になることが分かる.
この時 R1 を求めるのは中学で習う連立一次方程式が分かっていれば特に何も難しいことはない.

```
((R1+R2) / 2) - ((R2+R3) / 2) + ((R3+R4) / 2) - ((R4+R5) / 2) + ... + ((RN+R1) / 2) = A1 - A2 + A3 - A4 + ... + AN
R1 = A1 - A2 + A3 - A4 + ... + AN
```

`(Ri+Ri+1) / 2 = Ai` から `Ri+1 = 2 * Ai - Ri` なので、R1 一つ分かりさえすれば残りはドミノ倒し的に分かるので、後はこれを Python のコードに落とすだけ.

```python
N = int(input())
A = list(map(int, input().split()))

result = [0] * N
result[0] = sum(A[::2]) - sum(A[1::2])
for i in range(1, N):
    result[i] = 2 * A[i - 1] - result[i - 1]
print(*result)
```

## [ABC132D - Blue and Red Balls](https://atcoder.jp/contests/abc132/tasks/abc132_d)

再帰的に定式化は出来たものの遅い. 再帰関数の切り札 memoize や、小さい数字の部分は一発で返るようにして高速化したものの、TLE は一つも減らずに敗退.

```python
n, k = [int(e) for e in input().split()]
divisor = 10 ** 9 + 7
cache = {}
def memoize(f):
    global cache
    def _(*args):
        if not args in cache:
            cache[args] = f(*args)
        return cache[args]
    return _
@memoize
def f(n, x):
  if n == 0:
    return 1
  elif n == 1:
    return x
  elif x == 1:
    return 1
  elif x == 2:
    return n + 1
  elif x == 3:
    return (n + 1) * (n + 2) // 2
  else:
    return sum(f(i, x - 1) for i in range(n + 1)) % divisor
for i in range(1, k + 1):
  print(f(k - i, i) * f(n - k - i + 1, i + 1) % divisor)
```

追記:

「1回の操作で連続して並ぶ青いボールを何個でも回収することができる」というのは、要するに赤のボールで区切られた各グループは1回の操作ということなので、K 個の青いボールを i このグループに分けた場合の組み合わせが何通りかということになる.

N個の区別がつかないボールをK個の区別がつくグループに分ける組み合わせ(グループに0個もありうる)は、N個のボールとK-1個の区切り棒を用意して並べた場合の並べ方の数となり、N+K-1のスロットから、区切り帽を置くスロットをK-1個選ぶ組み合わせの数となるので、<sub>N+K-1</sub>C<sub>K-1</sub>となる. N個の区別がつかないボールをK個の区別がつくグループに分ける組み合わせ(グループに必ず1個はある)は、最初にグループに1個づつ分を差し引いておけばいいので、並べ方の数は <sub>N+K-1-K</sub>C<sub>K-1</sub>=<sub>N-1</sub>C<sub>K-1</sub> となる.

青いボールはiグループに1個はある並べ方の数になり <sub>K-1</sub>C<sub>i-1</sub> となり、赤いボールはi-1グループに1個あるのは保証するが、i+1グループの可能性もあるため、<sub>(N-K)+(i+1-1)-(i-1)</sub>C<sub>i</sub>=<sub>N-K+1</sub>C<sub>i</sub> となる.

<sub>N</sub>C<sub>K</sub> を高速に求めるのはパスカルの三角形を用いれば良い.

```python
N, K = map(int, input().split())

n = max(K, N - K + 1)
c = [[0] * (n + 1) for _ in range(n + 1)]
c[0][0] = 1
for i in range(1, n + 1):
    c[i][0] = 1
    for j in range(1, i + 1):
        c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % 1000000007

result = []
for i in range(1, K + 1):
    result.append(c[K - 1][i - 1] * c[N - K + 1][i] % 1000000007)
print('\n'.join(str(i) for i in result))
```

フェルマーの小定理を使うと以下になる.

```python
N, K = map(int, input().split())

n = max(K, N - K + 1)
fac = [0] * (n + 1)
fac[0] = 1
for i in range(n):
    fac[i + 1] = fac[i] * (i + 1) % 1000000007


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], 1000000005, 1000000007) * pow(fac[k], 1000000005, 1000000007) % 1000000007


result = []
for i in range(1, K + 1):
    result.append(mcomb(K - 1, i - 1) * mcomb(N - K + 1, i) % 1000000007)
print('\n'.join(str(i) for i in result))
```

## [ABC131D - Megalomania](https://atcoder.jp/contests/abc131/tasks/abc131_d)

締め切り時間でソートして、順番に処理時間を加算しながら制限時間を超過しないか確認していくだけ. 簡単すぎて、D 問題って難易度に波があるなあという感想.

```python
M = int(input())
AB = [list(map(int, input().split())) for _ in range(M)]

t = 0
AB.sort(key=lambda x: x[1])
for A, B in AB:
    t += A
    if t > B:
        print('No')
        exit()
print('Yes')
```

## [ABC130D - Enough Array](https://atcoder.jp/contests/abc130/tasks/abc130_d)

素直に書き下ろしてみたが、予想通り TLE が出た. ウインドウをずらしながら計算するのって20年近く前に大学で習ったよなあと書き直したら通った. 要するに A[i..j] で初めて K を越えたなら A[i+1...j-1] は絶対 K 未満なので、この区間をチェックするだけ無駄なのだ. なので a<sub>i+1</sub> を先頭とする部分列は、A[i+1..j] からチェックを開始すれば良い.

```python
N, K = map(int, input().split())
a = list(map(int, input().split()))

result = 0
i = 0
j = 0
v = 0
while True:
    v += a[j]
    if v < K:
        j += 1
    else:
        result += N - j
        v -= a[i]
        if j > i:
            v -= a[j]
        i += 1
        if j < i:
            j += 1
    if j == N:
        print(result)
        break
```
