# yukicoder contest 256 参戦記

前回0完なのに今回は5完. 振れ幅激しい!

## [A 1107 三善アクセント](https://yukicoder.me/problems/no/1107)

1-indexed を 0-indexed に読み替える必要はあるが、問題文の通りに判定するだけ.

```python
A = list(map(int, input().split()))

if A[0] < A[1] and A[2] > A[3]:
    print('YES')
else:
    print('NO')
```

## [B 1108 移調](https://yukicoder.me/problems/no/1108)

H ずらした値を出力するだけ.

```python
N, H, *T = map(int, open(0).read().split())

print(*[t + H for t in T])
```

## [C 1109 調の判定](https://yukicoder.me/problems/no/1109)

12の調全てについて、各調に含まれる音しか使われていないか、全部試せばいい.

```python
N, *T = map(int, open(0).read().split())

x = [0, 2, 4, 5, 7, 9, 11]

result = -1
for i in range(12):
    t = set((i + e) % 12 for e in x)
    for e in T:
        if e not in t:
            break
    else:
        if result == -1:
            result = i
        else:
            print(-1)
            exit()
print(result)
```

## [D 1110 好きな歌](https://yukicoder.me/problems/no/1110)

尺取り法でも解けるよなあと思いつつにぶたんで解いた.

```python
from bisect import bisect_left

N, D, *A = map(int, open(0).read().split())

t = sorted(A)

for a in A:
    print(bisect_left(t, a - D))
```

## [E 1111 コード進行](https://yukicoder.me/problems/no/1111)

実装重たいなあと思いつつ、DP で解いた. 最初、複雑度の合計がKを超えても計算してて TLE を食らった. PyPy じゃないと TLE になります.

```python
N, M, K = map(int, input().split())

m = 1000000007

pqc = [[] * 301 for _ in range(301)]
for _ in range(M):
    P, Q, C = map(int, input().split())
    pqc[P].append((Q, C))

t = [{0: 1} for _ in range(301)]
for i in range(N - 1):
    nt = [{} for _ in range(301)]
    for j in range(1, 301):
        for k in t[j]:
            for q, c in pqc[j]:
                v = k + c
                if v <= K:
                    nt[q].setdefault(v, 0)
                    nt[q][v] += t[j][k]
                    nt[q][v] %= m
    t = nt

result = 0
for i in range(1, 301):
    t[i].setdefault(K, 0)
    result += t[i][K]
    result %= m
print(result)
```
