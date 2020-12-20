# パナソニックプログラミングコンテスト (AtCoder Beginner Contest 186) 参戦記

## [ABC186A - Brick](https://atcoder.jp/contests/abc186/tasks/abc186_a)

1分で突破. 書くだけ.

```python
N, W = map(int, input().split())

print(N // W)
```

## [ABC186B - Blocks on Grid](https://atcoder.jp/contests/abc186/tasks/abc186_b)

2分で突破. 書くだけ.

```python
H, W, *A = map(int, open(0).read().split())

print(sum(A) - min(A) * H * W)
```

## [ABC186C - Unlucky 7](https://atcoder.jp/contests/abc186/tasks/abc186_c)

4分で突破. Python は oct 関数で8進数の文字列に変換できるので、それを使うと楽.

```python
N = int(input())

result = 0
for i in range(1, N + 1):
    if oct(i).count('7') + str(i).count('7') == 0:
        result += 1
print(result)
```

## [ABC186D - Sum of difference](https://atcoder.jp/contests/abc186/tasks/abc186_d)

12分半で突破. Aの順序は変えても答えに影響はないので、ソートしてしまえば絶対値が外せる. 後は合計を使って *O*(*N*) に落とせば解ける.

```python
N, *A = map(int, open(0).read().split())

A.sort()
s = sum(A)
result = 0
for i in range(N):
    a = A[i]
    s -= a
    result += s - a * (N - i - 1)
print(result)
```

## [ABC186E - Throne](https://atcoder.jp/contests/abc186/tasks/abc186_e)

突破できず. *O*(1) ないし、*O*(log<i>N</i>) で回数計算する方法が思いつかず.

## [ABC186F -  	Rook on Grid](https://atcoder.jp/contests/abc186/tasks/abc186_f)

突破できず. E問題を完全に捨てて、こっちに注力したら解けたかもしれない.
