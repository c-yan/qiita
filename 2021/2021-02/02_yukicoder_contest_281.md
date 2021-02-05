# yukicoder contest 281 参戦記

## [A 1372 Median of Submasks](https://yukicoder.me/problems/no/1372)

MSB(most significant bit) が立つまでと、立った後が同じなので、N の MSB だけ立っているのが答え.

```python
N = int(input())

i = 0
while N != 0:
    N >>= 1
    i += 1
print(1 << (i - 1))
```
