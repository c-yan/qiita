# AtCoder Beginner Contest 154 参戦記

## ABC154A - Remaining Balls

2分半で突破. 変数が5つもあることに一瞬戸惑ったけど、書くだけだった.

```python
S, T = input().split()
A, B = map(int, input().split())
U = input()

if U == S:
    A -= 1
else:
    B -= 1
print(A, B)
```

## ABC154B - I miss you...

1分で突破. 書くだけ.

```python
S = input()

print('x' * len(S))
```

## ABC154C - Distinct or Not

2分で突破. 書くだけ. ABC063B - Varied を思い出した.

```python
N = int(input())
A = list(map(int, input().split()))

if len(set(A)) == N:
    print('YES')
else:
    print('NO')
```

## ABC154D - Dice in Line

10分で突破. リスト p から、平均値のリスト m を作って、幅 K の Sliding Window で最大値を求めるだけ.

```python
N, K = map(int, input().split())
p = list(map(int, input().split()))

m = [(e + 1) / 2 for e in p]

t = sum(m[0:K])
result = t
for i in range(N - K):
    t -= m[i]
    t += m[i + K]
    if t > result:
        result = t
print(result)
```

## ABC154E - Almost Everywhere Zero

敗退. 前に解けなくて放置したアレと同じ問題だなあと思ったが、そのときに解いておかなかったのが運の尽きだった…….
