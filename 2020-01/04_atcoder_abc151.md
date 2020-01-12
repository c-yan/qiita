# AtCoder Beginner Contest 151 参戦記

## ABC151A - Next Alphabet

1分半で突破. 書くだけ.

```python
C = input()

print(chr(ord(C[0]) + 1))
```

## ABC151B - Achieve the Goal

4分で突破. 書くだけ.

```python
N, K, M = map(int, input().split())
A = list(map(int, input().split()))

t = sum(A)
if N * M > t + K:
    print(-1)
else:
    print(max(N * M - t, 0))
```

## ABC151C - Count Order

7分半で突破. AC 後の WA は無視しないといけないところだけを気にすればよい.

```python
N, M = map(int, input().split())

ac = [False] * N
wa = [0] * N
for _ in range(M):
    p, S = input().split()
    p = int(p) - 1
    if S == 'AC':
        ac[p] = True
    else:
        if not ac[p]:
            wa[p] += 1

a = 0
b = 0
for i in range(N):
    if ac[i]:
        a += 1
        b += wa[i]
print(*[a, b])
```

## ABC151D - Maze Master

17分で突破. 見た瞬間に AtCoder Typical Contest 002A - 幅優先探索 を思い出したので、全箇所から幅優先探索して、最長をかき集める方向性で解いた.

```python
H, W = map(int, input().split())
S = [input() for _ in range(H)]


def f(i, j):
    t = [[-1] * W for _ in range(H)]
    t[i][j] = 0
    q = [(i, j)]
    while q:
        y, x = q.pop(0)
        if y - 1 >= 0 and S[y - 1][x] != '#' and t[y - 1][x] == -1:
            t[y - 1][x] = t[y][x] + 1
            q.append((y - 1, x))
        if y + 1 < H and S[y + 1][x] != '#' and t[y + 1][x] == -1:
            t[y + 1][x] = t[y][x] + 1
            q.append((y + 1, x))
        if x - 1 >= 0 and S[y][x - 1] != '#' and t[y][x - 1] == -1:
            t[y][x - 1] = t[y][x] + 1
            q.append((y, x - 1))
        if x + 1 < W and S[y][x + 1] != '#' and t[y][x + 1] == -1:
            t[y][x + 1] = t[y][x] + 1
            q.append((y, x + 1))
    return max(max(tt) for tt in t)


result = 0
for i in range(H):
    for j in range(W):
        if S[i][j] != '#':
            result = max(result, f(i, j))
print(result)
```

## ABC151E - Max-Min Sums

糸口すらつかめず敗退. 70分の間、順位が800番台から1300番台までずり落ちるのを眺める羽目になった.
