# yukicoder contest 291 参戦記

## [A 1476 esreveR dna esreveR](https://yukicoder.me/problems/no/1476)

a<sub>i,i</sub> と a<sub>N-i,N-i</sub> の組み合わせ、それぞれが<sub>4</sub>C<sub>2</sub>で6パターンとなり、それぞれの組み合わせは互いに影響しあわないので、6<sup>⌊N/2⌋</sup>が答えとなる.

```python
m = 998244353

N = int(input())

print(pow(6, N // 2, m))
```
