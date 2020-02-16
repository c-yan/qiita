# AtCoder Beginner Contest 155 参戦記

## ABC155A - Poor

2分で突破. 書くだけ. 前回に引き続き、set での重複判定.

```python
ABC = list(map(int, input().split()))

if len(set(ABC)) == 2:
    print('Yes')
else:
    print('No')
```

## ABC155B - Papers, Please

2分半で突破. 書くだけ.

```python
N = int(input())
A = list(map(int, input().split()))

for a in A:
    if a % 2 == 1:
        continue
    if a % 3 == 0 or a % 5 == 0:
        continue
    print('DENIED')
    exit()
print('APPROVED')
```

## ABC155C - Poll

8分半で突破. 書くだけ……といいつつそれなりに時間がかかったけど(汗). C# 使いが壊滅状態と聞いて、AC した人のコードを眺めると 全員 string 配列のソートに自前の comparer を使っていたので、Mono は string の比較がヤバイのかなと思った.

```python
N = int(input())

d1 = {}
for _ in range(N):
    S = input()
    if S in d1:
        d1[S] += 1
    else:
        d1[S] = 1

d2 = {}
for key in d1:
    value = d1[key]
    if value in d2:
        d2[value].append(key)
    else:
        d2[value] = [key]

key = sorted(d2.keys(), reverse=True)[0]
for s in sorted(d2[key]):
    print(s)
```

## ABC155D - Pairs

敗退. Eの方が解いてる人が多いので、Eに行った. にぶたんかなあと思った.

## ABC155E - Payment

敗退. 入力例1, 入力例2 は突破したものの、入力例3が243ではなく、249になってしまって、自分のロジックでうまく行かないパターンを考えていたけど思いつかなかった.
