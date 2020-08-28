# yukicoder contest 263 参戦記

## [A 1198 お菓子配り-1](https://yukicoder.me/problems/no/1198)

突破できず. どこが★1だよというツッコミどころしかない. N≤10<sup>25</sup> で int64 に収まらず、これだけでも★1ではないよなと思った. まあ、Python は int64 の値域を超えてても平気であるが. 数学問題なんだろうなあとは思ったけど、やっぱり解けなかった.

```python
N = int(input())

if N == 1 or N == 4 or N % 4 == 2:
    print(-1)
else:
    print(1)
```

## [No.1199 お菓子配り-2](https://yukicoder.me/problems/no/1199)

素直な DP. 偶数個取った時の最大値、奇数個取った時の最大値を更新してばいいだけ.

```python
from sys import stdin
readline = stdin.readline

N, M = map(int, readline().split())

odd, even = 0, 0
for _ in range(N):
    s = sum(map(int, readline().split()))
    t = odd
    odd = max(odd, even + s)
    even = max(even, t - s)
print(odd)
```
