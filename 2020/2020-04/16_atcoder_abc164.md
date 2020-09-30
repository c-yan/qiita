# AtCoder Beginner Contest 164 参戦記

## [ABC164A - Sheep and Wolves](https://atcoder.jp/contests/abc164/tasks/abc164_a)

2分半で突破. 書くだけだったけど、コードテストが詰まってて、慌ててローカルでテストしたりして時間がかかった.

```python
S, W = map(int, input().split())

if W >= S:
    print('unsafe')
else:
    print('safe')
```

## [ABC164B - Battle](https://atcoder.jp/contests/abc164/tasks/abc164_b)

2分半で突破. B問題なので、素直にシミュレートしても TLE しないので書くだけですね.

```python
A, B, C, D = map(int, input().split())

while True:
    C -= B
    if C <= 0:
        break
    A -= D
    if A <= 0:
        break

if A > 0:
    print('Yes')
else:
    print('No')
```

## [ABC164C - gacha](https://atcoder.jp/contests/abc164/tasks/abc164_c)

2分半で突破. set で distinct して個数を数えるだけ. C問題にしたって簡単すぎませんかね.

```python
N = int(input())

print(len(set(input() for _ in range(N))))
```

## [ABC164D - Multiple of 2019](https://atcoder.jp/contests/abc164/tasks/abc164_d)

敗退.

追記: [ABC158E - Divisible Substring](https://atcoder.jp/contests/abc158/tasks/abc158_e) を簡単にした問題なので、[AtCoder Beginner Contest 158 参戦記](https://qiita.com/c-yan/items/87f5fd32472a964a996b#abc158e---divisible-substring)を見てください. 復習をサボっていなかったら高パフォ取れてたのに…….

```python
S = input()

S = S[::-1]
result = 0
t = [0] * 2019
m = 1
n = 0
for i in range(len(S)):
    t[n] += 1
    n += int(S[i]) * m
    n %= 2019
    result += t[n]
    m *= 10
    m %= 2019
print(result)
```
