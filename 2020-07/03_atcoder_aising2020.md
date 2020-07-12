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
