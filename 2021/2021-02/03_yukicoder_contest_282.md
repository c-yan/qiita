# yukicoder contest 282 参戦記

## [A 1389 Clumsy Calculation](https://yukicoder.me/problems/no/1389)

chinerist 君が間違わなかったとすると、S<sub>i</sub> の合計は X * N - (A<sub>1</sub> +  A<sub>2</sub> + ... + A<sub>N</sub>) = X * N - X = X * (N - 1) となる. 間違っているのが S<sub>j</sub> だとすると、S<sub>j</sub> = 2 * (X - A<sub>j</sub>) となり、実際の S<sub>i</sub> の合計は X - A<sub>j</sub> 増えた X * (N - 1) + (X - A<sub>j</sub>) となる. 求めるのは間違えた計算の正しい答え、つまり S<sub>j</sub> の正しい値 X - A<sub>j</sub> なので、実際の S<sub>i</sub> の合計から、正しい S<sub>i</sub> の合計 X * (N - 1) を引けば答えになる.

```python
N, X, *S = map(int, open(0).read().split())

print(sum(S) - X * (N - 1))
```
