# yukicoder contest 284 参戦記

## [A 1406 Test](https://yukicoder.me/problems/no/1406)

0点から100点までの101パターン、平均点が整数になるかをチェックすればいいだけ.

```python
N, *A = map(int, open(0).read().split())

if N == 1:
    print(101)
    exit()

a = sum(A)
result = 0
for i in range(100 + 1):
    if (i + a) % N == 0:
        result += 1
print(result)
```
