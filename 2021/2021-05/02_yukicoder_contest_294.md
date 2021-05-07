# yukicoder contest 294 参戦記

B問題、初めての1桁番目の順位で解けて驚いた,

## [A 1499 羊が、何匹だっけ](https://yukicoder.me/problems/no/1499)

バラして、1足して、戻すだけですよね.

```python
T = int(input())
for _ in range(T):
    s = input()
    t = s.split()
    t[2] = str(int(t[2]) + 1)
    print(' '.joint(t))
```

## [B 1500 Super Knight](https://yukicoder.me/problems/no/1500)

先週のA問題と同じやんけってなった. なのでまずナイーブなコードを書く.

```python
def f(n):
    q = set([(0, 0)])
    for _ in range(n):
        nq = set()
        for x, y in q:
            for dx, dy in [(3, 0), (3, 2), (2, 3), (0, 3), (-2, 3), (-3, 2), (-3, 0), (-3, -2), (-2, -3), (0, -3), (2, -3), (3, -2)]:
                nq.add((x + dx, y + dy))
        q = nq
    return len(q)
```

実際に実行してみる.

```
>>> f(0)
1
>>> f(1)
12
>>> f(2)
65
>>> f(3)
172
>>> f(4)
297
>>> f(5)
456
>>> f(6)
649
```

どうやら *f*(*n*) = *f*(*n*-1) + (*f*(*n*-1) - *f*(*n*-2) + 34) になっているように見える. [Wolfram|Alpha](https://ja.wolframalpha.com/) に `f(n)=f(n-1)+(f(n-1)-f(n-2)+34), f(3)=172, f(4)=297` を入力したら *f*(*n*) = *n*(17<i>n</i>+6)+1 と教えてくれたので解けた.

```python
m = 1000000007

N = int(input())

if N == 0:
    print(1)
elif N == 1:
    print(12)
elif N == 2:
    print(65)
else:
    print((N * (17 * N + 6) + 1) % m)
```
