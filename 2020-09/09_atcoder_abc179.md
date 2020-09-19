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

result = False
t = 0
for _ in range(N):
    D1, D2 = map(int, input().split())
    if D1 == D2:
        t += 1
    else:
        t = 0
    if t >= 3:
        result = True
        break

if result:
    print('Yes')
else:
    print('No')
```

## [ABC179C - A x B + C](https://atcoder.jp/contests/abc179/tasks/abc179_c)

4分で突破. 書くだけ. 数秒くらい計算量大丈夫かなって考えはしたけど、まあC問題だしと腐った考えで.

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

## [ABC179D - Leaping Tak](https://atcoder.jp/contests/abc179/tasks/abc179_d)

突破できず. 50分くらい考えて閃いたけど、5分では実装できません.

## [ABC179E - Sequence Sum](https://atcoder.jp/contests/abc179/tasks/abc179_e)

36分半で突破. ループ検出題は何題か解いているので、そこまで難しいとは思わなかった. 入出力例3がなかなか合わなくてこれだけ時間がかかっていると説得力がないけど(笑).

```python
N, X, M = map(int, input().split())

existence = [0] * M
a = []
A = X
for i in range(N):
    if existence[A] != 0:
        break
    existence[A] = 1
    a.append(A)
    A = A * A % M

for i in range(len(a)):
    if a[i] == A:
        break
j = i

result = sum(a[:j])
a = a[j:]
N -= j
x = N // len(a)
y = N % len(a)
result += sum(a) * x + sum(a[:y])
print(result)
```
