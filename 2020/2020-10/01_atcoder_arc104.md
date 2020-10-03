# AtCoder Regular Contest 104 参戦記

C, D, E を眺めて D が一番簡単かなと思って考え始めて、途中で勘違いに気づいてどれも解けないやんってなった. 速解きチャレンジ失敗していて、1900位くらいになってしまったのでパフォが怖い.

## [ABC179A - Plus Minus](https://atcoder.jp/contests/arc104/tasks/arc104_a)

3分で突破. どれくらいの範囲をチェックすればいいんだろうと思いつつ.

```python
A, B = map(int, input().split())

for X in range(-200, 200):
    for Y in range(-200, 200):
        if X + Y == A and X - Y == B:
            print(X, Y)
            exit()
```

今思うと方程式を解いて、(A+B)÷2 と (A-B)÷2 でよかった.

```python
A, B = map(int, input().split())

print((A + B) // 2, (A - B) // 2)
```

## [ABC179B - DNA Sequence](https://atcoder.jp/contests/arc104/tasks/arc104_b)

14分で突破. SegmentTree かなと思ったあとで、累積和だなと思い直したのが無駄に時間がかかった原因かなって. Python だと割と制限時間ギリギリ.

```python
from itertools import accumulate


def main():
    N, S = input().split()
    N = int(N)

    a = [0] * (N + 1)
    g = [0] * (N + 1)
    c = [0] * (N + 1)
    t = [0] * (N + 1)

    for i in range(N):
        x = S[i]
        if x == 'A':
            a[i + 1] = 1
        elif x == 'G':
            g[i + 1] = 1
        elif x == 'C':
            c[i + 1] = 1
        elif x == 'T':
            t[i + 1] = 1

    a = list(accumulate(a))
    g = list(accumulate(g))
    c = list(accumulate(c))
    t = list(accumulate(t))

    result = 0
    for i in range(N):
        for j in range(i + 2, N + 1, 2):
            k = a[j] - a[i]
            l = g[j] - g[i]
            m = c[j] - c[i]
            n = t[j] - t[i]
            if k == n and l == m:
                result += 1
    print(result)


main()
```
