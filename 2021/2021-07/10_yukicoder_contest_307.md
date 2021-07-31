# yukicoder contest 307 参戦記

## [A 1628 Sorting Integers (MAX of M)](https://yukicoder.me/problems/no/1628)

実際にN個の整数列を生成して、大きい順にソートで並び替えて、文字列として連結すればいいです.

```python
N, *c = map(int, open(0).read().split())

t = []
for i in range(9):
    for _ in range(c[i]):
        t.append(i + 1)

print(''.join(str(i) for i in sorted(t, reverse=True)))
```

## [B 1629 Sorting Integers (SUM of M)](https://yukicoder.me/problems/no/1629)

ある数字 a を一つ抽出して考えてみる. a は b 個存在するとする. a が1の桁の場合のパターン数は (N-1)! となる. ただし区別がつかないものは排除しないといけないので、実際には (N-1)!/b! となる. これが10の桁の場合、100の桁の場合……と繰り返すことになる. 各桁の合計は (10<sup>N</sup>-1)/9 で計算できる. またある数字 a は実際には b 個存在するので b を掛けないといけない. 結論として、ある数字 a に起因して総和に足される数は (10<sup>N</sup>-1)/9*a*(N-1)!/(b-1)! となる. 後は a を1から9に変更しながら総和を取るだけである.

```python
m = 1000000007

N, *c = map(int, open(0).read().split())

fac = [1] * (N + 1)
for i in range(N):
    fac[i + 1] = fac[i] * (i + 1)
    fac[i + 1] %= m

x = fac[N - 1] * (pow(10, N, m) - 1) * pow(9, -1, m) % m
frac = 0
denom = 1
for i in range(9):
    if c[i] == 0:
        continue
    frac += c[i] * (i + 1) * x
    frac %= m
    denom *= pow(fac[c[i]], -1, m)
    denom %= m
print(frac * denom % m)
```
