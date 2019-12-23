# AtCoder Beginner Contest 148 参戦記

## A - Round One

2分半で突破. 簡単なんだけど、なんかシンプルに書こうと思うと難しいなあと思った.

```python
A = int(input())
B = int(input())

print(*(set([1, 2, 3]) - set([A, B])))
```

## B - Strings with the Same Length

1分半で突破. 書くだけ. zip かなあと思いつつ使わなかった.

```python
N = int(input())
S, T = input().split()

print(''.join(S[i] + T[i] for i in range(N)))
```

## C - Snack

3分で突破. 最小公倍数が答えだけど、どう解けばいいんだっけ、最大公約数が分かれば分かる?と思いつつ、Google に「最小公倍数 Python」ってググったら `lcm(a, b) = a * b / gcd(a, b)` って出てきたので書いて終わり.

```python
from fractions import gcd

A, B = map(int, input().split())

print(A * B // gcd(A, B))
```

## E - Double Factorial

20分くらい?で突破. WA1. Dをちょっと考えたけど難しかったので、飛ばしてこっちをやってみた. とりあえずナイーブに f(n) を実装して、動かしてみた. N が奇数のときは常に0はすぐ分かった.
`N // 10` で入力例3を流すとだいぶズレたので、n 増やして試してみたところ、f(50) で数字がずれて、50には5が含まれるからカーとなる. `N // 10 + N // 50` でも入力例3とは合わず、脳内で次は250カーとなってようやくからくりが分かって実装. `while t <= N:` を `while t < N:` と書いて WA を食らったが無事AC.

```python
from sys import exit

N = int(input())

if N % 2 == 1:
    print(0)
    exit()

result = 0
t = 10
while t <= N:
    result += N // t
    t *= 5
print(result)
```

## D - Brick Break

27分くらい?で突破. 右から処理することばっかり考えてて、難しいなと飛ばしてEからやって戻ってきたんだけど、順位表の状況から明らかに簡単なので、多分考えすぎてると左から処理することを考えたら簡単だった orz.

```python
N = int(input())
a = [int(s) - 1 for s in input().split()]

result = 0
for i in range(N):
    if a[i] != (i - result):
        result += 1

if result == N:
    print(-1)
else:
    print(result)
```

## F - Playing Tag on Tree

突破できず. ミニマックス法なのは分かったけど、過去問であたったのは一回だけで、幅優先だとどうなるんだろうと考えているうちに終わった. 精進不足.

追記: ミニマックス法じゃなかった(爆). 解説PDF通りで、青木さんから一番遠いところかつ高橋さんのほうが一番近いところの一個手前で捕まる感じで実装すれば良い.

```python
from sys import exit

N, u, v = map(int, input().split())

if u == v:
    print(0)
    exit()

edges = [[] for _ in range(N + 1)]
for _ in range(N - 1):
    A, B = map(int, input().split())
    edges[A].append(B)
    edges[B].append(A)


def calc_destination(start, edges):
    destination = [-1] * (N + 1)
    destination[start] = 0
    q = [start]
    while len(q) != 0:
        current = q.pop()
        for n in edges[current]:
            if destination[n] != -1:
                continue
            destination[n] = destination[current] + 1
            q.append(n)
    return destination


tak = calc_destination(u, edges)
aok = calc_destination(v, edges)

result = 0
for i in range(1, N + 1):
    aoki = aok[i]
    if tak[i] >= aoki:
        continue
    if aoki > result:
        result = aoki
print(result - 1)
```
