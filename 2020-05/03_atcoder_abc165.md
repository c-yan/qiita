# AtCoder Beginner Contest 165 参戦記

突然10分遅れになって驚いた. なんかラッキーでC問題、D問題が解けた感がある. まあ、解けた問題を逃してしまったと地団駄することも多いのでたまには良いでしょう.

## [ABC165A - We Love Golf](https://atcoder.jp/contests/abc165/tasks/abc165_a)

2分半で突破. O(1) で書けるんだろうなと思いつつも、ループを回すチキン.

```python
K = int(input())
A, B = map(int, input().split())

for i in range(A, B + 1):
    if i % K == 0:
        print('OK')
        exit()
print('NG')
```

## [ABC165B - 1%](https://atcoder.jp/contests/abc165/tasks/abc165_b)

4分で突破. X≤10<sup>18</sup> を見て、B問題なのに TLE ありうる!? と思ったけど、コードテストで 10<sup>18</sup> を食わせても全然平気だったので、なーんだと思いつつ AC.

```python
X = int(input())

t = 100
result = 0
while t >= X:
    t += t // 100
    result += 1
print(result)
```

## [ABC165C - Many Requirements](https://atcoder.jp/contests/abc165/tasks/abc165_c)

23分半で突破. 問題見た瞬間に頭が真っ白になったけど、なんとか解けた. AC した瞬間にこれ現在順位すごいんじゃねと思ったけど、みんな先にD問題を解いててそこまででもなかった(笑).

まあ、総当たりをすれば良いのかなあと思ったけど、妥当でないものも含めて生成すると O(10<sup>10</sup>) でダメそう. 1≤M≤10 に注目して、M - 1 は0～9なので文字列で表現できるじゃんと、文字列で妥当なものだけを全組生成することに. 後は総当たりで. TLE するかなと思ったけど、せずに AC.

```python
N, M, Q = map(int, input().split())
abcd = [list(map(int, input().split())) for _ in range(Q)]

m = M - 1


def f(s):
    if len(s) == N:
        return [s]
    result = []
    if s == '':
        r = 0
    else:
        r = int(s[-1])
    for i in range(r, m + 1):
        result.extend(f(s + str(i)))
    return result


result = 0
V = f('')
for s in V:
    A = [int(c) + 1 for c in s]
    t = 0
    for a, b, c, d in abcd:
        if A[b - 1] - A[a - 1] == c:
            t += d
    result = max(result, t)
print(result)
```

## [ABC165D - Floor Function](https://atcoder.jp/contests/abc165/tasks/abc165_d)

20分で突破. Ax/B が増えるタイミングに最大値があるはずと思ったので、Ax/B = n の式を解くと x = Bn/A となるので、n を1づつインクリメントした x を N になるまで回してみた. ただこれは A < B のときにアレなので、その場合用に A まで回してみた. というちゃらんぽらんな合体で出してみたら AC が来て自分でもびっくりした. つまり、問題をあまり理解してないのに解けてしまった…….

D問題が解けた瞬間は900番台だったので青パフォの夢を見たが、E問題が解けなくて1500位手前まで後退してしまったのは残念.

```python
from math import floor

A, B, N = map(int, input().split())


def f(x):
    return floor(A * x / B) - A * floor(x / B)


result = 0
for x in range(min(A + 1, N + 1)):
    result = max(result, f(x))

if B > A:
    for i in range(10 ** 20):
        x = (i * B + A - 1) // A
        if x > N:
            break
        result = max(result, f(x))

print(result)
```

## [ABC165E - Rotation Matching](https://atcoder.jp/contests/abc165/tasks/abc165_e)

敗退
