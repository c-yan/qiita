# AtCoder 日立製作所 社会システム事業部 プログラミングコンテスト2020 参戦記

## hitachi2020A - Hitachi String

3分で突破. どう書くのが楽か戸惑ったけど、長さが10までなので、ありえる解答総当りが一番速いかとこうなった.

```python
S = input()

for i in range(1, 6):
    if S == 'hi' * i:
        print('Yes')
        exit()
print('No')
```

## hitachi2020B - Nice Shopping

8分で突破. B問題だから総当たりでいいかと思ったら、制限を見るに無理. ちょっと考えたけど、割引券が無ければ最小金額は min(a) + min(b) に決まってるので、後は割引券だけ総当りすればいいやとこうなった.

```python
A, B, M = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

result = min(a) + min(b)
for _ in range(M):
    x, y, c = map(int, input().split())
    t = a[x - 1] + b[y - 1] - c
    if t < result:
        result = t
print(result)
```

## hitachi2020C - ThREE

敗退. 書くだけは書いたけど、7割型 TLE な上に WA も食らったので残り1時間くらい残して心が折れました(笑).
