# AtCoder Beginner Contest 166 参戦記

## [ABC166A - A?C](https://atcoder.jp/contests/abc166/tasks/abc166_a)

2分半で突破. 書くだけ. コードテストが詰まってて時間がかかった.

```python
S = input()

if S == 'ABC':
    print('ARC')
elif S == 'ARC':
    print('ABC')
```

## [ABC166B - Trick or Treat](https://atcoder.jp/contests/abc166/tasks/abc166_b)

2分半で突破. B問題にしては難しいような? 持ってるか持っていないかをテーブルで管理して、持っている人は持っているとテーブルに書き込んで、最後に持っていない人を集計するだけ.

```python
N, K = map(int, input().split())

t = [0] * N
for _ in range(K):
    d = int(input())
    A = list(map(int, input().split()))
    for a in A:
        t[a - 1] += 1
print(t.count(0))
```

## [ABC166C - Peaks](https://atcoder.jp/contests/abc166/tasks/abc166_c)

6分半で突破. テーブルに隣接の展望台の最高峰を更新していって、最後に隣接の展望台の最高峰と高さを比較するだけ.

```python
N, M = map(int, input().split())
H = list(map(int, input().split()))

t = [0] * N
for _ in range(M):
    A, B = map(int, input().split())
    t[A - 1] = max(t[A - 1], H[B - 1])
    t[B - 1] = max(t[B - 1], H[A - 1])

result = 0
for i in range(N):
    if H[i] > t[i]:
        result += 1
print(result)
```

## [ABC166D - I hate Factorization](https://atcoder.jp/contests/abc166/tasks/abc166_d)

6分半で突破. Aを負にするとBが負のパターンが両方正とだぶるので、Aは0以上でいいはず. 後は 10<sup>3</sup> もあれば、10<sup>15</sup> までカバーできるし、O(2 * 10<sup>6</sup>) だから TLE にもならんだろうで AC. 最初100で出して WA1 orz.

```python
X = int(input())

for A in range(1000):
    for B in range(-1000, 1000):
        if A ** 5 - B ** 5 == X:
            print(A, B)
            exit()
```

## [ABC166E - This Message Will Self-Destruct in 5s](https://atcoder.jp/contests/abc166/tasks/abc166_e)

敗退

追記: 解説PDF通りの実装. 式変形かー.

```python
N = int(input())
A = list(map(int, input().split()))

c1 = {}
c2 = {}

for i in range(N):
    c1.setdefault(i + A[i], 0)
    c1[i + A[i]] += 1
    c2.setdefault(i - A[i], 0)
    c2[i - A[i]] += 1

result = 0
for k in set(c1).intersection(c2):
    result += c1[k] * c2[k]
print(result)
```
