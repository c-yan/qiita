# yukicoder contest 299 (BANNED CONTEST) 参戦記

## [A 1543 [Cherry 2nd Tune A] これがプログラミングなのね](https://yukicoder.me/problems/no/1543)

cに従って比較をするだけ.

```python
S, T, c = input().split()

S = int(S)
T = int(T)

if c == '<':
    result = S < T
elif c == '>':
    result = S > T
elif c == '=':
    result = S == T

if result:
    print('YES')
else:
    print('NO')
```

## [B 1544 [Cherry 2nd Tune C] Synchroscope](https://yukicoder.me/problems/no/1544)

*O*(*NM*) が実行可能な範囲なので、愚直に比較すればいいだけ.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

for i in range(N * M):
    if A[i % N] != B[i % M]:
        continue
    print(i + 1)
    break
else:
    print(-1)
```
