# AtCoder Beginners Contest 過去問チャレンジ9

## [ABC071D - Coloring Dominoes](https://atcoder.jp/contests/abc071/tasks/arc081_b)

まず左端が縦だったら塗り方は3通りである. 横の場合は 3 * 2 で6通りである. 次に縦の次に縦が来た場合、前の縦の色と違えばいいので2通りとなる. 縦の次に横が来た場合、上側から決めるとすると、縦と違うのにすればいいので2通りで、そうすると下側はもう一通りしか無いので、2通りとなる. 横の次に縦が来た場合は、既に2色決まっているので1通りしか無い. 横の次に横が来たときは、以下の通り3通りある.

```
0022  0011  0011
1100  1100  1122
```

後はそれを積算すれば良い.

```python
N = int(input())
S1 = input()
S2 = input()

if S1[0] == S2[0]:
    result = 3
else:
    result = 6
x = 0
W = len(S1)
while x < W:
    if S1[x] == S2[x]:
        if x + 1 == W:
            break
        result *= 2
        result %= 1000000007
        x += 1
    else:
        if x + 2 == W:
            break
        if S1[x + 2] != S2[x + 2]:
            result *= 3
            result %= 1000000007
        x += 2
print(result)
```

## [ABC109D - Make Them Even](https://atcoder.jp/contests/abc109/tasks/abc109_d)

最小移動でという制限は付いていないので、左上から右に行き、端まで来たら一つ下に下がって左に行き、端まで来たら一つ下に下がって右に行き……というつづらスキャンをして、奇数枚だったら次のマスに送るを繰り返せば全て偶数枚か、最後のマスだけ奇数枚のどちらかになる.

```python
H, W = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(H)]

result = []
y = 0
while y < H:
    x = 0
    while x < W:
        if a[y][x] % 2 == 1:
            if x == W - 1:
                if y == H - 1:
                    break
                else:
                    result.append('%d %d %d %d' % (y + 1, x + 1, y + 2, x + 1))
                    a[y + 1][x] += 1
            else:
                result.append('%d %d %d %d' % (y + 1, x + 1, y + 1, x + 2))
                a[y][x + 1] += 1
        x += 1
    if y == H - 1:
        break
    y += 1
    x = W - 1
    while x >= 0:
        if a[y][x] % 2 == 1:
            if x == 0:
                if y == H - 1:
                    break
                else:
                    result.append('%d %d %d %d' % (y + 1, x + 1, y + 2, x + 1))
                    a[y + 1][x] += 1
            else:
                result.append('%d %d %d %d' % (y + 1, x + 1, y + 1, x))
                a[y][x - 1] += 1
        x -= 1
    y += 1

print(len(result))
print('\n'.join(result))
```

## [ABC096D - Five, Five Everywhere](https://atcoder.jp/contests/abc096/tasks/abc096_d)

5で割ると1余る素数を必要な数だけ小さい順に並べれば良い. どの組み合わせでも5の倍数になる.

```python
N = int(input())

result = []
sieve = [0] * (55555  + 1)
sieve[0] = -1
sieve[1] = -1
for i in range(2, 55555 + 1):
    if sieve[i] != 0:
        continue
    if i % 5 == 1:
        result.append(i)
        if len(result) == N:
            break
    sieve[i] = i
    for j in range(i * i, 55555 + 1, i):
        if sieve[j] == 0:
            sieve[j] = i
print(*result)
```

## [ABC099D - Good Grid](https://atcoder.jp/contests/abc099/tasks/abc099_d)

色の組み合わせは <sub>30</sub>C<sub>3</sub> = 4060 なので全探索でいいのだが、N≤500 なので、何も考えずにやると *O*(4060×500<sup>2</sup>≒10<sup>10</sup>) で TLE 必至. (i + j) % 3 の余り毎にグルーピングして元の色を数え上げておけば *O*(4060×30×3≒3.7×10<sup>5</sup>) なので解決.

```python
from itertools import permutations

N, C = map(int, input().split())
D = [list(map(int, input().split())) for _ in range(C)]
c = [list(map(int, input().split())) for _ in range(N)]

a = [[0] * C for _ in range(3)]
for y in range(N):
    for x in range(N):
        t = ((x + 1) + (y + 1)) % 3
        a[t][c[y][x] - 1] += 1

result = float('inf')
for b in permutations(range(C), 3):
    t = 0
    for i in range(3):
        for j in range(C):
            t += a[i][j] * D[j][b[i]]
    result = min(result, t)
print(result)
```

## [ABC103D - Islands War](https://atcoder.jp/contests/abc103/tasks/abc103_d)

bでソートすればいいというのは、[キーエンス プログラミング コンテスト 2020 B - Robot Arms](https://atcoder.jp/contests/keyence2020/tasks/keyence2020_b) のときに痛い目にあったので直感した. 区間スケジューリング問題というやつである. 後は右端の橋を必要なだけ取り除いていけばいいのだが、最初はセグ木で取り除いた橋を記録して解いてしまった. まあ、*O*(<i>N</i>log<i>N</i>) で解けるのだが、実際には最後に取り除いたものだけ覚えていれば大丈夫だったことに解説 PDF を見て気づいた.

```python
N, M = map(int, input().split())
ab = [list(map(int, input().split())) for _ in range(M)]

ab.sort(key=lambda x: x[1])

result = 0
t = -1
for a, b in ab:
    a, b = a - 1, b - 1
    if a <= t < b:
        continue
    result += 1
    t = b - 1
print(result)
```

## [ABC055D - Menagerie](https://atcoder.jp/contests/abc055/tasks/arc069_b)

1番と2番の動物を決めてしまえば、後は1択となり、間違っていれば最後に矛盾することは分かる. なので、1番目と2番目を S, W の総当たりの4パターンやればいい. *O*(4×10<sup>5</sup>) なので計算量的にも問題ない.

```python
from itertools import product

N = int(input())
s = input()

swap = {'S': 'W', 'W': 'S'}
for a, b in product('SW', repeat=2):
    t = [None] * N
    t[0] = a
    t[1] = b
    for i in range(1, N - 1):
        if t[i] == 'S':
            if s[i] == 'o':
                t[i + 1] = t[i - 1]
            elif s[i] == 'x':
                t[i + 1] = swap[t[i - 1]]
        elif t[i] == 'W':
            if s[i] == 'o':
                t[i + 1] = swap[t[i - 1]]
            elif s[i] == 'x':
                t[i + 1] = t[i - 1]
    if t[N - 1] == 'S':
        if s[N - 1] == 'o' and t[N - 2] != t[0]:
            continue
        elif s[N - 1] == 'x' and t[N - 2] == t[0]:
            continue
    elif t[N - 1] == 'W':
        if s[N - 1] == 'o' and t[N - 2] == t[0]:
            continue
        elif s[N - 1] == 'x' and t[N - 2] != t[0]:
            continue
    if t[0] == 'S':
        if s[0] == 'o' and t[N - 1] != t[1]:
            continue
        elif s[0] == 'x' and t[N - 1] == t[1]:
            continue
    elif t[0] == 'W':
        if s[0] == 'o' and t[N - 1] == t[1]:
            continue
        elif s[0] == 'x' and t[N - 1] != t[1]:
            continue
    print(''.join(t))
    exit()
print(-1)
```
