# yukicoder contest 266 参戦記

## [A 1229 ラグビーの得点パターン](https://yukicoder.me/problems/no/1229)

N≦100なので全探索で問題ないので、「コンバージョンはトライ後のキックなので必ずトライ数以下になることに注意してください」だけ注意して全探索すれば良い.

```python
N = int(input())

result = 0
for i in range(N // 5 + 1):
    for j in range(i + 1):
        for k in range(N // 3 + 1):
            if i * 5 + j * 2 + k * 3 == N:
                result += 1
print(result)
```

## [B 1230 Hall_and_me](https://yukicoder.me/problems/no/1230)

すべての確率を計算して最大値を取ればいい. ある宝箱にダイヤモンドが入っている確率を x とすると、STAY の場合は入ってた時に当たるので確率 x、CHANGE の場合は入ってなかった時に当たる(何故なら指定しなかったハズレの宝箱を開けてもらえるので、もし指定した宝箱に入っていなかったら CHANGE 先が確実に入っている宝箱になる)ので確率 1 - x となる.

```python
P, Q, R = map(int, input().split())

a = P / (P + Q + R)
b = Q / (P + Q + R)
c = R / (P + Q + R)

print(max(a, b, c, 1 - a, 1 - b, 1 - c))
```

## [C 1231 Make a Multiple of Ten](https://yukicoder.me/problems/no/1231)

DPの典型問題. それぞれのカードを取る取らないを、カードに書かれた整数の総和を10で割った余りをインデックス、取ったカードの枚数を値とし、取ったカードの枚数の最大値を取る DP をすれば良い.

```python
N, *A = map(int, open(0).read().split())

dp = [-1] * 10
dp[0] = 0
for a in A:
    t = [-1] * 10
    for i in range(10):
        if dp[i] == -1:
            continue
        if t[i] < dp[i]:
            t[i] = dp[i]
        n = (i + a) % 10
        if t[n] < dp[i] + 1:
            t[n] = dp[i] + 1
    dp = t
print(dp[0])
```

## [D 1232 2^x = x](https://yukicoder.me/problems/no/1232)

フェルマーの小定理より a<sup>p-1</sup> ≡ 1 (mod p) なので a<sup>(p-1)<sup>2</sup></sup>≡1 (mod p) なのだが、(p-1)<sup>2</sup> = p<sup>2</sup>-2p+1≡1 (mod p) なので (p-1)<sup>2</sup> が概ね答えになる. フェルマーの小定理にはaとpは互いに素という条件があるので、p=2だけはこの答えが使えないが、2の場合の答えはサンプルにある.

```python
N, *p = map(int, open(0).read().split())

for x in p:
    if x == 2:
        print(2)
    else:
        print((x - 1) * (x - 1))
```
