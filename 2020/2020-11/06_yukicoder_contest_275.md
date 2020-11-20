# yukicoder contest 275 参戦記

## [A 1291 小手調べ](https://yukicoder.me/problems/no/1291)

答えは0以上の整数なので、1桁にトラップがあるので注意. d<sub>i</sub>≦100 なので、素直に整数で答えを計算すると int64 の範囲を超えますね. Python だから何も考えずに計算できるけど.

```python
N, *d = map(int, open(0).read().split())

for i in range(N):
    if d[i] == 1:
        print(10)
    else:
        print(9 * 10 ** (d[i] - 1))
```

まあ、文字列で計算しても難しくはないですが.

```python
N, *d = map(int, open(0).read().split())

for i in range(N):
    if d[i] == 1:
        print(10)
    else:
        print('9' + '0' * (d[i] - 1))
```
