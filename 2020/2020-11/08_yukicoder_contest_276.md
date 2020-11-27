# yukicoder contest 276 参戦記

## [A 1298 OR XOR](https://yukicoder.me/problems/no/1298)

C = N とすると、求める A, B の条件は A or B = N, A xor B = 0 となる. つまり、N を2進数で考えた時に立っているビットが A と B のどちら片方で立っていれば条件を満たせる. そのようなものの一つとして、N の MSB だけ落とした整数と、N の MSB だけが立った整数がある. ただし、前者は N が MSB だけが立っている場合には 0 となってしまうので、答えとならない.

```python
N = int(input())

c = -1
t = N
while t != 0:
    c += 1
    t >>= 1

B = 1 << c
A = N ^ B
C = N

if A == 0:
    print(-1, -1, -1)
else:
    print(A, B, C)
```

## [B 1299 Random Array Score](https://yukicoder.me/problems/no/1299)

ある A<sub>i</sub> が選ばれる可能性は 1 / N で、選ばれたときは A の要素の総和は A<sub>i</sub>×N 増える. これを積算すると結局1回の操作で A の要素の総和が増える期待値は A の要素の総和そのものである. つまり1回の操作毎に倍になるので、最終的に求める解は A の要素の総和×2<sup>K</sup>となる.

```python
N, K, *A = map(int, open(0).read().split())

m = 998244353

print(sum(A) * pow(2, K, m) % m)
```
