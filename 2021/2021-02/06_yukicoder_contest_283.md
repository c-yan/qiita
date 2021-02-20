# yukicoder contest 283 参戦記

## [B 1399 すごろくで世界旅行 (構築)](https://yukicoder.me/problems/no/1399)

D=1 のときは全ての頂点同士を行き来できるようにしないといけないが、それ以外であれば中継点を一つ決めて底と全ての頂点を行き来できるようにすればいい. 開始頂点から1に行き、1から1に行くを繰り返し、1から終了頂点に行くといった感じになる.

```python
V, D = map(int, input().split())

t = [[0] * V for _ in range(V)]
if D == 1:
    t = [[1] * V for _ in range(V)]
else:
    for i in range(V):
        t[0][i] = 1
        t[i][0] = 1
for i in range(V):
    print(*t[i], sep='')
```
