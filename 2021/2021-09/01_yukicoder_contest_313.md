# yukicoder contest 313 参戦記

1時間遅れの参加だったけど、ABCが解けて嬉しかった.

## [A 1672 404 Not Found](https://yukicoder.me/problems/no/1672)

ステータスコードの値を100で割って4か5ならエラーコードですね.

```python
S = int(input())

if S // 100 in [4, 5]:
    print('Yes')
else:
    print('No')
```

文字列で処理してもいいですけどね.

```python
S = input()

if S[0] in '45':
    print('Yes')
else:
    print('No')
```

## [B 1673 Lamps on a line](https://yukicoder.me/problems/no/1673)

R<sub>i</sub>-L<sub>i</sub>≦10 という条件なので素直にシミュレーションすればいいです.

```python
from sys import stdin

readline = stdin.readline

N, Q = map(int, readline().split())

c = 0
t = [0] * (N + 1)
result = []
for _ in range(Q):
    L, R = map(int, readline().split())
    for i in range(L - 1, R):
        if t[i] == 0:
            c += 1
            t[i] = 1
        elif t[i] == 1:
            c -= 1
            t[i] = 0
    result.append(c)
print(*result, sep='\n')
```

## [C 1674 Introduction to XOR](https://yukicoder.me/problems/no/1674)

ちょっと考えると、どの A<sub>i</sub> でも立っていない LSB(Least Significant Bit / 最下位ビット)だと分かります.

```python
N, *A = map(int, open(0).read().split())

x = 0
for a in A:
    x |= a

for i in range(x.bit_length() + 1):
    if (x >> i) & 1 == 0:
        print(1 << i)
        break
```
