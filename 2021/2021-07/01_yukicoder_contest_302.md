# yukicoder contest 302 参戦記

いつもはだいたい途中でギブアップするのだけど、今回は2問解いた時点で残り10分とかそんなんだった……. どっちも瞬殺できないといけなかった…….

## [A 1578 A × B × C](https://yukicoder.me/problems/no/1578)

実際に計算してみると K=1 は A'=BC, B'=AC, C'=AB で (ABC)<sup>2</sup>、K=2 は A''=A<sup>2</sup>BC, B''=AB<sup>2</sup>C, C''=ABC<sup>2</sup> で (ABC)<sup>4</sup> となり、(ABC)<sup>2<sup>K</sup></sup> であることはすぐ見当がつく. K≦10<sup>12</sup> なので 2<sup>K</sup> は計算できない. 繰り返し二乗法で解けないかと考えてドツボにはまってしまったが、フェルマーの小定理により ABC<sup>10<sup>9</sup>+7-1</sup>≡1 (mod 10<sup>9</sup>+7) なので 2<sup>K</sup> を 10<sup>9</sup>+7-1</sup> で割った余りを求めればいいだけだった.

```python
m = 1000000007

A, B, C = map(int, input().split())
K = int(input())

print(pow(A * B * C, pow(2, K, m - 1), m))
```

## [B 1579 New Type of Nim](https://yukicoder.me/problems/no/1579)

ひとまず A, B が10未満について答えを総当りで調べてみた.

```
$ python
Python 3.8.9 (default, May  5 2021, 08:05:34)
[GCC 10.2.0] on cygwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from functools import lru_cache
>>> @lru_cache(maxsize=None)
... def f(a, b, turn):
...     if a == 0 or b == 0:
...         return turn
...     for i in range(1, a + 1):
...         c, d = a - i, b
...         if c == d:
...             if f(c, d, turn) == turn:
...                 return turn
...         else:
...             if f(c, d, turn ^ 1) == turn:
...                 return turn
...     for i in range(1, b + 1):
...         c, d = a, b - i
...         if c == d:
...             if f(c, d, turn) == turn:
...                 return turn
...         else:
...             if f(c, d, turn ^ 1) == turn:
...                 return turn
...     return turn ^ 1
...
>>> for a in range(1, 10):
...   for b in range(a, 10):
...     print(a, b, f(a, b, 0))
...
1 1 1
1 2 1
1 3 0
1 4 0
1 5 0
1 6 0
1 7 0
1 8 0
1 9 0
2 2 0
2 3 0
2 4 0
2 5 0
2 6 0
2 7 0
2 8 0
2 9 0
3 3 1
3 4 1
3 5 0
3 6 0
3 7 0
3 8 0
3 9 0
4 4 0
4 5 0
4 6 0
4 7 0
4 8 0
4 9 0
5 5 1
5 6 1
5 7 0
5 8 0
5 9 0
6 6 0
6 7 0
6 8 0
6 9 0
7 7 1
7 8 1
7 9 0
8 8 0
8 9 0
9 9 1
```

規則性が分かったので後は書くだけ. といいつつ、総当たりのコードで typo をして酷い目にあったのだが…….

```python
A, B = map(int, input().split())

if min(A, B) % 2 == 1 and abs(A - B) <= 1:
    print('Q')
else:
    print('P')
```
