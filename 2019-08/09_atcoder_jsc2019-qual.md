# 第一回日本最強プログラマー学生選手権-予選- 参戦記

## A - Takahashi Calendar

8分で突破. 全組み合わせチェックするだけ. 一応0から始めずにループ量を減らしたけど、減らす必要はなかったと思う.

```python
m, d = map(int, input().split())
result = 0
for i in range(4, m + 1):
  for j in range(22, d + 1):
    d10 = j // 10
    d1 = j % 10
    if d1 >= 2 and d10 >= 2 and d1 * d10 == i:
      result += 1
print(result)
```

## B - Kleene Inversion

30分で突破. 時間かけすぎて、順位ヤバそうと思ったけど、他の人も時間がかかってたようでセーフ. 入力例3を K=2 で手で解いてみてようやく1ループ目とそれ以降で違うことが認識されてスパッと解けた. 最後に一回だけ 10<sup>9</sup> + 7 の mod を取れば良いのは Python でちょっと得した感,

```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
v1 = 0
for i in range(n - 1):
  for j in range(i + 1, n):
    if a[i] > a[j]:
      v1 += 1
v2 = 0
for i in range(n):
  for j in range(n):
    if a[i] > a[j]:
      v2 += 1

t = v1 * k + v2 * k * (k - 1) // 2
print(t % (10 ** 9 + 7))
```
