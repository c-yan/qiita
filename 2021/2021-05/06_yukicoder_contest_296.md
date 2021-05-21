# yukicoder contest 296 参戦記

## [A 1511 A ? B Problem](https://yukicoder.me/problems/no/1511)

使っているプログラミング言語の論理和と論理積の演算子が分かっていれば書くだけ.

```python
A, B = map(int, input().split())

print((A | B) - (A & B))
```

## [B 1512 作文](https://yukicoder.me/problems/no/1512)

まず文字列内の文字がソート順に並んでないものは良い文字列になることがなく、選択できないので省く. 次に aa の後に ab は接続できても、その逆は無理なのでソートする. 後は末尾文字をキーとした DP をすれば解ける. 開始文字が DP のキー以上のときに、末尾のキーに対して最大長を取る DP をすればいい.

```python
from sys import stdin

readline = stdin.readline

N = int(readline())
S = [readline()[:-1] for _ in range(N)]

dp = {'a': 0}
for s in sorted(S):
    if ''.join(sorted(s)) != s:
        continue
    start = s[0]
    last = s[-1]
    l = len(s)
    dp.setdefault(last, 0)
    for c in 'zyxwvutsrqponmlkjihgfedcba':
        if c not in dp:
            continue
        if start < c:
            continue
        dp[last] = max(dp[last], dp[c] + l)
print(max(dp.values()))
```
