# yukicoder contest 297 参戦記

## [A 1517 Party Location](https://yukicoder.me/problems/no/1517)

重心に集合すれば良いのかと思ってたけど、どっちかの町だったのか……. WAになってしまったので、「つねに整数」を頼りに全探索になってしまった…….

```python
d = int(input())
a, b = map(int, input().split())

result = 10 ** 15
for i in range(d + 1):
    result = min(result, i * a + (d - i) * b)
print(result)
```

## [B 1518 Simple Combinatorics](https://yukicoder.me/problems/no/1518)

完全に数学. ある整数が出る確率は、1から出ない確率を引いたものなので 1-((N - 1)/N)<sup>K</sup>. これがN種類あり、求める答えはN<sup>K</sup>をかけたものなので、N<sup>K+1</sup> - N(N-1)<sup>K</sup> (≡ 10<sup>9</sup> + 7) となる.

```python
m = 1000000007

N, K = map(int, input().split())

print((pow(N, K + 1, m) - N * pow(N - 1, K, m)) % m)
```
