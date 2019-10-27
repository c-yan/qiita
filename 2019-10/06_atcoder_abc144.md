# AtCoder Beginner Contest 144 参戦記

## A - 9x9

2分で突破. 書くだけ.

```python
A, B = map(int, input().split())

if A < 10 and B < 10:
    print(A * B)
else:
    print(-1)
```

## B - 81

7分で突破. 書くだけだったんだけど、コードテストが正しく動かなくて(何故か標準出力が戻ってこない)、無駄に時間を使ってしまった.

```python
from sys import exit

N = int(input())

for i in range(1, 10):
    for j in range(1, 10):
        if N == i * j:
            print('Yes')
            exit()
print('No')
```

## C - Walk on Multiplication Table

5分で突破. できるだけ小さい数の掛け合わせがいいので、sqrt から下ろしていけばいいよなと瞬時に思いついたので、あとは書くだけだったのだが、コードテストが不調でローカルで入出力例を動かす羽目になったので時間が無駄にかかってしまった.

```python
from math import sqrt
from sys import exit

N = int(input())

for i in range(int(sqrt(N)) + 1, -1, -1):
    if N % i == 0:
        print((i - 1) + (N // i - 1))
        exit()
```

## D - Water Bottle

突破できず. WA, RE が1個づつ残ってしまった. 解説を見たら、半分未満の方の式の atan を asin と取り違えていた. 三角関数なんて嫌いだー. (というか、atan とか初めて使ったぞ)

```python
from math import atan, pi

a, b, x = map(int, input().split())

if x >= a * a * b / 2:
    print(atan((a*a*b-x)/(a*a*a/2))/(2 * pi)*360)
else:
    print(90 - atan(x/(a*b*b/2))/(2 * pi)*360)
```
