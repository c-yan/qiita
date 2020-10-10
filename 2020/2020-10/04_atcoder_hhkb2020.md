# AtCoder HHKB プログラミングコンテスト 2020 参戦記

## [HHKB2020A - Keyboard](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_a)

2分で突破. 書くだけ.

```python
S = input()
T = input()

if S == 'Y':
    print(T.upper())
elif S == 'N':
    print(T)
```

## [HHKB2020B - Futon](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_b)

3分で突破. 書くだけ.

```python
H, W = map(int, input().split())
S = [input() for _ in range(H)]

result = 0
for h in range(H):
    for w in range(W - 1):
        if S[h][w] == '.' and S[h][w + 1] == '.':
            result += 1
for h in range(H - 1):
    for w in range(W):
        if S[h][w] == '.' and S[h + 1][w] == '.':
            result += 1
print(result)
```

## [HHKB2020C - Neq Min](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_c)

6分半で突破. 制限からして *O*(*N*) にしないとダメそうだなあと思いつつ、一目でその方法が見えなくて悩んだ記憶がある割にそれほど時間がかかってないのが不思議. 毎行0から考え直していたら*O*(1)になる気がしなかったので、前の行の結果は受け継ぐとして、単調増加だからと考えた辺りでわかった.

```python
N, *p = map(int, open(0).read().split())

result = []
t = 0
s = set()
for x in p:
    s.add(x)
    while t in s:
        t += 1
    result.append(t)
print(*result, sep='\n')
```

## [HHKB2020D - Squares](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_d)

30分くらい悩んだけど、端の処理が難しくてどうにも解けなかった. 問題文を読んだ感触でも、順位表でもEのほうが簡単そうだったのでそっちへ.

## [HHKB2020E - Lamps](https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_e)

60分くらい悩んだけど、交差している部分の処理が難しくてどうにも解けなかった.

追記: 各マスごとに灯りがついていないパターン数を引いていくのか. なるほどなあ. それが分かれば実装は難しくなかった.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

const (
	m = 1000000007
)

func main() {
	defer flush()

	H := readInt()
	W := readInt()
	S := make([]string, H)
	for i := 0; i < H; i++ {
		S[i] = readString()
	}

	K := H * W
	for i := 0; i < H; i++ {
		for j := 0; j < W; j++ {
			if S[i][j] == '#' {
				K--
			}
		}
	}

	yoko := make([][]int, H)
	tate := make([][]int, H)
	for i := 0; i < H; i++ {
		yoko[i] = make([]int, W)
		tate[i] = make([]int, W)
	}

	for i := 0; i < H; i++ {
		s := 0
		l := 0
		for j := 0; j < W; j++ {
			if S[i][j] == '#' {
				for k := s; k < j; k++ {
					yoko[i][k] = l
				}
				s = j + 1
				l = 0
			} else if S[i][j] == '.' {
				l++
			}
		}
		for k := s; k < W; k++ {
			yoko[i][k] = l
		}
	}

	for i := 0; i < W; i++ {
		s := 0
		l := 0
		for j := 0; j < H; j++ {
			if S[j][i] == '#' {
				for k := s; k < j; k++ {
					tate[k][i] = l
				}
				s = j + 1
				l = 0
			} else if S[j][i] == '.' {
				l++
			}
		}
		for k := s; k < H; k++ {
			tate[k][i] = l
		}
	}

	t := make([]int, K+1)
	t[0] = 1
	for i := 1; i < K+1; i++ {
		t[i] = t[i-1] * 2
		t[i] %= m
	}

	c := 0
	for i := 0; i < H; i++ {
		for j := 0; j < W; j++ {
			if S[i][j] == '#' {
				continue
			}
			c += t[K-tate[i][j]-yoko[i][j]+1]
			c %= m
		}
	}

	result := K * t[K]
	result %= m
	result -= c
	result += m
	result %= m
	println(result)
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

var stdoutWriter = bufio.NewWriter(os.Stdout)

func flush() {
	stdoutWriter.Flush()
}

func println(args ...interface{}) (int, error) {
	return fmt.Fprintln(stdoutWriter, args...)
}
```
