# AtCoder Beginner Contest 140 参戦記

## ABC140A - Password

1分で突破. 書くだけ.

```python
N = int(input())
print(N ** 3)
```

## ABC140B - Buffet

6分半で突破. 1-indexed と 0-indexed を調整するのがめんどくさいだけ.
最初 before に -1 を入れていたら、料理1始まりの時に誤爆して危なかった. 入出力例3に救われた.

```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))
C = list(map(int, input().split()))
before = N
result = 0
for i in range(N):
  result += B[A[i] - 1]
  if before + 1 == A[i] - 1:
    result += C[before]
  before = A[i] - 1
print(result)
```

## ABC140C - Maximal Value

7分で突破. なんかややこしいけど手で解けばすぐ min(B<sub>i</sub>, B<sub>i-1</sub>) なのは分かるので、端だけ別処理して終わり.

```python
N = int(input())
B = list(map(int, input().split()))
result = B[0] + B[N - 2]
for i in range(1, N - 1):
  result += min(B[i - 1], B[i])
print(result)
```

## ABC140D - Face Produces Unhappiness

敗退. どこをひっくり返しても増える幸福は2ということが見抜けず、一番長いやつと一番長いやつを繋げることに奔走してしまった…….  結果、TLE だらけ.

終了後に AC を取ったコードは以下.

```python
N, K = map(int, input().split())
S = input()
c = 0
for i in range(1, N):
  if S[i] == S[i - 1]:
    c += 1
print(min(N - 1, c + K * 2))
```

## ABC140E - Second Sum

敗退. ナイーブに正しい値を求めるコードは書けたけど、10<sup>5</sup> * 10<sup>5</sup> の計算量では TLE 死.
