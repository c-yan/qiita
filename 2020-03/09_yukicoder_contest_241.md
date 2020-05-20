# yukicoder contest 241 参戦記

## [A 1009 面積の求め方](https://yukicoder.me/problems/no/1009)

8分半で突破. ヒント通り区分求積法でスパッと解いてみた. 区間をどれくらいの数で割ればいいのかに悩んだが、適当にえいやで4096でやったら一発目で良さげな精度が出たので、提出してみたら無事 AC.

```python
a, b = map(int, input().split())

x = a
result = 0
t = 1 / 4096
while x < b:
    result += abs((x - a) * (x - b) * t)
    x += t
print(result)
```

## [B 1010 折って重ねて](https://yukicoder.me/problems/no/1010)

41分で突破. 整数で処理するコードを書いて、なんで AC しないんだと延々と悩んでいた. アホすぎる. 縦と横で短い方を限界まで折ってから、長い方を限界まで折ればいい.

```python
x, y, h = map(int, input().split())

if x < y:
    x, y = y, x

x *= 1000
y *= 1000

result = 0

while y > h:
    y /= 2
    h *= 2
    result += 1

while x > h:
    x /= 2
    h *= 2
    result += 1

print(result)
```

整数で処理することもできる. 相対的に数字が正しく変わればいいのだ.

```python
x, y, h = map(int, input().split())

if x < y:
    x, y = y, x

x *= 1000
y *= 1000

result = 0

while y > h:
    x *= 2
    h *= 4
    result += 1

while x > h:
    y *= 2
    h *= 4
    result += 1

print(result)
```

## [C 1011 Infinite Stairs](https://yukicoder.me/problems/no/1011)

敗退. 素直に DP すると *O*(*N*<sup>2</sup>d) なので TLE する. よくよく考えると、i 段目にたどり着くのは i-d .. i-1 段目、i + 1 段目にたどり着くのは i - d + 1 .. i と端の2箇所以外は同じである. であれば、*O*(*d*) ではなく *O*(1) で処理できるので *O*(*N*<sup>2</sup>) になり解けた.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	N := readInt()
	d := readInt()
	K := readInt()

	buf0 := make([]int, d*N+K+1)
	buf1 := make([]int, d*N+K+1)
	buf0[0] = 1
	for i := 0; i < N; i++ {
		t := 0
		for j := 0; j < d; j++ {
			t += buf0[j]
			t %= 1000000007
			buf1[j] = t
		}
		for j := d; j < (i+1)*d; j++ {
			t -= buf0[j-d]
			if t < 0 {
				t += 1000000007
			}
			t += buf0[j]
			t %= 1000000007
			buf1[j] = t
		}
		buf0, buf1 = buf1, buf0
	}
	fmt.Println(buf0[K-N])
}

const (
	ioBufferSize = 1 * 1024 * 1024 // 1 MB
)

var stdinScanner = func() *bufio.Scanner {
	result := bufio.NewScanner(os.Stdin)
	result.Buffer(make([]byte, ioBufferSize), ioBufferSize)
	result.Split(bufio.ScanWords)
	return result
}()

func readString() string {
	stdinScanner.Scan()
	return stdinScanner.Text()
}

func readInt() int {
	result, err := strconv.Atoi(readString())
	if err != nil {
		panic(err)
	}
	return result
}
```

```python
# PyPy なら AC
N, d, K = map(int, input().split())

buf0 = [0] * (d * N + K + 1)
buf1 = [0] * (d * N + K + 1)
buf0[0] = 1

for i in range(N):
    t = 0
    for j in range(d):
        t += buf0[j]
        t %= 1000000007
        buf1[j] = t
    for j in range(d, (i + 1) * d):
        t -= buf0[j - d]
        t += buf0[j]
        t %= 1000000007
        buf1[j] = t
    buf0, buf1 = buf1, buf0

print(buf0[K - N])
```
