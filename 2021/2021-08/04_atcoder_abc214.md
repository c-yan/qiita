# AtCoder Beginner Contest 214 参戦記

過去最悪の順位をかろうじて更新しなくてすんだ程度の大爆死……….

## [ABC214A - New Generation ABC](https://atcoder.jp/contests/abc214/tasks/abc214_a)

1分半で突破. 書くだけ.

```python
N = int(input())

if N <= 125:
    print(4)
elif N <= 211:
    print(6)
else:
    print(8)
```

## [ABC214B - How many?](https://atcoder.jp/contests/abc214/tasks/abc214_b)

3分半で突破. 素直に3重ループしても *O*(10<sup>6</sup>) 程度で TLE しないので、素直にやれば良い.

```python
S, T = map(int, input().split())

result = 0
for a in range(101):
    for b in range(101):
        for c in range(101):
            if a + b + c > S:
                break
            if a * b * c > T:
                break
            result += 1
print(result)
```

## [ABC214C - Distribution](https://atcoder.jp/contests/abc214/tasks/abc214_c)

51分半で突破. TLE4、WA2. i番目のすぬけ君が初めて宝石をもらう時刻が最小となるスタート地点は1からNのどれか. 受け渡しリレーを2周すれば全てのすぬけくんに可能性があるスタート地点が必ず全て試されるので、2周回せば良い. 冷静に考えれば難しくなくて絶望.

```python
N = int(input())
S = list(map(int, input().split()))
T = list(map(int, input().split()))

result = T[:]
for _ in range(2):
    for i in range(N):
        j = (i + 1) % N
        x = result[i] + S[i]
        if result[j] > x:
            result[j] = x
print(*result, sep='\n')
```
