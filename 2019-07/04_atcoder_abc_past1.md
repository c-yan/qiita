# AtCoder Beginners Contest 過去問チャレンジ1

AtCoder Beginners Selection(以下 abs) を解いたので、AtCoder Beginners Contest (以下 abc)の準備は万端かと思いきや、abs には難易度 ABC 問題しかなく、abc には難易度 DEF の問題が出るようなので、過去問のD以上の問題にチャレンジ.

あと abs を解くときに input() が動かなくてひどい目にあって raw_input() に差し替えたが、Python 3 は input() で大丈夫らしいので、Python 2 から 3 に切り替えた. print に括弧を付けるのがめんどくさいなと思っていたが、map や range が list じゃないことのほうがめんどくさいかもしれない.

## ABC133D - Rain Flows into Dams

山iに降った水の量をRiとすると

```
(R1+R2) / 2 = A1
(R2+R3) / 2 = A2
(R3+R4) / 2 = A3
(R4+R5) / 2 = A3
...
(RN+R1) / 2 = AN
```

になることが分かる.
この時 R1 を求めるのは中学で習う連立一次方程式が分かっていれば特に何も難しいことはない.

```
((R1+R2) / 2) - ((R2+R3) / 2) + ((R3+R4) / 2) - ((R4+R5) / 2) + ... + ((RN+R1) / 2) = A1 - A2 + A3 - A4 + ... + AN
R1 = A1 - A2 + A3 - A4 + ... + AN
```

`(Ri+Ri+1) / 2 = Ai` から `Ri+1 = 2 * Ai - Ri` なので、R1 一つ分かりさえすれば残りはドミノ倒し的に分かるので、後はこれを Python のコードに落とすだけ.

```python
n = int(input())
a = [int(e) for e in input().split()]
result = [sum(a[::2])-sum(a[1::2])]
for i in range(len(a) - 1):
  result.append(2*a[i]-result[i])
print(' '.join(map(str, result)))
```

## ABC132D - Blue and Red Balls

再帰的に定式化は出来たものの遅い. 再帰関数の切り札 memoize や、小さい数字の部分は一発で返るようにして高速化したものの、TLE は一つも減らずに敗退.

```python
n, k = [int(e) for e in input().split()]
divisor = 10 ** 9 + 7
cache = {}
def memoize(f):
    global cache
    def _(*args):
        if not args in cache:
            cache[args] = f(*args)
        return cache[args]
    return _
@memoize
def f(n, x):
  if n == 0:
    return 1
  elif n == 1:
    return x
  elif x == 1:
    return 1
  elif x == 2:
    return n + 1
  elif x == 3:
    return (n + 1) * (n + 2) // 2
  else:
    return sum(f(i, x - 1) for i in range(n + 1)) % divisor
for i in range(1, k + 1):
  print(f(k - i, i) * f(n - k - i + 1, i + 1) % divisor)
```

## ABC131D - Megalomania

締め切り時間でソートして、順番に制限時間を超過しないか確認していくだけ. かんたんすぎる感じで、D 問題って難易度に波があるなあという感想.

```python
import sys
from functools import cmp_to_key
n = int(input())
data = [[int(j) for j in input().split()] for i in range(n)]
t = 0
data.sort(key = cmp_to_key(lambda x, y: x[1] - y[1]))
for d in data:
  t += d[0]
  if t > d[1]:
    print('No')
    sys.exit()
print('Yes')
```
## ABC130D - Enough Array

素直に書き下ろしてみたが、予想通り TLE が出た. ウインドウをずらしながら計算するのって20年近く前に大学で習ったよなあと書き直したら通った. 要するに A[i..j] で初めて K を越えたなら A[i+1...j-1] は絶対 K 未満なので、この区間をチェックするだけ無駄なのだ. なので ai+1 を先頭とする部分列は、A[i+1..j] からチェックを開始すれば良い.

```python
n, k = [int(e) for e in input().split()]
data = [int(e) for e in input().split()]
result = 0
i = 0
j = 0
v = 0
while True:
  v += data[j]
  if v < k:
    j += 1
  else:
    result += len(data) - j
    v -= data[i]
    if j > i:
      v -= data[j]
    i += 1
    if j < i:
      j += 1
  if j == len(data):
    print(result)
    break
```
