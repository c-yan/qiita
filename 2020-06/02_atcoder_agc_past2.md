# AtCoder Grand Contest 過去問チャレンジ 2

## [AGC009A - Multiple Array](https://atcoder.jp/contests/agc009/tasks/agc009_a)

A<sub>N</sub> のボタンから A<sub>1</sub> のボタンへと逆順に押していくのはすぐ分かる. A<sub>i</sub> が B<sub>i</sub> の倍数であるということは、A<sub>i</sub> mod B<sub>i</sub> = 0 ということだから、A<sub>i</sub> mod B<sub>i</sub> が0じゃないとき、B<sub>i</sub> - A<sub>i</sub> mod B<sub>i</sub> 回押せばいい. ところで、それまでに押した回数分だけ A<sub>i</sub> が増えていることに注意. 以上をもとに、以下のようなコードを書いて答えを求めた.

```python
N = int(input())

A = [None] * N
B = [None] * N
for i in range(N):
    A[i], B[i] = map(int, input().split())

result = 0
for i in range(N - 1, -1, -1):
    t = (A[i] + result) % B[i]
    if t != 0:
        result += B[i] - t
print(result)
```

## [AGC011A - Airport Bus](https://atcoder.jp/contests/agc011/tasks/agc011_a)

バスは今待っている先頭の人の限界時刻が来るか、待っている人がC人になった瞬間に出発する. なので、先頭の人の限界時刻と、現在の待ち人数を変数に入れてループを回せば良い. i番目の乗客といいつつ、時刻 T<sub>i</sub> は単調増加になっていないことに注意.

```python
N, C, K = map(int, input().split())
T = [int(input()) for _ in range(N)]
T.sort()

c = 0
l = float('inf')
result = 0
for i in range(N):
    t = T[i]
    if t > l:
        result += 1
        l = float('inf')
        c = 0
    if c == 0:
        l = t + K
    c += 1
    if c == C:
        result += 1
        l = float('inf')
        c = 0
if c != 0:
    result += 1
print(result)
```

## [AGC034A - Kenken Race](https://atcoder.jp/contests/agc034/tasks/agc034_a)

人間が手でやる分には簡単だけど、実装するのはめんどくさいなあと唸っていたが、右にしか行けないというのを見落としていただけだった(笑). ゴールがより右にある方を優先的に動かし、動かせなければもう一方を動かし、両方動けなかったら No を表示する、それだけ.

```python
N, A, B, C, D = map(int, input().split())
S = input()


def move_snuke():
    global A, B
    if B == D:
        return False
    if S[B + 1] == '.' and A != B + 1:
        B += 1
        return True
    if S[B + 2] == '.' and A != B + 2:
        B += 2
        return True
    return False


def move_fnuke():
    global A, B
    if A == C:
        return False
    if S[A + 1] == '.' and B != A + 1:
        A += 1
        return True
    if S[A + 2] == '.' and B != A + 2:
        A += 2
        return True
    return False


S = '#' + S
while True:
    if C < D:
        if not move_snuke():
            if not move_fnuke():
                print('No')
                break
    else:
        if not move_fnuke():
            if not move_snuke():
                print('No')
                break
    if A == C and B == D:
        print('Yes')
        break
```

## [AGC035A - XOR Circle](https://atcoder.jp/contests/agc035/tasks/agc035_a)

先頭を a、2番目を b とすると、3番目は a xor b、4番目は a、5番目は b となり、たかだか3種類の数字しか出てこないことになる. なので、a<sub>i</sub> の数字が4種類以上あったらその時点で No としてよい. 3種類であれば、先頭2つは9通りしか無く、全組み合わせをシミュレーションしても N≤10<sup>5</sup> から最大 O(9×10<sup>5</sup>) となり、AC できる.

```python
from itertools import product

N = int(input())
a = list(map(int, input().split()))

c = {}
for e in a:
    c.setdefault(e, 0)
    c[e] += 1

if len(set(c)) > 3:
    print('No')
    exit()

t = {}
for x, y in product(set(c), repeat=2):
    i = x
    j = y
    t[i] = 1
    if t[i] > c[i]:
        continue
    t.setdefault(j, 0)
    t[j] += 1
    if t[j] > c[j]:
        continue
    for _ in range(N - 2):
        k = i ^ j
        t.setdefault(k, 0)
        t[k] += 1
        c.setdefault(k, 0)
        if t[k] > c[k]:
            j = -1
            break
        i, j = j, k
    if j ^ x == y:
        print('Yes')
        exit()
print('No')
```

## [AGC028A - Two Abbreviations](https://atcoder.jp/contests/agc028/tasks/agc028_a)

L は N と M で割りきれるので、N と M の最小公倍数の倍数となる. L / N * i == L / M * j のとき S[i] == T[j] でないといけないが、L が L * a になったとしても結局両辺 a 倍されるだけのため、比較対象とする i, j の組は変わらない. 回答は最短のものであるため L は N と M の最小公倍数となる.

```python
from fractions import gcd


def lcm(x, y):
    return x // gcd(x, y) * y


N, M = map(int, input().split())
S = input()
T = input()

for i in range(N):
    if M * i % N == 0 and S[i] != T[M * i // N]:
        print(-1)
        exit()
print(lcm(N, M))
```

## [AGC022A - Diverse Word](https://atcoder.jp/contests/agc022/tasks/agc022_a)

次の多彩な単語は

1. 文字列の長さが26文字未満の場合は、使われてない文字で一番辞書順で小さいものを足したもの
2. 文字列の長さが26文字の場合には、末尾の一文字を取って、最後の文字を使われていない文字の中で最後の文字より大きい一番辞書順で小さいものを足したもの. そのようなものが存在しなければまた末尾の一文字を取ってを繰り返す.

となる.

所詮最大でも高々長さ26の文字列なので、実際に取ったり足したりして全然問題ないので、実装はそれほど大変ではない.

```python
def next_diverse_word(s):
    alphabets = set(chr(ord('a') + i) for i in range(26))
    if s == 'zyxwvutsrqponmlkjihgfedcba':
        return -1
    r = alphabets - set(s)
    if len(r) != 0:
        return s + sorted(r)[0]
    s = list(s)
    r = [s[-1]]
    s = s[:-1]
    while True:
        k = s[-1]
        r.append(s[-1])
        s = s[:-1]
        t = [c for c in r if c > k]
        if len(t) != 0:
            s.append(sorted(t)[0])
            return ''.join(s)


S = input()
print(next_diverse_word(S))
```
