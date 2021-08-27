# yukicoder contest 311 参戦記

1時間遅れの参加でAしか解けませんでした.

## [A 1656 Recuperation](https://yukicoder.me/problems/no/1656)

B 日でストレスが A×B になって、それを癒やすのに A×B 日かかるので、合計日数は B + A×B = (A+1)B 日ですね.

```python
from sys import stdin

readline = stdin.readline

T = int(readline())

result = []
for _ in range(T):
    A, B = map(int, readline().split())
    result.append((A + 1) * B)
print(*result, sep='\n')
```

## [C 1658 Product / Sum](https://yukicoder.me/problems/no/1658)

直感的に A<sub>1</sub>=x, A<sub>2</sub>=y, A<sub>3</sub>=1, A<sub>4</sub>=1, ..., A<sub>N</sub>=1 なんだろうなと思い、xy/(x + y + N - 2) = K という式を書きましたが、ここから式変形する才能がありませんでした orz.

```python
N, K = map(int, input().split())

result = [1] * N
result[0] = 2 * K
result[1] = 2 * K + N - 2
print(*result)
```
