# AtCoder Beginner Contest 160 参戦記

## ABC160A - Coffee

1分半で突破. 書くだけ.

```python
S = input()

if S[2] == S[3] and S[4] == S[5]:
    print('Yes')
else:
    print('No')
```

## ABC160B - Golden Coins

2分半で突破. 500円のほうがコスパがいいので、500円を可能なだけ、余りを5円で.

```python
X = int(input())

result = (X // 500) * 1000
X -= (X // 500) * 500
result += (X // 5) * 5
print(result)
```

## ABC160C - Traveling Salesman around Lake

7分で突破. ジャッジが詰まっててコードテストがなかなか実行されなくて参った. N軒全て回るということは、移動開始地点と移動終了地点の間だけ歩かなくていいということなので、それが最大のところを探せばいい.

```python
K, N = map(int, input().split())
A = list(map(int, input().split()))

result = A[0] - A[N - 1] + K
for i in range(N - 1):
    result = max(result, A[i + 1] - A[i])
print(K - result)
```

## ABC160D - Line++

解かずにEに行ってしまったがこっちを解くべきだったろうか.

## ABC160E - Red and Green Apples

敗退. ソートして、累積和して、にぶたんすればいいかと思ったが、WA 5個が消せず. 無念.
