# yukicoder contest 261 参戦記

## [A 1168 Digit Sum Sequence](https://yukicoder.me/problems/no/1168)

増えていったりしないので、何も考えずに99回ループを回せば OK.

```python
N = int(input())

for _ in range(99):
    N = sum(int(c) for c in str(N))
print(N)
```

## [B 1169 Row and Column and Diagonal](https://yukicoder.me/problems/no/1169)

解けず.
