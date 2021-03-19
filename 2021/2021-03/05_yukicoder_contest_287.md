# yukicoder contest 287 参戦記

起きたら始まっていて、A問題を解いて、B問題をちょっと考えたら眠くなってまた寝てた. コンテスト後に起きてB問題を解いてみたらそこまでは難しくなかった.

## [A 1432 Not Xor](https://yukicoder.me/problems/no/1432)

yukicoder では珍しい ABC-A レベルの問題. 使っている言語のビット演算子を知っていれば難なく解けるでしょう.

```python
A, B = map(int, input().split())

print((A | B) + (A & B))
```

## [B 1433 Two color sequence](https://yukicoder.me/problems/no/1433)

連続部分列の右端を固定して考えると、そこを右端とした部分列で最大のものは、先頭から始まる部分列で最小を引いたもの(プラスの値で絶対値の最大狙い)か、最大を引いたもの(マイナスの値で絶対値の最大狙い). なのでそこまでの最小値、最大値、合計の変数を更新しながらループすることによって *O*(*N*) で解ける.

```python
N = int(input())
S = input()
a = list(map(int, input().split()))

result = 0
minv = 0
maxv = 0
v = 0
for i in range(N):
    if S[i] == 'R':
        v += a[i]
    elif S[i] == 'B':
        v -= a[i]
    result = max(result, abs(v - minv))
    result = max(result, abs(v - maxv))
    minv = min(minv, v)
    maxv = max(maxv, v)
print(result)
```
