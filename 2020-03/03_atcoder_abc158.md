# AtCoder Beginner Contest 158 参戦記

## [ABC158A - Station and Bus](https://atcoder.jp/contests/abc158/tasks/abc158_a)

2分で突破. 書くだけ. 1種類か、2種類か、それだけ.

```python
S = input()

if len(set(S)) == 1:
    print('No')
else:
    print('Yes')
```

## [ABC158B - Count Balls](https://atcoder.jp/contests/abc158/tasks/abc158_b)

3分半で突破. N が操作何回分を含むかを計算. n 回であれば n * A 個青いボールがある. 途中かけになった分については、A個以内であればその個数が、そうでなければA個青いボールがある.

```python
N, A, B = map(int, input().split())

result = 0
result += N // (A + B) * A
result += min(N % (A + B), A)
print(result)
```

## [ABC158C - Tax Increase](https://atcoder.jp/contests/abc158/tasks/abc158_c)

4分半で突破. A≤B≤100 ってことは、たかだか1000くらいが答えになるので総当りで OK! もっとクレバーにやることもできるだろうけど、ここ2回くらいそれで WA を食らって懲りたのである. クレバーより愚直に安全.

```python
A, B = map(int, input().split())

for i in range(2000):
    if int(i * 0.08) == A and int(i * 0.1) == B:
        print(i)
        exit()
print(-1)
```

追記: 解説 PDF で O(1) で解く課題が出てたので.

```python
A, B = map(int, input().split())

x_low = (A * 100 + 7) // 8
x_high = ((A + 1) * 100 + 7) // 8 - 1
y_low = (B * 100 + 9) // 10
y_high = ((B + 1) * 100 + 9) // 10 - 1

result = max(x_low, y_low)
if result > min(x_high, y_high):
    result = -1
print(result)
```

## [ABC158D - String Formation](https://atcoder.jp/contests/abc158/tasks/abc158_d)

17分で突破. もちろん言われたとおりに文字列操作をしたら TLE まっしぐら. 右からも左からも足し込むので deque を使うのは見え見え. 後は反転をいちいちしていると TLE になるので、必要であれば最後に1回するだけにつじつまを合わせるだけ.

```python
from collections import deque

S = input()
Q = int(input())

t = deque(S)
reverse = False
for _ in range(Q):
    query = input()
    if query[0] == '1':
        reverse = not reverse
    elif query[0] == '2':
        _, F, C = query.split()
        if F == '1':
            if reverse:
                t.append(C)
            else:
                t.appendleft(C)
        elif F == '2':
            if reverse:
                t.appendleft(C)
            else:
                t.append(C)
result = ''.join(t)
if reverse:
    result = result[::-1]
print(result)
```

## [ABC158E - Divisible Substring](https://atcoder.jp/contests/abc158/tasks/abc158_e)

敗退. O(N<sup>2</sup>) から削る手段が全く思いつかず. と、コンテスト中に書いたが、終了5分前に桁ごとに余りとその組み合わせ数を引き回して1桁づつ進めれば DP で解けるんじゃないかと気づいたが時すでに遅かった.

追記: P が2と5のときは簡単で、一番下の桁が2の倍数、5の倍数のときに割りきれるので、後は左に伸ばせる数を積算すれば求まる. その他の P については、例えば S<sub>6</sub>S<sub>5</sub>S<sub>4</sub>S<sub>3</sub>S<sub>2</sub>S<sub>1</sub>S<sub>0</sub> の S<sub>6</sub>S<sub>5</sub>S<sub>4</sub>S<sub>3</sub> が P で割り切れたとすると、S<sub>6</sub>S<sub>5</sub>S<sub>4</sub>S<sub>3</sub>S<sub>2</sub>S<sub>1</sub>S<sub>0</sub> ≡ S<sub>2</sub>S<sub>1</sub>S<sub>0</sub> (mod P) なので、余りを記録しながら左に伸ばして、同じ余りのものの個数を積算すれば求まる.

```python
N, P = map(int, input().split())
S = input()

S = S[::-1]
result = 0
if P == 2 or P == 5:
    for i in range(N):
        if int(S[i]) % P == 0:
            result += N - i
else:
    t = [0] * P
    m = 1
    n = 0
    for i in range(len(S)):
        t[n] += 1
        n += int(S[i]) * m
        n %= P
        result += t[n]
        m *= 10
        m %= P
print(result)
```
