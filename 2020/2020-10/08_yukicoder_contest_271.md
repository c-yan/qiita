# yukicoder contest 271 参戦記

## [A 1264 010](https://yukicoder.me/problems/no/1264)

010 は 010 → 101 と1回操作ができ、0100 は 0100 → 1010 → 1101 と2回操作ができ、01000 は 01000 → 10100 → 11010 → 11101 と3回操作できの辺りで答えが分かる.

```python
N = int(input())

print('01' + '0' * N)
```

## [B 1265 Balloon Survival](https://yukicoder.me/problems/no/1265)

N≦1000なので*O*(<i>N</i><sup>2</sup>log<i>N</i>)でもギリギリなんとかなるので、全組み合わせの距離を出して、小さい順に並べて消滅をシミュレートしていけばいい. 既に消滅した風船は記録しておいて、それの衝突が来たらスキップ. 1番目の風船と衝突する風船の数が答え.

```python
N = int(input())
xy = [tuple(map(int, input().split())) for _ in range(N)]

a = []
for i in range(N - 1):
    for j in range(i + 1, N):
        d = (xy[i][0] - xy[j][0]) * (xy[i][0] - xy[j][0]) + (xy[i][1] - xy[j][1]) * (xy[i][1] - xy[j][1])
        a.append((d, i, j))
a.sort()

result = 0
t = set()
for _, i, j in a:
    if i in t or j in t:
        continue
    if i == 0:
        result += 1
        t.add(j)
    else:
        t.add(i)
        t.add(j)
print(result)
```
