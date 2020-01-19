# AtCoder キーエンス プログラミング コンテスト 2020 参戦記

## A - Painting

9分半で突破. 縦、横長い方で何回塗ったら超えるか、それだけ. 最初勘違いして交互に塗っていくと思ったせいで時間がかかってしまって順位どん底に…….

```python
H = int(input())
W = int(input())
N = int(input())

t = max(H, W)
print((N + t - 1) // t)
```

## C - Subarray Sum

13分?で突破. S を K 個、どう足しても S にならないものを N - K 個並べればいいだけ. 回答の数列の各値は 1～10<sup>9</sup> であることには注意.

```python
N, K, S = map(int, input().split())

result = [S % 10 ** 9 + 1] * N
for i in range(K):
    result[i] = S
print(*result)
```

## B - Robot Arms

敗退.

追記: 解説を Python のコードに落として AC. 自分が書いた WA 食らったコードと違う部分は x でソートするか、x + l でソートするか、それだけだった…….

```python
N = int(input())
XL = [list(map(int, input().split())) for _ in range(N)]

t = [(x + l, x - l) for x, l in XL]
t.sort()
max_r = -float('inf')
result = 0
for i in range(N):
    r, l = t[i]
    if max_r <= l:
        result += 1
        max_r = r
print(result)
```
