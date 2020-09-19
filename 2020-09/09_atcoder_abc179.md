# AtCoder Beginner Contest 179 参戦記

## [ABC179A - Plural Form](https://atcoder.jp/contests/abc179/tasks/abc179_a)

1分半で突破. 書くだけ.

```python
S = input()

if S[-1] != 's':
    print(S + 's')
else:
    print(S + 'es')
```

## [ABC179B - Go to Jail](https://atcoder.jp/contests/abc179/tasks/abc179_b)

1分半で突破. 書くだけ. 問題名はなんで「牢屋に行く」なんすかね.

```python
N = int(input())

t = 0
max_run_length = 0
for _ in range(N):
    D1, D2 = map(int, input().split())
    if D1 == D2:
        t += 1
    else:
        t = 0
    max_run_length = max(max_run_length, t)

if max_run_length >= 3:
    print('Yes')
else:
    print('No')
```

## [ABC179C - A x B + C](https://atcoder.jp/contests/abc179/tasks/abc179_c)

4分で突破. 書くだけ. 数秒くらい計算量大丈夫かなって考えはしたけど、まあC問題だし大丈夫だろうと腐った考えで PyPy で提出して AC.

```python
N = int(input())

result = 0
for A in range(1, N + 1):
    for B in range(1, (N // A) + 1):
        C = N - A * B
        if C == 0:
            continue
        result += 1
print(result)
```

A×B+1≦N なら良いので、式変形して B≦(N-1)÷A となり、B の値域が一発で求まる.

```python
N = int(input())

result = 0
for A in range(1, N + 1):
    result += (N - 1) // A
print(result)
```

## [ABC179D - Leaping Tak](https://atcoder.jp/contests/abc179/tasks/abc179_d)

突破できず. 50分くらい考えて閃いたけど、5分では実装できません.

## [ABC179E - Sequence Sum](https://atcoder.jp/contests/abc179/tasks/abc179_e)

36分半で突破. ループ検出題は何題か解いているので、そこまで難しいとは思わなかった. 入出力例3がなかなか合わなくてこれだけ時間がかかっていると説得力がないけど(笑).

```python
N, X, M = map(int, input().split())

existence = [False] * M
a = []
A = X
for i in range(N):
    if existence[A]:
        break
    existence[A] = True
    a.append(A)
    A = A * A % M

for i in range(len(a)):
    if a[i] == A:
        break
loop_start = i

result = sum(a[:loop_start])
a = a[loop_start:]
N -= loop_start
loops = N // len(a)
remainder = N % len(a)
result += sum(a) * loops + sum(a[:remainder])
print(result)
```
