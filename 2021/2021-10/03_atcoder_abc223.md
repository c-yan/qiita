# AtCoder Beginner Contest 223 参戦記

## [ABC223A - Exact Price](https://atcoder.jp/contests/abc223/tasks/abc223_a)

2分半で突破. 0円だけ特別であることを意識して書くだけ.

```python
X = int(input())

if X == 0:
    print('No')
elif X % 100 == 0:
    print('Yes')
else:
    print('No')
```

## [ABC223B - String Shifting](https://atcoder.jp/contests/abc223/tasks/abc223_b)

6分で突破. 全部実際に生成すればいいですね.

```python
S = input()

a = {S}
for i in range(1, len(S)):
    a.add(S[i:] + S[:i])
    a.add(S[:-i] + S[-i:])
a = sorted(a)
print(a[0])
print(a[-1])
```

## [ABC223C - Doukasen](https://atcoder.jp/contests/abc223/tasks/abc223_c)

26分半で突破、TLE1. 片側からだけ燃やしてかかる時間の半分の時間が、両側から燃やすのにかかる時間. 後は左からその時間でどこまで進むかシミュレーションすればOK! 所要時間をにぶたんして、左右からシミュレートしてちょうど燃え尽きるところを探したりして TLE してはいけません(笑).

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
AB = [tuple(map(int, readline().split())) for _ in range(N)]

s = sum(a / b for a, b in AB) / 2
result = 0
for a, b in AB:
    if s - a / b <= 0:
        result += s * b
        break
    s -= a / b
    result += a
print(result)
```

## [ABC223D - Restricted Permutation](https://atcoder.jp/contests/abc223/tasks/abc223_d)

48分半で突破. 考察が手強かったけど、思いついてしまえばそんなに実装は難しくはなくて面白かった. 既に自分より前に出ないといけない数が全て出終わっている数を管理し、これを「利用可能」と呼ぶ. 求めたい数列は辞書順最小なので、「利用可能」は性能の観点から heap で管理し、最小順に取り出して出力数列に加える. その値が出力数列に入ったことによって新たに「利用可能」が発生するかもしれないのでそれを処理する. もし循環などで矛盾があり数列Pが存在しない場合には、すべての数字を使い終わる前に「利用可能」が空になるので、出力数列の長さを確認すれば分かる.

```python
from sys import stdin
from heapq import heappop, heappush, heapify

readline = stdin.readline

N, M = map(int, readline().split())
AB = [tuple(map(int, readline().split())) for _ in range(M)]

fronts = [set() for _ in range(N + 1)]
rears = [set() for _ in range(N + 1)]

availables = set(range(1, N + 1))
for a, b in AB:
    if b in availables:
        availables.remove(b)
    fronts[b].add(a)
    rears[a].add(b)
availables = list(availables)
heapify(availables)

result = []
while availables:
    x = heappop(availables)
    result.append(x)
    for y in rears[x]:
        fronts[y].remove(x)
        if len(fronts[y]) == 0:
            heappush(availables, y)
if len(result) != N:
    print(-1)
else:
    print(*result)
```
