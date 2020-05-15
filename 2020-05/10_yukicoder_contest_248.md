# yukicoder contest 248 参戦記

## [A 1052 電子機器X](https://yukicoder.me/problems/no/1052)

29分で突破. N=10,11 について K=1～8 まで手で求めたらようやく分かりました(笑). N が偶数のときは、Kが奇数のときは偶数、Kが偶数のときは奇数しかでてこないので、最大が N / 2 になります. N が奇数のときは、偶数奇数両方とも出てくるので最大 N になります.

```python
N, K = map(int, input().split())

if N % 2 == 0:
    print(min(K + 1, N // 2))
else:
    print(min(K + 1, N))
```

## [B 1053 ゲーミング棒](https://yukicoder.me/problems/no/1053)

突破できず. WA 2個まで AC した後、3つに分かれているやつが一つでもあれば無理というのが抜けたまま、他に条件を満たすことが出来るパターンがあるのかと悩んで終了(笑). 条件を満たせることが出来るパターンは以下の2つ.

1. 色が最初から分かれていない (回数は0回)
2. 2つに分かれているのが1つだけあり、それが先頭と末尾 (回数は1回)

```python
N = int(input())
A = list(map(int, input().split()))

d = {}
prev = -1
for a in A:
    if prev != a:
        d.setdefault(a, 0)
        d[a] += 1
    prev = a

if all(v == 1 for v in d.values()):
    print(0)
    exit()

if any(v >= 3 for v in d.values()):
    print(-1)
    exit()

if len([v for v in d.values() if v == 2]) == 1 and A[0] == A[-1]:
    print(1)
else:
    print(-1)
```

## [C 1054 Union add query](https://yukicoder.me/problems/no/1054)

突破できず. [ABC138D - Ki](https://atcoder.jp/contests/abc138/tasks/abc138_d) みたいにまとめて足せるようにするんだろうなあとは思ったが、残り時間的に実装は無理で…….
