# AtCoder Beginner Contest 252 参戦記

## [ABC252A - ASCII code](https://atcoder.jp/contests/abc252/tasks/abc252_a)

1分で突破. 書くだけ.

```python
N = int(input())

print(chr(N))
```

## [ABC252B - Takahashi's Failure](https://atcoder.jp/contests/abc252/tasks/abc252_b)

4分半で突破. 書くだけ.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

x = max(A)
if len(set(B) & set(i + 1 for i in range(N) if A[i] == x)) > 0:
    print('Yes')
else:
    print('No')
```

## [ABC252C - Slot Strategy](https://atcoder.jp/contests/abc252/tasks/abc252_c)

35分で突破. 「ここで、$S_i$​ は $0, 1, \ldots , 9$ がちょうど $1$ 回ずつ現れる長さ $10$ の文字列です。」を読み飛ばして、揃わない数字もある難しい問題に勝手にして自滅. シミュレートを `heapq` で高速化して解いたけど、高速化しなくても問題なかった模様.

```python
from heapq import heappush, heappop

N = int(input())
S = [[int(c) for c in input()] for _ in range(N)]

result = 10 ** 15
for i in range(10):
    q = []
    for j in range(N):
        heappush(q, S[j].index(i))
    used = set()
    while len(q) != 0:
        x = heappop(q)
        if x in used:
            heappush(q, x + 10)
        else:
            used.add(x)
            t = x
    result = min(result, t)
print(result)
```

冷静になって考えればシミュレートする必要もなくダブった分だけ10を積み増せばいいと分かる.

```python
N = int(input())
S = [[int(c) for c in input()] for _ in range(N)]

result = 10 ** 15
for i in range(10):
    t = {}
    for j in range(N):
        x = S[j].index(i)
        t.setdefault(x, 0)
        t[x] += 1
    result = min(result, max(k + (t[k] - 1) * 10 for k in t))
print(result)
```

## [ABC252D - Distinct Trio](https://atcoder.jp/contests/abc252/tasks/abc252_d)

7分半で突破. 過去に似たような問題を解いたような. 組み合わせの問題なのでソートしても大丈夫. ソートさえすれば、真ん中に対し、左右がどこまで移動できるかがにぶたんで $O(\log N)$ で取れるので、$O(N \log N )$ で解ける.

```python
from bisect import bisect_left, bisect_right

N, *A = map(int, open(0).read().split())

A.sort()

result = 0
for j in range(1, N - 1):
    i = bisect_right(A, A[j] - 1)
    k = bisect_left(A, A[j] + 1)
    result += i * (N - k)
print(result)
```
