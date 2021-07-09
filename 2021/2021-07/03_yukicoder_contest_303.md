# yukicoder contest 303 参戦記

## [A 1585 Cubic Number](https://yukicoder.me/problems/no/1585)

*N*≦10<sup>18</sup> なので、シングルループで一発ではある.

```python
N = int(input())

for i in range(1, 10 ** 6 + 1):
    if i ** 3 != N:
        continue
    print('Yes')
    break
else:
    print('No')
```

にぶたんでもいい.

```python
N = int(input())

ok = 0
ng = 10 ** 6 + 1
while ng - ok > 1:
    m = ok + (ng - ok) // 2
    if m ** 3 <= N:
        ok = m
    else:
        ng = m
if ok ** 3 == N:
    print('Yes')
else:
    print('No')
```

三乗根を取って、その近辺をチェックしてもいい.

```python
N = int(input())

n = int(N ** (1 / 3))
for i in range(n - 1, n + 1 + 1):
    if i ** 3 != N:
        continue
    print('Yes')
    break
else:
    print('No')
```

## [B 1586 Equal Array](https://yukicoder.me/problems/no/1586)

操作自体が +1 と -1 の組で、トータルでは±0なので、合計が N の倍数になっているかが全て.

```python
N, *A = map(int, open(0).read().split())

if sum(A) % N == 0:
    print('Yes')
else:
    print('No')
```
