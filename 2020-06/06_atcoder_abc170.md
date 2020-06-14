# AtCoder Beginner Contest 170 参戦記

## [ABC170A - Five Variables](https://atcoder.jp/contests/abc170/tasks/abc170_a)

2分で突破. 書くだけ. 最初から x<sub>i</sub> が0の場合はどうすれば良いんだと思ったが、x<sub>i</sub> = i だった、あはは.

```python
x = list(map(int, input().split()))

for i in range(5):
    if x[i] == 0:
        print(i + 1)
        break
```

## [ABC170B - Crane and Turtle](https://atcoder.jp/contests/abc170/tasks/abc170_b)

2分半で突破. 書くだけ. O(1) で書けない辺りが弱い.

```python
X, Y = map(int, input().split())

for i in range(X + 1):
    if i * 2 + (X - i) * 4 == Y:
        print('Yes')
        break
else:
    print('No')
```

## [ABC170C - Forbidden List](https://atcoder.jp/contests/abc170/tasks/abc170_c)

3分で突破. 差の絶対値が同じものが2つあった時に小さい方が優先されることだけ気にして書くだけ. 解説 PDF の「本問が問題Bとして出題されていたなら以下に具体的な実装方法を述べるところですが、問題Cを解かれる方にそのような説明は不要と考え、省略します。」にはウケた.

```python
X, N = map(int, input().split())
p = list(map(int, input().split()))

p = set(p)
for i in range(200):
    if X - i not in p:
        print(X - i)
        break
    if X + i not in p:
        print(X + i)
        break
```

## [ABC170D - Not Divisible](https://atcoder.jp/contests/abc170/tasks/abc170_d)

56分半で突破. RE1、WA3. エラトステネスの篩っぽくやれば計算量が下がるんだって気づくまでが長かった.

```python
N = int(input())
A = list(map(int, input().split()))

d = {}
for a in A:
    d.setdefault(a, 0)
    d[a] += 1

A = sorted(set(A))
t = [True] * (10 ** 6 + 1)
for a in A:
    if d[a] > 1:
        t[a] = False
    for i in range(a + a, 10 ** 6 + 1, a):
        t[i] = False
print(sum(1 for a in A if t[a]))
```

## [ABC170E - Count Median](https://atcoder.jp/contests/abc170/tasks/abc170_e)

突破できず. multiset だよなあと思ったけど、Python にはビルトインで multiset ないし、残り時間も足りなかった……. 自前の Treap をもうちょっと汎用性があるように書き直しておくべきか…….
