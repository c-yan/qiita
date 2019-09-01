# AtCoder Beginner Contest 139 参戦記

## A - Tenki

2分半で突破. 書くだけ.

```python
s = input()
t = input()
print(sum(1 for i in range(3) if s[i] == t[i]))
```

## B - Power Socket

9分半で突破. 時間かかりすぎな上に、WA1.
最初は2行で書いてたんだけど、入出力例3でコケて、日和って while 文が登場しました.
そして1個も買わないことは想定してなかったので WA を食らう.

```python
a, b = map(int, input().split())
n = 0
t = 1
while t < b:
  t += a - 1
  n += 1
print(n)
```

終わった後に書いた while 無し版

```python
a, b = map(int, input().split())
print(((b - 1) + (a - 2)) // (a - 1))
```

## C - Lower

4分半で突破. 書くだけ. 最近、C問題が簡単な気がする.

```python
n = int(input())
h = list(map(int, input().split()))
t = 0
result = 0
for i in range(n - 1):
  if h[i + 1] <= h[i]:
    t += 1
  else:
    result = max(t, result)
    t = 0
result = max(t, result)
print(result)
```

## D - ModSum

5分で突破. n = 3 と n = 4 を手でやって、あまりの簡単さに困惑した. 前回のDも簡単だったけど、今回のは輪をかけて簡単でおかしい.

```python
n = int(input())
print((n + 1) * n // 2 - n)
```

これ、どう考えても

```python
n = int(input())
print(n * (n - 1) // 2)
```

でいいのにそうなっていないのは、1～n の合計を `(n + 1) * n // 2` として覚えていて、1～n-1 の合計の式に自信がなかった日和である. (今回日和が多いですね)

## E - League

TLE 1個を消せずに敗退.

## F - Engines

ナイーブに書いたら TLE 食らいまくって、計算量減らしたら後半半分くらい WA になって敗退.
