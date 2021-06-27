# AtCoder Beginner Contest 207 参戦記

ABC 完で水色中間パフォが出るって、鬼畜セットやな.

## [ABC207A - Repression](https://atcoder.jp/contests/abc207/tasks/abc207_a)

1分半で突破. 3パターンしか無いので、全部計算してしまえばいい.

```python
A, B, C = map(int, input().split())

print(max(A + B, B + C, A + C))
```

## [ABC207B - Hydrate](https://atcoder.jp/contests/abc207/tasks/abc207_b)

8分で突破. B問題としてはかなり難しいのでは. 腐った読みから言えば、B問題はシングルループで解ける問題なので答えは10<sup>7</sup>くらい回せば出るんだろうなあとは思ったんだけど確信がなく、とはいえ 64bit int 以内ではあるだろうということでにぶたんでオーバーキルをした.

```python
A, B, C, D = map(int, input().split())

INF = 10 ** 20

ok = INF
ng = 0
while ok - ng > 1:
    m = ng + (ok - ng) // 2
    if A + B * m <= C * m * D:
        ok = m
    else:
        ng = m

if ok == INF:
    print(-1)
else:
    print(ok)
```

真面目に式変形をすれば A / (CD - B) ≦ x となり、最大 A 回でいいことが分かるようですが、ここまで来たら O(1) で解いちゃうよね.

```python
A, B, C, D = map(int, input().split())

x = C * D - B
if x <= 0:
    print(-1)
else:
    print((A + x - 1) // x)
```

## [ABC207C - Many Segments](https://atcoder.jp/contests/abc207/tasks/abc207_c)

11分で突破. 真面目に開区間と閉区間を場合分けで処理したくないので、閉区間に統一したい. 開区間は要するには 1/∞ だけずれていると思えばいいのだが、まあ今回に限っては 0＜x≦0.5 で代用できるので 0.1 を使って閉区間として解いた. 0.5を採用して、2倍して全て整数でやっても良さそう.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
tlr = [map(int, readline().split()) for _ in range(N)]

lr = []
for t, l, r in tlr:
    if t == 1:
        lr.append((l, r))
    elif t == 2:
        lr.append((l, r - 0.1))
    elif t == 3:
        lr.append((l + 0.1, r))
    elif t == 4:
        lr.append((l + 0.1, r - 0.1))

result = 0
for i in range(N):
    for j in range(i + 1, N):
        a, b = lr[i]
        c, d = lr[j]
        if c > b or d < a:
            continue
        result += 1
print(result)
```
