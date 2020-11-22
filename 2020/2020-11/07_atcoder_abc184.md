# AtCoder Beginner Contest 184 参戦記

ABC でC問題解けなかったの初めて…….

## [ABC184A - Determinant](https://atcoder.jp/contests/abc184/tasks/abc184_a)

1分で突破. 書くだけ.

```python
a, b = map(int, input().split())
c, d = map(int, input().split())

print(a * d - b * c)
```

## [ABC184B - Quizzes](https://atcoder.jp/contests/abc184/tasks/abc184_b)

3分で突破. 書くだけ.

```python
N, X = map(int, input().split())
S = input()

result = X
for c in S:
    if c == 'o':
        result += 1
    elif c == 'x':
        if result != 0:
            result -= 1
print(result)
```

## [ABC184C - Super Ryuma](https://atcoder.jp/contests/abc184/tasks/abc184_c)

突破できず. 頭真っ白になりました.

## [ABC184D - increment of coins](https://atcoder.jp/contests/abc184/tasks/abc184_d)

突破できず. 入出力例でも TLE したり、NaN になったりと上手く行かず.

## [ABC184E - Third Avenue](https://atcoder.jp/contests/abc184/tasks/abc184_e)

CとDを諦めてから始めたので何分かかったのか不明. 56分未満なのは確か. 見るからに難しそうなところが何もなく、何か罠があるのかと思いながら実装したけど、何も罠はなかった(笑). ワープだけは繰り返しトライするとヤバそうだったので、一回しかトライしないようにしたけど、これは見え見えすぎて罠ではないな(笑).

```python
from collections import deque

INF = 10 ** 9

H, W = map(int, input().split())
a = [input() for _ in range(H)]

d = {}
for h in range(H):
    for w in range(W):
        c = a[h][w]
        if c in 'SG':
            d[c] = (h, w)
        elif c in '.#':
            continue
        else:
            if c in d:
                d[c].append((h, w))
            else:
                d[c] = [(h, w)]

not_warped = {}
for c in 'abcdefghijklmnopqrstuvwxyz':
    not_warped[c] = True


def move(h, w, p):
    c = a[h][w]
    if c == '#':
        return
    if t[h][w] > p:
        t[h][w] = p
        q.append((h, w))


t = [[INF] * W for _ in range(H)]
h, w = d['S']
t[h][w] = 0
q = deque([(h, w)])
while q:
    h, w = q.popleft()
    c = a[h][w]
    p = t[h][w] + 1
    if 'a' <= c <= 'z' and not_warped[c]:
        for nh, nw in d[c]:
            if t[nh][nw] > p:
                t[nh][nw] = p
                q.append((nh, nw))
        not_warped[c] = False
    if h != 0:
        move(h - 1, w, p)
    if h != H - 1:
        move(h + 1, w, p)
    if w != 0:
        move(h, w - 1, p)
    if w != W - 1:
        move(h, w + 1, p)

h, w = d['G']
if t[h][w] == INF:
    print(-1)
else:
    print(t[h][w])
```
