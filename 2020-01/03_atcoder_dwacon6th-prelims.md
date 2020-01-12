# AtCoder 第6回 ドワンゴからの挑戦状 予選 参戦記

## A - Falling Asleep

4分で突破. 書くだけ.

```python
N = int(input())

total = 0
p = 0
d = {}
for _ in range(N):
    s, t = input().split()
    t = int(t)
    total += t
    d[s] = total
print(total - d[input()])
```

## B - Fusing Slimes

敗退.
