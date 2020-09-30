# AtCoder パナソニックプログラミングコンテスト2020 参戦記

## [panasonic2020A - Kth Term](https://atcoder.jp/contests/panasonic2020/tasks/panasonic2020_a)

2分半で突破. まあ、書くだけ.

```python
K = int(input())

t = [1, 1, 1, 2, 1, 2, 1, 5, 2, 2, 1, 5, 1, 2, 1, 14, 1, 5, 1, 5, 2, 2, 1, 15, 2, 2, 5, 4, 1, 4, 1, 51]
print(t[K - 1])
```

## [panasonic2020B - Bishop](https://atcoder.jp/contests/panasonic2020/tasks/panasonic2020_b)

6分くらい?で突破. 1WA. H と W が1の場合をすっかり忘れてました.

```python
H, W = map(int, input().split())

if H == 1 or W == 1:
    print(1)
elif W % 2 == 0:
    print(H * W // 2)
else:
    if H % 2 == 0:
        print(H * W // 2)
    else:
        print((W + 1) // 2 + (H - 1) * W // 2)
```

## [panasonic2020C - Sqrt Inequality](https://atcoder.jp/contests/panasonic2020/tasks/panasonic2020_c)

敗退. 整数で計算しないと駄目なんだろうとは分かっていても、整数の式に落とせなかった. 二回二乗すればいいじゃんと言われればあああーってすぐ分かるやつ. 何故かコンテスト中は分からない orz. 数学問題嫌いだー.

```python
a, b, c = map(int, input().split())

if c - a - b > 0 and (c - a - b) * (c - a - b) > 4 * a * b:
    print('Yes')
else:
    print('No')
```

## [panasonic2020D - String Equivalence](https://atcoder.jp/contests/panasonic2020/tasks/panasonic2020_d)

32分半で突破. 1WA. 何回読んでも、何回読んでも定義が頭に入ってこなくて困った. で、完全に定義を誤解して出して WA を食らい、その後に N = 4 くらいまで手で全部書いてようやく分かって AC. 要するに N - 1 までの文字列に、aからそれまでに出ている文字の一番辞書順で大きいやつの次のやつまでを追加したのが答え.

```python
N = int(input())

q = ['a']
for i in range(N - 1):
    nq = []
    for s in q:
        stop = ord(max(s)) + 2
        for i in range(ord('a'), stop):
            nq.append(s + chr(i))
    q = nq
for s in q:
    print(s)
```
