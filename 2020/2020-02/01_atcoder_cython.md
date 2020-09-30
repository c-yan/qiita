# AtCoder に Cython がやってくる!!

[言語アップデート](https://twitter.com/atcoder/status/1223141026707017728)クルーってことで、新ジャッジテスト環境で Cython が試せたので触ってみました. ちなみに Cython に触るのは初めてで何もわかっていません(笑)

ちなみに言語アップデート後の Python 3 は 3.8.0 で、Cython は 0.29.14 です.

## Python のコードはそのまま動く

ABC085C - Otoshidama で試してみました. Python 3 で 678 ms で AC したソースコードをそのまま Cython で提出したら 614ms で AC しました.

```python
from sys import exit

N, Y = map(int, input().split())

for i in range(N + 1):
    for j in range(N + 1 - i):
        k = N - i - j
        if 10000 * i + 5000 * j + 1000 * k == Y:
            print('%d %d %d' % (i, j, k))
            exit()
print('-1 -1 -1')
```

## シンタックスエラーが RE ではなく CE になる!

しょぼい typo をしてもコンパイラ勢は CE でノーペナなのに、Python 3 は RE でペナルティーでずるいと思っていませんでしたか? Cython は CE になります. やったね!!

試しに最初の代入文の = を消すとこんな感じのエラーになります.

```
Error compiling Cython file:
------------------------------------------------------------
...
from sys import exit

N, Y  map(int, input().split())
     ^
------------------------------------------------------------

Main.py:3:6: Syntax error in simple statement list
./Main.c:1:2: error: #error Do not use this file, it is the result of a failed Cython compilation.
    1 | #error Do not use this file, it is the result of a failed Cython compilation.
      |  ^~~~~
```

## 型付けをすると速い!

Cython のドキュメントを見ると .pyx ファイルを書いてコンパイルして、.py でそれをインポートして使うみたいな感じで2つソースコードが必要そうでどう使えばいいのか最初わからなかったのですが、Cython には Pure Python Mode というのがあって、AtCoder ではこれを使うようです.

結果、107ms で AC. 6倍速くなった!!

```python
from cython import longlong

def main():
    N: longlong
    Y: longlong
    N, Y = map(int, input().split())

    i: longlong
    j: longlong
    for i in range(N + 1):
        for j in range(N + 1 - i):
            k: longlong = N - i - j
            if 10000 * i + 5000 * j + 1000 * k == Y:
                print('%d %d %d' % (i, j, k))
                return
    print('-1 -1 -1')

if __name__ == '__main__':
    main()
```

## Appendix: PyPy 3 だと

言語アップデート後の PyPy 3 は 7.3.0 で、150ms で AC. ただし、消費メモリは Python 3 / Cython の 8.7MB に対し、64MB と大幅アップでした.
