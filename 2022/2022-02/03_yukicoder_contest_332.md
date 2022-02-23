# yukicoder contest 332 不参戦記

## [A 1841 Long Long](https://yukicoder.me/problems/no/1841)

Python だと乗算一発.

```python
N = int(input())

print('Long' * N)
```

## [B 1842 Decimal Point](https://yukicoder.me/problems/no/1842)

Aに10<sup>C</sup>をかけて、Bで割った商を10で割ったあまりが答えだけど、10<sup>C</sup>が大きすぎて計算できない. ここで10×BをBで割った商を10で割ったあまりが常に0になるという事実に気づくと計算可能になる.

```python
T = int(input())

for _ in range(T):
    A, B, C = map(int, input().split())
    print((A * pow(10, C, B * 10)) // B % 10)
```
