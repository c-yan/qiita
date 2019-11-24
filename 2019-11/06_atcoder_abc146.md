# AtCoder Beginner Contest 146 参戦記

## A - Can't Wait for Holiday

2分半で突破. 書くだけ.

```python
S = input()

if S == 'SUN':
    print(7)
elif S == 'MON':
    print(6)
elif S == 'TUE':
    print(5)
elif S == 'WED':
    print(4)
elif S == 'THU':
    print(3)
elif S == 'FRI':
    print(2)
elif S == 'SAT':
    print(1)
```

## B - ROT N

4分半で突破. 書くだけ.

```python
N = int(input())
S = input()

print(''.join(chr((ord(c) - ord('A') + N) % 26 + ord('A')) for c in S))
```

## C - Buy an Integer

16分で突破. 一目にぶたんで、最近やったなーと ABC144E のソースコードをコピってきて、is_ok を書き直して、調整してポイ. 調整に手間取ったのでまだめぐる式が手についてないなあという感想.

```python
A, B, X = map(int, input().split())


def is_ok(N):
    return A * N + B * len(str(N)) <= X


ok = 0
ng = 10 ** 9 + 1
while ng - ok > 1:
    m = (ok + ng) // 2
    if is_ok(m):
        ok = m
    else:
        ng = m
print(ok)
```

## D - Coloring Edges on Tree

敗退. 幅優先探索というアルゴリズム方針はあっていたようなのだが……. 最初に書いたのは TLE を食らい、書き直したのはデータ構造が駄目で MLE を食らったので…….
