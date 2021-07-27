# AtCoder Beginner Contest 203 (Sponsored by Panasonic) 参戦記

## [ABC203A - Chinchirorin](https://atcoder.jp/contests/abc203/tasks/abc203_a)

3分で突破. 書くだけ.

```python
a, b, c = map(int, input().split())

x = [a, b, c]
x.sort()

if x[0] == x[1]:
    print(x[2])
elif x[1] == x[2]:
    print(x[0])
else:
    print(0)
```

## [ABC203B - AtCoder Condominium](https://atcoder.jp/contests/abc203/tasks/abc203_b)

2分半で突破. ループを回さなくても一発で求めれるような気もしたけど、書くだけだからいいやとなった.

```python
N, K = map(int, open(0).read().split())

result = 0
for i in range(N):
    for j in range(K):
        result += (i + 1) * 100 + (j + 1)
print(result)
```

## [ABC203C - Friends and Travel costs](https://atcoder.jp/contests/abc203/tasks/abc203_c)

6分半で突破. *N*≤2×10<sup>5</sup> なので *O*(<i>N</i>log<i>N</i>) くらいまで大丈夫だなと思ったら、ソートして前から順に処理していけばいいよねというのが分かる.

```python
from sys import stdin

readline = stdin.readline

N, K = map(int, readline().split())
AB = [list(map(int, readline().split())) for _ in range(N)]

AB.sort()
p = 0
result = 0
while K != 0:
    result += K
    K = 0
    while p < N and AB[p][0] <= result:
        K += AB[p][1]
        p += 1
print(result)
```

## [ABC203D - Pond](https://atcoder.jp/contests/abc203/tasks/abc203_d)

突破できず. にぶたんかなと思って実装したら TLE. 方針が間違っていたのか、実装がいまいちなのか.
