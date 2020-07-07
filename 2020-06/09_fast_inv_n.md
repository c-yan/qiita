# Python 3.8 では pow(n, 1000000007 - 2, 1000000007) より pow(n, -1, 1000000007) が良さそう

Python 3.8 の pow は第三引数を指定した場合でも、第二引数に負の値が取れるようになった. その事実は Python 3.8 のリリースノートにも書かれている.

[What’s New In Python 3.8: Other Language Changes](https://docs.python.org/3/whatsnew/3.8.html#other-language-changes)

> For integers, the three-argument form of the pow() function now permits the exponent to be negative in the case where the base is relatively prime to the modulus.

そして、どうも -1 を指定して、逆元を計算するとだいぶ速いようである.

```python
$ python3.8
Python 3.8.3 (default, May 23 2020, 15:50:53)
[GCC 9.3.0] on cygwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from timeit import timeit
>>> m = 1000000007
>>> pow(2, m - 2, m)
500000004
>>> pow(2, -1, m)
500000004
>>> timeit(lambda: [pow(i, m - 2, m) for i in range(1, 10000)], number=100)
3.5805746999103576
>>> timeit(lambda: [pow(i, -1, m) for i in range(1, 10000)], number=100)
1.2788116000592709
```

追記: なぜ速いのかなと思って CPython のソースコードを眺めてみた. [longobject.c](https://github.com/python/cpython/blob/master/Objects/longobject.c) の `long_pow` が実装なのだが、第二引数が負で第三引数が `None` でない場合は `long_invmod` という関数を呼んでいた. `long_invmod` は拡張ユークリッドの互除法で逆元を求めていた.
