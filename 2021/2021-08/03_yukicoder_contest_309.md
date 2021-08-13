# yukicoder contest 309 参戦記

## [A 1642 Registration](https://yukicoder.me/problems/no/1642)

自分が使っている言語の書式指定子を知っているかだけですね.

```python
N = int(input())

print('%03d' % N)
```

## [B 1643 Not Substring](https://yukicoder.me/problems/no/1643)

文字列を辞書順に並べると、'a', 'aa', 'aaa', 'aaaa', ... となる. 後は 'a' を幾つ並べるとSの部分列では無くなるかを考えれば 'a' の個数を数えて、それより1つ長くすればいいと分かるはず.

```python
S = input()

print('a' * (S.count('a') + 1))
```

## [C 1644 Eight Digits](https://yukicoder.me/problems/no/1644)

8! ≒ 4.0×10<sup>4</sup> なので、総当りしても TLE しないので、総当たりすれば良いと分かる.

```python
from itertools import permutations

K = int(input())

result = 0
for p in permutations('12345678'):
    if int(''.join(p)) % K == 0:
        result += 1
print(result)
```

## [D 1645 AB's abs](https://yukicoder.me/problems/no/1645)

*N*≦100で、A<sub>i</sub>≦100なのを見たらナップサック問題かなってなりますよね.

```python
from collections import defaultdict

m = 998244353

N, *A = map(int, open(0).read().split())

d = {0: 1}
for a in A:
    t = defaultdict(int)
    for k in d:
        t[k + a] += d[k]
        t[k + a] %= m
        t[k - a] += d[k]
        t[k - a] %= m
    d = t

result = 0
for k in d:
    result += abs(k) * d[k]
    result %= m
print(result)
```
