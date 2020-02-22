# AtCoder Beginner Contest 156 参戦記

## ABC156A - Beginner

4分で突破. 書くだけ.

```python
N, R = map(int, input().split())

if N >= 10:
    print(R)
else:
    print(R - 100 * (10 - N))
```

## ABC156B - Digits

2分で突破. 書くだけ. K進数の 10 は K で、100 は K<sup>2</sup> だと分かっていれば、K で何回割れるか調べるだけと分かるはず.

```python
N, K = map(int, input().split())

result = 0
while N != 0:
    result += 1
    N //= K
print(result)
```

## ABC156C - Rally

3分半で突破. 書くだけ……なのは、この計算をしなれてるからなんですが. (X<sub>i</sub>−P)<sup>2</sup> が最小になる P は重心. 減色の K-means で3次元のこの計算をしてます…….

```python
N = int(input())
X = list(map(int, input().split()))

P = int(sum(X) / N + 0.5)
print(sum((x - P) * (x - P) for x in X))
```

## ABC156D - Bouquet

70分半で突破. まず <sub>n</sub>C<sub>0</sub>+<sub>n</sub>C<sub>1</sub>+...+<sub>n</sub>C<sub>n</sub> = 2<sup>n</sup> を知っていないといけない. 私は知りませんでした orz. パスカルの三角形の Wikipedia 見てたら書いてあって知りました. これで過去に書いた mpow と mcomb を貼って終わりかと思ったら n! を求める部分があって 2≤n≤10<sup>9</sup> に殺される. a, b はたかだか 10<sup>5</sup> であることに気づき、<sub>n</sub>C<sub>k</sub> を求める別の式に組み替えてようやく解けた. 良問!

```python
def mpow(x, n):
    result = 1
    while n != 0:
        if n & 1 == 1:
            result *= x
            result %= 1000000007
        x *= x
        x %= 1000000007
        n >>= 1
    return result


def mcomb(n, k):
    a = 1
    b = 1
    for i in range(k):
        a *= n - i
        a %= 1000000007
        b *= i + 1
        b %= 1000000007
    return a * mpow(b, 1000000005) % 1000000007


n, a, b = map(int, input().split())

result = mpow(2, n) - 1
result -= mcomb(n, a)
result + 1000000007
result %= 1000000007
result -= mcomb(n, b)
result + 1000000007
result %= 1000000007
print(result)
```
