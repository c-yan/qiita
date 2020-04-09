# AtCoder Grand Contest 040 参戦記

## [AGC040A - Dividing a String](https://atcoder.jp/contests/agc040/tasks/agc040_a)s

25分で突破したが、WA1なので30分扱い.

`><` の間は0で、`<>` の間は、左右の0から1づつインクリメントしてきた大きい方なので、以下みたいに2パスでやれば解ける.

```python
S = input()

N = len(S) + 1
t = [0] * N
for i in range(N - 1):
    if S[i] == '<':
        t[i + 1] = t[i] + 1
for i in range(N - 2, -1, -1):
    if S[i] == '>':
        t[i] = max(t[i], t[i + 1] + 1)
print(sum(t))
```

## [AGC040B - Two Contests](https://atcoder.jp/contests/agc040/tasks/agc040_b)

全く手付かずで敗退.
