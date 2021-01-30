# yukicoder contest 280 （門松コンテスト) 参戦記

## [A 1366 交換門松列・梅](https://yukicoder.me/problems/no/1366)

書くだけではあるけど、途中でやけっぱちになってしまってこうなった.

```python
def is_kadomatsu(a, b, c):
    if a == b or b == c or a == c:
        return False
    return min([a, b, c]) == b or max([a, b, c]) == b


a1, a2, a3 = map(int, input().split())
b1, b2, b3 = map(int, input().split())

if is_kadomatsu(b1, a2, a3) and is_kadomatsu(a1, b2, b3):
    print('Yes')
elif is_kadomatsu(b2, a2, a3) and is_kadomatsu(b1, a1, b3):
    print('Yes')
elif is_kadomatsu(b3, a2, a3) and is_kadomatsu(b1, b2, a1):
    print('Yes')
elif is_kadomatsu(a1, b1, a3) and is_kadomatsu(a2, b2, b3):
    print('Yes')
elif is_kadomatsu(a1, b2, a3) and is_kadomatsu(b1, a2, b3):
    print('Yes')
elif is_kadomatsu(a1, b3, a3) and is_kadomatsu(b1, b2, a2):
    print('Yes')
elif is_kadomatsu(a1, a2, b1) and is_kadomatsu(a3, b2, b3):
    print('Yes')
elif is_kadomatsu(a1, a2, b2) and is_kadomatsu(b1, a3, b3):
    print('Yes')
elif is_kadomatsu(a1, a3, b3) and is_kadomatsu(b1, b2, a3):
    print('Yes')
else:
    print('No')
```

交換の組み合わせを全パターン、素直にループでやるべきですよね. 交換して、成立するかチェックして、同じ交換をもう一度して元に戻すの繰り返し.

```python
def is_kadomatsu(a):
    if a[0] == a[1] or a[1] == a[2] or a[0] == a[2]:
        return False
    return min(a) == a[1] or max(a) == a[1]

a = list(map(int, input().split()))
b = list(map(int, input().split()))

for i in range(3):
    for j in range(3):
        a[i], b[j] = b[j], a[i]
        if is_kadomatsu(a) and is_kadomatsu(b):
            print('Yes')
            exit()
        a[i], b[j] = b[j], a[i]
print('No')
```

## [B 1367 文字列門松](https://yukicoder.me/problems/no/1367)

与えられた文字が同じ順番で kadomatsu に入っているか確認するだけなので、文字列検索関数を繰り返し適用すれば解けます.

```python
S = input()

i = 0
for c in S:
    j = 'kadomatsu'.find(c, i)
    if j == -1:
        print('No')
        break
    i = j + 1
else:
    print('Yes')
```

## [C 1368 サイクルの中に眠る門松列](https://yukicoder.me/problems/no/1368)

循環しなければ簡単に DP で解けるのだが……. 先頭の当たりを使ったか使ってないか覚えながら DP するのは大変なので、先頭の使用を禁止したパターンと、先頭から2番目までの使用を禁止したパターンを含め3回 DP して答えを求めた.

```python
def is_kadomatsu(a, b, c):
    if a == b or b == c or a == c:
        return False
    return min([a, b, c]) == b or max([a, b, c]) == b


def solve():
    dp = [0] * (N + 1)
    for i in range(N + 1):
        if i != 0:
            dp[i] = max(dp[i], dp[i - 1])
        if i <= N - 3:
            if is_kadomatsu(A[i], A[i + 1], A[i + 2]):
                dp[i + 3] = dp[i] + A[i]
    result = dp[N]
    if is_kadomatsu(A[N - 1], A[0], A[1]):
        dp = [0] * (N + 1)
        dp[2] = A[N - 1]
        for i in range(2, N + 1):
            dp[i] = max(dp[i], dp[i - 1])
            if i <= N - 4:
                if is_kadomatsu(A[i], A[i + 1], A[i + 2]):
                    dp[i + 3] = dp[i] + A[i]
        result = max(result, dp[N])
    if is_kadomatsu(A[N - 2], A[N - 1], A[0]):
        dp = [0] * (N + 1)
        dp[1] = A[N - 2]
        for i in range(1, N + 1):
            dp[i] = max(dp[i], dp[i - 1])
            if i <= N - 5:
                if is_kadomatsu(A[i], A[i + 1], A[i + 2]):
                    dp[i + 3] = dp[i] + A[i]
        result = max(result, dp[N])
    return result


T = int(input())
for _ in range(T):
    N = int(input())
    A = list(map(int, input().split()))
    print(solve())
```

よくよく考えると循環しない数列を3つ用意したほうが楽ですね.

```python
def is_kadomatsu(a):
    if a[0] == a[1] or a[1] == a[2] or a[0] == a[2]:
        return False
    return min(a) == a[1] or max(a) == a[1]


def f(a):
    dp = [0] * (N + 1)
    for i in range(N + 1):
        if i != 0:
            dp[i] = max(dp[i], dp[i - 1])
        if i <= N - 3:
            if is_kadomatsu(a[i:i + 3]):
                dp[i + 3] = dp[i] + a[i]
    return dp[N]


T = int(input())
for _ in range(T):
    N = int(input())
    A = list(map(int, input().split()))
    print(max([f(A), f(A[1:N] + A[:1]), f(A[2:N] + A[:2])]))
```
