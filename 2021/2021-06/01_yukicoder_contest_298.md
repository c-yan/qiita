# yukicoder contest 298 不参戦記

寝落ちして起きたら終わってた(笑).

## [A 1527 Input Constant is Good](https://yukicoder.me/problems/no/1527)

AとBを比較するだけです.

```python
A, B = map(int, input().split())

if A <= B:
    print('Yes')
else:
    print('No')
```

## [B 1528 Not 1](https://yukicoder.me/problems/no/1528)

ざっくりで考えると、2, 4, 6, 8, 10, ..., N で良いのかなと思いつくので、それでは上手く行かないパターンを考える. まず、N が奇数だと1つ足りない. それ以外に入れるなら3かなと思うと、3との行き来できる6が隣接する必要があるので、2, 4, 8, 10, ..., N-1, 6, 3 で良いんじゃないかと思い付く. 後はこれらを適用できない N=1, 2, 3, 4, 5 を別途処理して完成.

```python
N = int(input())

if N in [1, 2]:
    print(1)
elif N in [3, 5]:
    print(-1)
elif N == 4:
    print(2, 4)
else:
    t = [2, 4]
    for i in range(8, N + 1, 2):
        t.append(i)
    t.append(6)
    if N % 2 == 1:
        t.append(3)
    print(*t)
```
