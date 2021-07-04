# AtCoder Beginner Contest 208 参戦記

## [ABC208A - Rolling Dice](https://atcoder.jp/contests/abc208/tasks/abc208_a)

1分半で突破. 最小値は1のゾロ目、最大値は6のゾロ目なので、その範囲かをチェックすればいい.

```python
A, B = map(int, input().split())

if A <= B <= 6 * A:
    print('Yes')
else:
    print('No')
```

## [ABC208B - Factorial Yen Coin](https://atcoder.jp/contests/abc208/tasks/abc208_b)

6分で突破. n! は (n-1)! の倍数なので、大きい硬貨が使えるときに、小さい硬貨を使ったほうが枚数が少なくなるパターンはない. なので大きい硬貨から順に使えるかチェックしていけばいい.

```python
P = int(input())

x = 3628800  # 10!
y = 10

result = 0
while P != 0:
    n = P // x
    if n != 0:
        result += n
        P -= n * x
    x //= y
    y -= 1
print(result)
```

## [ABC208C - Fair Candy Distribution](https://atcoder.jp/contests/abc208/tasks/abc208_c)

6分半で突破. KをNで割った余り人だけ1個余分にもらえる. インデックスと値の組を値でソートすれば、どの人が余分にもらえるか分かる.

```python
N, K, *a = map(int, open(0).read().split())

result = [K // N] * N
for i, _ in sorted(enumerate(a), key=lambda x: x[1])[:K % N]:
    result[i] += 1
print(*result, sep='\n')
```
