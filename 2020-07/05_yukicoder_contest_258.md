# yukicoder contest 258 参戦記

## [A 1119 Division 3](https://yukicoder.me/problems/no/1119)

Python の場合、X×Y×Z を素直に計算して3で割れるか確認すればいいです. 言語によっては X,Y,Z≤10<sup>9</sup> の制限から 64bit の範囲を超えて死にそうです.

```python
X, Y, Z = map(int, open(0).read().split())

if X * Y * Z % 3 == 0:
    print('Yes')
else:
    print('No')
```

## [B 1120 Strange Teacher](https://yukicoder.me/problems/no/1120)

N = 2 の場合は簡単. N > 2 の場合、合計が N - 2 づつ減っていくので、「操作回数の最小値を求めよ。」といいつつ、操作回数は (sum(A) - sum(B)) / (N - 2) 固定となる. あとはその操作回数で B が実現できるかを考えるだけ. A<sub>i</sub> と B<sub>i</sub> の差の絶対値が操作回数より大きい場合はおかしい. +1 操作を適用した場合、その回数の2倍だけ B<sub>i</sub> が増えるので、B<sub>i</sub> と A<sub>i</sub> から操作回数を引いた数の差が2の倍数じゃないとおかしい. これら2つのありえないパターンを弾いたら AC した.

```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

if N == 2:
    if sum(A) == sum(B):
        print(abs(A[0] - B[0]))
    else:
        print(-1)
elif N > 2:
    if sum(B) > sum(A):
        print(-1)
    elif (sum(A) - sum(B)) % (N - 2) == 0:
        t = (sum(A) - sum(B)) // (N - 2)
        for i in range(N):
            if abs(A[i] - B[i]) > t or (B[i] - (A[i] - t)) % 2 == 1:
                print(-1)
                break
        else:
            print(t)
    else:
        print(-1)
```
