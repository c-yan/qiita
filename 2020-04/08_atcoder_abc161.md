# AtCoder Beginner Contest 161 参戦記

## [ABC161A - ABC Swap](https://atcoder.jp/contests/abc161/tasks/abc161_a)

3分で突破. 書くだけ. オンラインのコードテストが詰まってて時間がかかってしまった.

```python
X, Y, Z = map(int, input().split())

X, Y = Y, X
X, Z = Z, X
print(X, Y, Z)
```

## [ABC161B - Popular Vote](https://atcoder.jp/contests/abc161/tasks/abc161_b)

4分で突破. 閾値が総投票数の 4 * M 分の一なのでまずそれを求め、その閾値を超える票数の商品がM個以上あるかを調べる. オンラインのコードテストが詰まってて時間がかかってしまった.

```python
N, M = map(int, input().split())
A = list(map(int, input().split()))

threshold = sum(A) / (4 * M)
if len([a for a in A if a >= threshold]) >= M:
    print('Yes')
else:
    print('No')
```

## [ABC161C - Replacing Integer](https://atcoder.jp/contests/abc161/tasks/abc161_c)

6分で突破. N が K を超えていれば、まず剰余を取る. N が K 未満になった後は、適当に回せば収束するだろうと、適当に1000回して放り込んだら AC が出たので結果オーライ. 後で真面目に考え直す.

```python
N, K = map(int, input().split())

result = N
if N > K:
    result = min(result, N % K)
for i in range(1000):
    result = min(result, abs(result - K))
print(result)
```

## [ABC161D - Lunlun Number](https://atcoder.jp/contests/abc161/tasks/abc161_d)

49分で突破. 当然数字を1づつ増やしながらのループでは TLE 必須なのでスキップを考える. ルンルン数は 12 の次が 21 となる. 13は3に問題があるが、3の桁ではなく、1の桁が2になるまでルンルン数は発生しない. 問題が発生した場合は上の桁を一つ進めて、それより下の桁を0にフラッシュしてみる. それだけだと、13→20→30となってしまうので、問題のある桁の値がその一つ前の桁より小さい場合には、問題のある桁の値を一つ進めることにした. これで 13→21 と進むようになり AC. コードは is_lunlun が真偽値ではなく数値を返すやっつけなので後で直す.

```python
K = int(input())


def is_lunlun(i):
    result = -1
    n = [ord(c) - 48 for c in str(i)]
    for j in range(len(n) - 1):
        if abs(n[j] - n[j + 1]) <= 1:
            continue
        if n[j] < n[j + 1]:
            for k in range(j + 1, len(n)):
                n[k] = 0
            result = int(''.join(str(k) for k in n))
            result += 10 ** (len(n) - (j + 1))
        else:
            result = int(''.join(str(k) for k in n))
            result += 10 ** (len(n) - (j + 2))
        break
    return result


i = 1
while True:
    # print(i)
    t = is_lunlun(i)
    if t == -1:
        K -= 1
        if K == 0:
            print(i)
            exit()
        i += 1
    else:
        i = t
```

## [ABC161E - Yutori](https://atcoder.jp/contests/abc161/tasks/abc161_e)

順位表を見るに F のほうが明らかに簡単そうなのでパスした.

## [ABC161F - Division or Substraction](https://atcoder.jp/contests/abc161/tasks/abc161_f)

突破できず.
