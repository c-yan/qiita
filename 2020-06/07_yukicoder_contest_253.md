# yukicoder contest 253 参戦記

## [A 1081 和の和](https://yukicoder.me/problems/no/1081)

結論的に ∑<sub>i-1</sub>C<sub>N-1</sub>A<sub>i</sub> になる. N≤100 なのでパスカルの三角形でも解けるだろうけど、めんどくさいのでいつもの mcomb を貼った.


```python
N = int(input())
A = list(map(int, input().split()))

fac = [0] * N
fac[0] = 1
for i in range(N - 1):
    fac[i + 1] = fac[i] * (i + 1) % 1000000007


def mcomb(n, k):
    if n == 0 and k == 0:
        return 1
    if n < k or k < 0:
        return 0
    return fac[n] * pow(fac[n - k], 1000000005, 1000000007) * pow(fac[k], 1000000005, 1000000007) % 1000000007


result = 0
for i in range(N):
    result += mcomb(N - 1, i) * A[i]
    result %= 1000000007
print(result)
```

## [B 1082 XORのXOR](https://yukicoder.me/problems/no/1082)

並べ替えた A<sub>i</sub> を元にすると、X = A<sub>1</sub> xor A<sub>2</sub> xor A<sub>2</sub> xor A<sub>3</sub> xor ... xor A<sub>N - 1</sub> xor A<sub>N - 1</sub> xor A<sub>N</sub> となる. A<sub>1</sub> xor A<sub>1</sub> = 1 なので、X = A<sub>1</sub> xor A<sub>N</sub> となる. 結果として任意の i != j の i, j について A<sub>i</sub> xor A<sub>j</sub> の最大値が答えとなる.

```python
N = int(input())
A = list(map(int, input().split()))

result = 0
for i in range(N - 1):
    for j in range(i + 1, N):
        result = max(result, A[i] ^ A[j])
print(result)
```

## [C 1083 余りの余り](https://yukicoder.me/problems/no/1083)

K を i で割ったとして、j > i である j については K % i % j = K % i である. なので A を大きい順にソートして、A<sub>N</sub> で割るまでの総当りをすれば良い. 前進するだけなので計算量的に解ける.

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))

A.sort(reverse=True)


def f(k, i):
    if i == N:
        return k
    result = -1
    for j in range(i, N):
        result = max(result, f(k % A[j], j + 1))
    return result


print(f(K, 0))
```
