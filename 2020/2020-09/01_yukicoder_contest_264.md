# yukicoder contest 264 参戦記

## [A 1217 せしすせそ](https://yukicoder.me/problems/no/1217)

ループを回して、一致しないインデックスを求めて、XtoY を出力するだけ.

```python
S = input()

for i in range(26):
    a = chr(i + ord('a'))
    if S[i] == a:
        continue
    print('%sto%s' % (a, S[i]))
    break
```

## [B 1218 Something Like a Theorem](https://yukicoder.me/problems/no/1218)

何も考えずに全探索しても TLE にならなくて、あれってなった.

```python
n, z = map(int, input().split())

for x in range(1, z + 1):
    for y in range(x, z + 1):
        if x ** n + y ** n == z ** n:
            print('Yes')
            exit()
print('No')
```

## [C 1219 Mancala Combo](https://yukicoder.me/problems/no/1219)

操作としては一番左側の A<sub>i</sub>=i を操作してを繰り返すわけだけど、もちろんナイーブにそれをシミュレートする実装をすると TLE になる(なお、heapq で実装して体験しました orz). よくよく考えると、A<sub>i</sub> より右の A<sub>i+1</sub>～A<sub>N</sub> を消した回数だけ積み増されるので、その積み増しを c とした時に c + A<sub>i</sub> が i の倍数になっていればいい. c を積みながら右からスキャンすると *O*(*N*) となって解ける.

```python
N, *A = map(int, open(0).read().split())

c = 0
for i in range(N - 1, -1, -1):
    if (A[i] + c) % (i + 1) != 0:
        print('No')
        break
    c += (A[i] + c) // (i + 1)
else:
    print('Yes')
```
