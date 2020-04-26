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
