# AtCoder Beginner Contest 171 参戦記

20分でE問題まで終わって、これは初の全完!?と思ったが、Fの問題文を見た瞬間に無理だーってなって、232位から700番台まで下がっていくのを80分眺めることになった.

## [ABC171A - αlphabet](https://atcoder.jp/contests/abc171/tasks/abc171_a)

1分半で突破. 書くだけ.

```python
A = input()

if A.isupper():
    print('A')
else:
    print('a')
```

## [ABC171B - Mix Juice](https://atcoder.jp/contests/abc171/tasks/abc171_b)

1分半で突破. 書くだけ. B問題で sort が出てくるのは珍しいのでは?

```python
N, K = map(int, input().split())
p = list(map(int, input().split()))

p.sort()
print(sum(p[:K]))
```

## [ABC171C - One Quadrillion and One Dalmatians](https://atcoder.jp/contests/abc171/tasks/abc171_c)

5分半で突破. 過去にEXCEL列名と数字の双方向の変換を書いたことがあったので、それを引っ張り出してきて Python に落とした. 微妙に26進数でもない辺りがいやらしいというか.

```python
N = int(input())

t = []
while N > 0:
    N -= 1
    t.append(chr(N % 26 + ord('a')))
    N //= 26
print(''.join(t[::-1]))
```

## [ABC171D - Replacing](https://atcoder.jp/contests/abc171/tasks/abc171_d)

6分で突破. 先週の [ABC170E - Count Median](https://atcoder.jp/contests/abc170/tasks/abc170_e) を思い出した. 現在値を引いて、新値を足すという操作をループするのが似てる. 合計値をループごとに計算すると *O*(*NQ*) になってしまうが、引いて足すというつじつま合わせをすることによって計算量が *O*(*N*+*Q*) になって解ける.

```python
from sys import stdin
readline = stdin.readline

N = int(readline())
A = list(map(int, readline().split()))
Q = int(readline())

t = [0] * (10 ** 5 + 1)
s = sum(A)
for a in A:
    t[a] += 1

for _ in range(Q):
    B, C = map(int, readline().split())
    s -= B * t[B]
    s += C * t[B]
    t[C] += t[B]
    t[B] = 0
    print(s)
```

## [ABC171E - Red Scarf](https://atcoder.jp/contests/abc171/tasks/abc171_e)

5分で突破. 一目簡単すぎて、誤読かと一瞬思った. xor の特性からして、自分以外の全ての xor と自分を含めた全ての xor を xor すれば自分が出てくるのは分かる. そして、a を全て xor すれば自分を含めた全ての xor が出来るのも分かる.

```python
N = int(input())
a = list(map(int, input().split()))

t = 0
for e in a:
    t ^= e

print(*[e ^ t for e in a])
```

## [ABC171F - Strivore](https://atcoder.jp/contests/abc171/tasks/abc171_f)

全然わかりません. 本当にありがとうございました. 入力例の oof に o を一つ挿入するのでも、3つダブるのだが、このダブリをどう除去するのかが全然思いつかない.

追記: 解説動画通りに実装しました. 何も考えずに投稿して TLE になって、あるぇ? ってなりましたが、定数倍が重い *O*(<i>N</i>log<i>N</i>) なのでありえる話でした. PyPy で再投して AC. しかし、abc でも aaa でも実は一緒なのですという解説を聞いた瞬間の mjd 感はすごかった. この問題から得た経験値は高かったと思う.

```python
K = int(input())
S = input()

m = 1000000007


def make_factorial_table(n):
    result = [0] * (n + 1)
    result[0] = 1
    for i in range(1, n + 1):
        result[i] = result[i - 1] * i % m
    return result


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], m - 2, m) * pow(fac[k], m - 2, m) % m


fac = make_factorial_table(len(S) - 1 + K)

result = 0
for i in range(K + 1):
    result += pow(26, i, m) * mcomb(len(S) - 1 + K - i, len(S) - 1) * pow(25, K - i, m)
    result %= m
print(result)
```

追々記: テーブルを作るのを止めたら Python でもギリギリ通りました. `pow(n, -1, m)` を `pow(n, m - 2, m)` にすると通らなくなりますが…….

```python
K = int(input())
S = input()

m = 1000000007

result = 0
l = len(S)
t = pow(26, K, m)
u = pow(26, -1, m) * 25 % m
for i in range(K + 1):
    # result += pow(26, K - i, m) * mcomb(len(S) - 1 + i, i) * pow(25, i, m)
    result = (result + t) % m
    t = (t * u) % m * (l + i) % m * pow(i + 1, -1, m) % m
print(result)
```
