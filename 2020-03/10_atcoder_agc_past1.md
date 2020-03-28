# AtCoder Grand Contest 過去問チャレンジ1

## [AGC003B - Simplified mahjong](https://atcoder.jp/contests/agc003/tasks/agc003_b)

同じカードで最大限ペアを作って、余ったら繰り越して、繰越分を優先して使ってることにする(つまり余った場合は今のカードの方を繰り越す)で OK なのだが、0枚のカードに繰越が来た場合、優先して使うカードがないので、そこで繰越分を捨てなければいけない. そこだけ気にすれば OK.

```python
N = int(input())

result = 0
remainder = 0
for _ in range(N):
    A = int(input())
    result += (A + remainder) // 2
    if A == 0:
        remainder = 0
    else:
        remainder = (A + remainder) % 2
print(result)
```

## [AGC016A - Shrinking](https://atcoder.jp/contests/agc016/tasks/agc016_a)

s の長さはたかだか100で、アルファベットの種類も26しかないので、総当りしても TLE しない. s に含まれる全てのアルファベットについて、それが最後に残るとして、長さ N の文字列 t を長さ N−1 の文字列 t' に変える操作を実際に行って、操作回数の最小値を求めることができる.

```python
s = input()


def f(s, c):
    result = 0
    while len(set(s)) != 1:
        t = ''
        for i in range(len(s) - 1):
            if s[i] == c or s[i + 1] == c:
                t += c
            else:
                t += s[i]
        s = t
        result += 1
    return result


result = float('inf')
for c in set(s):
    result = min(result, f(s, c))
print(result)
```

## [AGC017A - Biscuits](https://atcoder.jp/contests/agc017/tasks/agc017_a)

奇数個入っている袋は偶数個食べて、偶数個入っている袋は何袋でもいい. 奇数個入っている袋が a 袋、偶数個入っている袋が b 袋だとすると、答えは (<sub>a</sub>C<sub>0</sub> + <sub>a</sub>C<sub>2</sub> + ... + <sub>a</sub>C<sub>int(a / 2) * 2</sub>) * (<sub>b</sub>C<sub>0</sub> + <sub>b</sub>C<sub>1</sub> + ... + <sub>b</sub>C<sub>b</sub>) になる. ところで <sub>b</sub>C<sub>0</sub> + <sub>b</sub>C<sub>1</sub> + ... + <sub>b</sub>C<sub>b</sub> は 2<sup>b</sup> なので、後はループを回して奇数の袋の選び方の数を求めればよい.

```python
def comb(n, k):
    if n - k < k:
        k = n - k
    if k == 0:
        return 1
    a = 1
    b = 1
    for i in range(k):
        a *= n - i
        b *= i + 1
    return a // b


N, P = map(int, input().split())
A = list(map(int, input().split()))

odds = sum(a % 2 for a in A)
evens = len(A) - odds

print(sum(comb(odds, i) for i in range(P, odds + 1, 2)) * (2 ** evens))
```

## [AGC007A - Shik and Stone](https://atcoder.jp/contests/agc007/tasks/agc007_a)

移動する過程で、駒が常に右または下に動いていたということは、移動回数が (H - 1) + (W - 1) 回になる. 開始地点を含めて # の数１が H + W - 1 個であれば、上や左には動いていないことになる.

```python
H, W = map(int, input().split())
A = [input() for _ in range(H)]

if H + W - 1 == sum(a.count('#') for a in A):
    print('Possible')
else:
    print('Impossible')
```

## [AGC031A - Colorful Subsequence](https://atcoder.jp/contests/agc031/tasks/agc031_a)

文字列 S の中に a が n 回現れていたとすると、a を使わないパターン、1つ目の a を使うパターン、2つ目の a を使うパターン、... n つ目の a を使うパターンの n + 1 パターンがある. それぞれの文字ごとに組み合わせの数は独立なので、文字ごとに出現数を数えて、出現数 + 1 を積算したのが答えになる.

```python
N = int(input())
S = input()

MOD = 1000000007

d = {}
for c in S:
    if c in d:
        d[c] += 1
    else:
        d[c] = 1

result = 1
for x in d.values():
    result *= x + 1
    result %= MOD
result += MOD - 1
result %= MOD

print(result)
```
