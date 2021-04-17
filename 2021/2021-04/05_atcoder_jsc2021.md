# 第二回日本最強プログラマー学生選手権 参戦記

難易度絶壁の直前に数学問題置くのやめて……. 久々の三桁パフォ…….

## [JSC2021A - Competition](https://atcoder.jp/contests/jsc2021/tasks/jsc2021_a)

3分で突破. 書くだけ.

```python
X, Y, Z = map(int, input().split())

if Y * Z % X == 0:
    print(Y * Z // X - 1)
else:
    print(Y * Z // X)
```

## [JSC2021B - Xor of Sequences](https://atcoder.jp/contests/jsc2021/tasks/jsc2021_b)

3分で突破. 書くだけ.

```python
N, M = map(int, input().split())
A = set(map(int, input().split()))
B = set(map(int, input().split()))

print(*sorted((A | B) - (A & B)))
```

## [JSC2021C - Max GCD 2](https://atcoder.jp/contests/jsc2021/tasks/jsc2021_c)

9分で突破. B≤2×10<sup>5</sup> だからシングルループは OK. GCD を a と置くと x は A 以上の最小の a の倍数とし、y は x + a として、y ≤ B であれば、その a は答えとして有効であるということになる. あとは B までループを回してやれよい.

```python
A, B = map(int, input().split())

for a in range(1, B + 1):
    x = (A + (a - 1)) // a * a
    y = x + a
    if y > B:
        continue
    result = a
print(result)
```

## [JSC2021D - Nowhere P](https://atcoder.jp/contests/jsc2021/tasks/jsc2021_d)

64分で突破. [Wolfram|Alpha](https://ja.wolframalpha.com/) に `g(1)=P-1, g(n+1) = g(n) * (P-2)` を入力したら *g*(*n*) = (*P* - 1)(*P* - 2)<sup>*n*-1</sup> と教えてくれたので解けた.

```python
m = 1000000007

N, P = map(int, input().split())

print((P - 1) * pow(P - 2, N - 1, m) % m)
```
