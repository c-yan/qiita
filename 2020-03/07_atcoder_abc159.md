# AtCoder Beginner Contest 159 参戦記

## [ABC159A - The Number of Even Pairs](https://atcoder.jp/contests/abc159/tasks/abc159_a)

3分半で突破. 奇数のペアか、偶数のペアが答えになる. <sub>k</sub>C<sub>2</sub> = k(k-1)/2 が分かっていればあとは書くだけなのだが、A問題にしては難しいね、これ.

```python
N, M = map(int, input().split())

print(N * (N - 1) // 2 + M * (M - 1) // 2)
```

## [ABC159B - String Palindrome](https://atcoder.jp/contests/abc159/tasks/abc159_b)

12分で突破. 時間かかりすぎた orz. 回分チェックルーチンを書いて、後は 1-indexed を 0-indexed に変換しながら、言われたとおりにチェックしていくだけ. B問題にしては難しいね、これ.

```python
S = input()


def is_palindrome(s):
    return s == s[::-1]


N = len(S)

if not is_palindrome(S):
    print('No')
    exit()

if not is_palindrome(S[:(N - 1) // 2]):
    print('No')
    exit()

if not is_palindrome(S[(N + 3) // 2 - 1:]):
    print('No')
    exit()

print('Yes')
```

## [ABC159C - Maximum Volume](https://atcoder.jp/contests/abc159/tasks/abc159_c)

2分で突破. A問題の間違いじゃないの、これ?

```python
L = int(input())

print((L / 3) ** 3)
```

## [ABC159D - Banned K](https://atcoder.jp/contests/abc159/tasks/abc159_d)

10分で突破. 当然 k 番目のボールを除いた場合の数を毎回全部計算し直していたら TLE. とりあえず抜かない場合の数を計算して、k 番目の値の場合の数だけ調整して答える.

```python
N = int(input())
A = list(map(int, input().split()))

d = {}
for a in A:
    if a in d:
        d[a] += 1
    else:
        d[a] = 1

s = 0
for k in d:
    s += d[k] * (d[k] - 1) // 2

for i in range(N):
    t = d[A[i]]
    print(s - t * (t - 1) // 2 + (t - 1) * (t - 2) // 2)
```

## [ABC159E - Dividing Chocolate](https://atcoder.jp/contests/abc159/tasks/abc159_e)

敗退. 累積和でホワイトチョコの数え上げを *O*(*HW*) から *O*(*H*) に軽減して、再帰関数で総当りしたが、TLE & WA でした. 総当たりなのに WA なのでどこか実装がバグってますね…….
