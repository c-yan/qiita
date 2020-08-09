# AtCoder Grand Contest 047 参戦記

レーティング1200以上の人しかレーティングに関係ないんだから、0完でも問題あるまいと思っていたが、パフォ915で憤死した. 以前に0完したときはパフォ339だったのでそれに比べればマシだけど.

## [AGC047A - Integer Product](https://atcoder.jp/contests/agc047/tasks/agc047_a)

突破できず. Twitter で 10<sup>9</sup> を掛けて、2と5が何個含まれるかを調べて云々と言っている人がいて、その人の説明は分からない部分があったものの閃いた. ようするに10進数なので、2と5が足りればいいのだと. 入力例で出てくる 2.4 だが、当然10倍すれば整数になるが、1.25倍でも整数になる. 要するに 2<sup>2</sup>×3×5<sup>-1</sup> なので 5<sup>-1</sup> さえ打ち消してもらえれば、2<sup>-2</sup> までは大丈夫なのである. これが分かれば後は A<sub>i</sub> 毎に2と5が何乗なのかを求めて、組み合わせ毎に2と5の乗数の合計がそれぞれ0以上であれば整数であると分かる. 2と5の組み合わせの総数はそこまで多くなく、*O*(<i>N</i><sup>2</sup>) でも間に合う.

```python
def conv(s):
    if s.find('.') == -1:
        s += '.'
    s = s.rstrip('0')
    t = len(s) - s.find('.') - 1
    a = int(s.replace('.', ''))
    if a == 0:
        return (0, 0)
    x, y = -t, -t
    while a % 5 == 0:
        x += 1
        a //= 5
    while a % 2 == 0:
        y += 1
        a //= 2
    return (x, y)


N, *A = open(0).read().split()
N = int(N)

d = {}
for a in A:
    t = conv(a)
    d.setdefault(t, 0)
    d[t] += 1

result = 0
xs = list(d.keys())
for i in range(len(xs)):
    x, y = xs[i]
    t = d[xs[i]]
    if x >= 0 and y >= 0:
        result += t * (t - 1) // 2
    for j in range(i + 1, len(xs)):
        m, n = xs[j]
        if x + m >= 0 and y + n >= 0:
            result += t * d[xs[j]]
print(result)
```
