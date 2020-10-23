# yukicoder contest 270 (mathematics contest) 参戦記

mathematics という文字列を見ただけでやる気が無くなるのはなぜだろうか. そしてまた鯖落ち.

## [A 1256 連続整数列](https://yukicoder.me/problems/no/1256)

長さ3の連続整数列を考える. 0+1+2=3 で、これに +3n できるので、3の倍数なら何でも生成できる. 同様の長さ4の整数列は4n+2、長さ5の整数列は5nが、長さ6の整数列は6n+3が生成できることが分かる. これをまとめると長さnの整数列は、nが奇数の場合にはnの倍数が、nが偶数のときにはnの倍数+(n/2)が生成できることが分かる.

つまりAが偶数の場合には長さ2Aの連続整数列で必ず生成できる. Aが奇数の場合には長さAもしくは長さ2Aの連続整数列で必ず生成できる. ところで問題の制約により長さ1, 2の連続整数列は使えないので、1だけが生成できない整数になる.

```python
A = int(input())

if A == 1:
    print('NO')
else:
    print('YES')
```

## [B 1257 変わった平均値](https://yukicoder.me/problems/no/1257)

操作1を1回だけやると整数ではなくなって絶対に一致しない. かといって2回やるともとの値に戻る. となると使える操作は実質2だけになるので、A, B, C と D, E, F の構成する整数が順序を変えただけなら、'Yes' で1回だけ操作2をしてやればよく、そうでないなら 'No' となる.

```python
A, B, C = map(int, input().split())
D, E, F = map(int, input().split())

if sorted([A, B, C]) == sorted([D, E, F]):
    print('Yes')
    print(2)
else:
    print('No')
```

## [C 1258 コインゲーム](https://yukicoder.me/problems/no/1258)

求めるものは X = 0 のとき <sub>N</sub>C<sub>0</sub>M<sup>0</sup>+<sub>N</sub>C<sub>2</sub>M<sup>2</sup>+... となり、X = 1 のときは <sub>N</sub>C<sub>1</sub>M<sup>1</sup>+<sub>N</sub>C<sub>3</sub>M<sup>3</sup>+... となる. ところで (M+1)<sup>N</sup>=<sub>N</sub>C<sub>N</sub>M<sup>N</sup>+<sub>N</sub>C<sub>N-1</sub>M<sup>N-1</sup>+...+<sub>N</sub>C<sub>0</sub>M<sup>0</sup> であり (M-1)<sup>N</sup>=<sub>N</sub>C<sub>N</sub>M<sup>N</sup>-<sub>N</sub>C<sub>N-1</sub>M<sup>N-1</sup>+... なことから (M+1)<sup>N</sup> と (M-1)<sup>N</sup> から求めるものは計算できる.

```python
from sys import stdin

readline = stdin.readline
m = 1000000007

S = int(readline())
div2 = pow(2, m - 2, m)
for _ in range(S):
    N, M, X = map(int, readline().split())
    a = pow(M + 1, N, m)
    b = pow(M - 1, N, m)
    if (N + X) % 2 == 0:
        print((a + b) * div2 % m)
    else:
        print((a - b) * div2 % m)
```
