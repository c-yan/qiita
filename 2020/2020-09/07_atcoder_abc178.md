# AtCoder Beginner Contest 178 参戦記

## [ABC178A - Not](https://atcoder.jp/contests/abc178/tasks/abc178_a)

1分半で突破. xor を使っちゃったけど、タイトルは Not だった(笑).

```python
x = int(input())

print(x ^ 1)
```

## [ABC178B - Product Max](https://atcoder.jp/contests/abc178/tasks/abc178_b)

2分で突破. 端同士のどれかが最大のはずだ.

```python
a, b, c, d = map(int, input().split())

print(max(a * c, a * d, b * c, b * d))
```

## [ABC178C - Ubiquity](https://atcoder.jp/contests/abc178/tasks/abc178_c)

5分で突破. すべての組み合わせの数から、0を含まない組み合わせの数と9を含まない組み合わせの数を引いて、それだと0と9を両方含まない組み合わせの数がダブって引かれているので足し戻したものが答え.

```python
m = 1000000007

N = int(input())

print((pow(10, N, m) - pow(9, N, m) * 2 + pow(8, N, m)) % m)
```

## [ABC178D - Redistribution](https://atcoder.jp/contests/abc178/tasks/abc178_d)

29分で突破. 数列の長さが n の時に S - 3×n を n 個に分配する場合分けの数ってどうするんだって唸っていたら、仕切り棒を入れると思うんだと [ABC132D - Blue and Red Balls](https://atcoder.jp/contests/abc132/tasks/abc132_d) を思い出して解けた. n種類のものから重複を許してr個選ぶ場合の数は<sub>n</sub>H<sub>r</sub>=<sub>n+r−1</sub>C<sub>r</sub>通り.

```python
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


m = 1000000007

S = int(input())

fac = make_factorial_table(S + 10)

result = 0
for i in range(1, S // 3 + 1):
    result += mcomb(S - i * 3 + i - 1, i - 1)
    result %= m
print(result)
```

## [ABC178E - Dist Max](https://atcoder.jp/contests/abc178/tasks/abc178_e)

突破できず. この問題がこんなに解かれているということはと、確信を持って「マンハッタン距離最大」でググったらやっぱりでてきましたね orz.
