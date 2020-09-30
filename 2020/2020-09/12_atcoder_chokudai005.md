# AtCoder Chokudai Contest 005 参戦記

参加しなくていいかなあと思いつつ、結局参加してしまうやつ.

結果は49,656,737点で、AC 528人中240位でした. 最終コードは以下で Go 言語で提出しました.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func fill(d [][]byte, i, j int, x, y byte) {
	q := [][2]int{{i, j}}
	for len(q) != 0 {
		m, n := q[0][0], q[0][1]
		q = q[1:]
		d[m][n] = x
		if m > 0 {
			if d[m-1][n] == y {
				q = append(q, [2]int{m - 1, n})
			}
		}
		if m < N-1 {
			if d[m+1][n] == y {
				q = append(q, [2]int{m + 1, n})
			}
		}
		if n > 0 {
			if d[m][n-1] == y {
				q = append(q, [2]int{m, n - 1})
			}
		}
		if n < N-1 {
			if d[m][n+1] == y {
				q = append(q, [2]int{m, n + 1})
			}
		}
	}
}

//
var (
	N int
)

func main() {
	defer flush()

	_ = readInt()
	N = readInt()
	K := readInt()

	S := make([]string, N)
	for i := 0; i < N; i++ {
		S[i] = readString()
	}

	d := make([][]byte, N)
	for i := 0; i < N; i++ {
		d[i] = make([]byte, N)
	}

	result := make([][]string, 0, K)
	for n := 1; n <= K; n++ {
		x := byte(n + '0')
		t := make([]string, 0, 10000)

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				d[i][j] = S[i][j]
			}
		}

		for i := 1; i < N-1; i++ {
			for j := 1; j < N-1; j++ {
				if d[i][j] == x {
					continue
				}

				a := 0
				b := 0
				if d[i-1][j] != d[i][j] {
					a++
					if d[i-1][j] == d[i+1][j] {
						a++
					}
					if d[i-1][j] == d[i][j+1] {
						a++
					}
				} else if d[i][j-1] != d[i][j] {
					b++
					if d[i][j-1] == d[i+1][j] {
						b++
					}
					if d[i][j-1] == d[i][j+1] {
						b++
					}
				}

				if a == 0 && b == 0 {
					continue
				}

				var c byte
				if a >= b {
					c = d[i-1][j]
				} else {
					c = d[i][j-1]
				}
				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, c))
				fill(d, i, j, c, d[i][j])
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] == x {
					continue
				}

				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, x))
				fill(d, i, j, x, d[i][j])
			}
		}
		result = append(result, t)
	}

	best := math.MaxInt64
	for i := 0; i < K; i++ {
		best = min(best, len(result[i]))
	}

	bestIndex := -1
	for i := 0; i < K; i++ {
		if len(result[i]) == best {
			bestIndex = i
			break
		}
	}

	println(best)
	for i := 0; i < best; i++ {
		println(result[bestIndex][i])
	}
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

## 感想、反省点など

- [Introduction to Heuristics Contest](https://atcoder.jp/contests/intro-heuristics) で学んだ局所探索法が全く使えなかったのが残念.
- [Introduction to Heuristics Contest](https://atcoder.jp/contests/intro-heuristics) は貪欲法の部分が悪くて余り点が伸びなかったのだが、今回はそこは結構がんばれた感があってよかった.

## タイムライン

以下は大まかなタイムラインです.

- [21:00] YouTube で動画見てた(ぉぃ).
- [21:07] アァァ! となって急いで https://atcoder.jp/ にアクセスした.
- [21:07] 一応 checker.zip を拾って斜め読みした.
- [21:12] 問題文を読んで、何もしなくても一応 AC はするんだなと思い、以下を提出して5807500点をゲット.

```python
    id, N, K = map(int, input().split())
    S = [input() for _ in range(N)]

    print(0)
```

- [21:25] 取り敢えず最初に一番多い色に塗りつぶすかと思い、以下を提出して49558075点をゲット.

```python
from collections import Counter

id, N, K = map(int, input().split())
S = [input() for _ in range(N)]

c = Counter()
for i in range(N):
    c.update(S[i])
x = c.most_common(1)[0][0]

result = []
for i in range(N):
    for j in range(N):
        if S[i][j] != x:
            result.append('%d %d %s' % (i + 1, j + 1, x))
print(len(result))
print(*result, sep='\n')
```

- [21:37] 左と上が同じ色の場合は既に塗る必要がないなと思い、以下を提出して49649188点をゲット.

```python
from collections import Counter

id, N, K = map(int, input().split())
S = [input() for _ in range(N)]

c = Counter()
for i in range(N):
    c.update(S[i])
x = c.most_common(1)[0][0]

result = []
for i in range(N):
    for j in range(N):
        if S[i][j] != x and (i == 0 or S[i][j] != S[i - 1][j]) and (j == 0 or S[i][j] != S[i][j - 1]):
            result.append('%d %d %s' % (i + 1, j + 1, x))
print(len(result))
print(*result, sep='\n')
```

- [21:42] 別に全色試してもいいんじゃねって気づき、以下を提出して49649427点をゲット. 99位.

```python
id, N, K = map(int, input().split())
S = [input() for _ in range(N)]

result = []
for n in range(1, K + 1):
    x = str(n)
    t = []
    for i in range(N):
        for j in range(N):
            if S[i][j] != x and (i == 0 or S[i][j] != S[i - 1][j]) and (j == 0 or S[i][j] != S[i][j - 1]):
                t.append('%d %d %s' % (i + 1, j + 1, x))
    result.append(t)

best = min(len(result[i]) for i in range(K))
for i in range(K):
    if len(result[i]) == best:
        best_index = i
        break

print(len(result[best_index]))
print(*result[best_index], sep='\n')
```

- [22:13] 塗るシミュレーションをしたいなと思い、Go 言語に移植し完了する.
- [22:22] 塗るシミュレーションを入れたコードを提出し、49654250点をゲット. 184位.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func main() {
	defer flush()

	_ = readInt()
	N := readInt()
	K := readInt()

	S := make([]string, N)
	for i := 0; i < N; i++ {
		S[i] = readString()
	}

	d := make([][]byte, N)
	for i := 0; i < N; i++ {
		d[i] = make([]byte, N)
	}

	result := make([][]string, 0, K)
	for n := 1; n <= K; n++ {
		x := byte(n + '0')
		t := make([]string, 0, 10000)

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				d[i][j] = S[i][j]
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] != x {
					s := fmt.Sprintf("%d %d %c", i+1, j+1, x)
					t = append(t, s)
					y := d[i][j]
					q := [][2]int{[2]int{i, j}}
					for len(q) != 0 {
						m, n := q[0][0], q[0][1]
						q = q[1:]
						d[m][n] = x
						if m > 0 {
							if d[m-1][n] == y {
								q = append(q, [2]int{m - 1, n})
							}
						}
						if m < N-1 {
							if d[m+1][n] == y {
								q = append(q, [2]int{m + 1, n})
							}
						}
						if n > 0 {
							if d[m][n-1] == y {
								q = append(q, [2]int{m, n - 1})
							}
						}
						if n < N-1 {
							if d[m][n+1] == y {
								q = append(q, [2]int{m, n + 1})
							}
						}
					}
				}
			}
		}
		result = append(result, t)
	}

	best := math.MaxInt64
	for i := 0; i < K; i++ {
		best = min(best, len(result[i]))
	}

	bestIndex := -1
	for i := 0; i < K; i++ {
		if len(result[i]) == best {
			bestIndex = i
			break
		}
	}

	println(best)
	for i := 0; i < best; i++ {
		println(result[bestIndex][i])
	}
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

- [23:00] 先に離れている島を繋げておけば、繋がれば1点は増えるし、繋がらなくても損はないと思った. 繋げるパスを追加して提出し、49654387点をゲット. 205位.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func fill(d [][]byte, i, j int, x, y byte) {
	q := [][2]int{{i, j}}
	for len(q) != 0 {
		m, n := q[0][0], q[0][1]
		q = q[1:]
		d[m][n] = x
		if m > 0 {
			if d[m-1][n] == y {
				q = append(q, [2]int{m - 1, n})
			}
		}
		if m < N-1 {
			if d[m+1][n] == y {
				q = append(q, [2]int{m + 1, n})
			}
		}
		if n > 0 {
			if d[m][n-1] == y {
				q = append(q, [2]int{m, n - 1})
			}
		}
		if n < N-1 {
			if d[m][n+1] == y {
				q = append(q, [2]int{m, n + 1})
			}
		}
	}
}

//
var (
	N int
)

func main() {
	defer flush()

	_ = readInt()
	N = readInt()
	K := readInt()

	S := make([]string, N)
	for i := 0; i < N; i++ {
		S[i] = readString()
	}

	d := make([][]byte, N)
	for i := 0; i < N; i++ {
		d[i] = make([]byte, N)
	}

	result := make([][]string, 0, K)
	for n := 1; n <= K; n++ {
		x := byte(n + '0')
		t := make([]string, 0, 10000)

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				d[i][j] = S[i][j]
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] != x {
					if i > 0 && d[i-1][j] != d[i][j] {
						s := fmt.Sprintf("%d %d %c", i+1, j+1, d[i-1][j])
						t = append(t, s)
						fill(d, i, j, d[i-1][j], d[i][j])
					} else if j > 0 && d[i][j-1] != d[i][j] {
						s := fmt.Sprintf("%d %d %c", i+1, j+1, d[i][j-1])
						t = append(t, s)
						fill(d, i, j, d[i][j-1], d[i][j])
					}
				}
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] != x {
					s := fmt.Sprintf("%d %d %c", i+1, j+1, x)
					t = append(t, s)
					fill(d, i, j, x, d[i][j])
				}
			}
		}
		result = append(result, t)
	}

	best := math.MaxInt64
	for i := 0; i < K; i++ {
		best = min(best, len(result[i]))
	}

	bestIndex := -1
	for i := 0; i < K; i++ {
		if len(result[i]) == best {
			bestIndex = i
			break
		}
	}

	println(best)
	for i := 0; i < best; i++ {
		println(result[bestIndex][i])
	}
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

- [23:15] 繋げるパスが上優先で、上が駄目な時に左となっていて、左が得そうな時に損なので、スコアリングして上か左か決めるように書いたが、今見るとバグっていて動作していない. でもなぜか提出したら点が増えて、49656737点をゲット. 221位.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func fill(d [][]byte, i, j int, x, y byte) {
	q := [][2]int{{i, j}}
	for len(q) != 0 {
		m, n := q[0][0], q[0][1]
		q = q[1:]
		d[m][n] = x
		if m > 0 {
			if d[m-1][n] == y {
				q = append(q, [2]int{m - 1, n})
			}
		}
		if m < N-1 {
			if d[m+1][n] == y {
				q = append(q, [2]int{m + 1, n})
			}
		}
		if n > 0 {
			if d[m][n-1] == y {
				q = append(q, [2]int{m, n - 1})
			}
		}
		if n < N-1 {
			if d[m][n+1] == y {
				q = append(q, [2]int{m, n + 1})
			}
		}
	}
}

//
var (
	N int
)

func main() {
	defer flush()

	_ = readInt()
	N = readInt()
	K := readInt()

	S := make([]string, N)
	for i := 0; i < N; i++ {
		S[i] = readString()
	}

	d := make([][]byte, N)
	for i := 0; i < N; i++ {
		d[i] = make([]byte, N)
	}

	result := make([][]string, 0, K)
	for n := 1; n <= K; n++ {
		x := byte(n + '0')
		t := make([]string, 0, 10000)

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				d[i][j] = S[i][j]
			}
		}

		for i := 1; i < N-1; i++ {
			for j := 1; j < N-1; j++ {
				if d[i][j] == x {
					continue
				}

				a := 0
				b := 0
				if d[i-1][j] != d[i][j] {
					a++
					if d[i-1][j] == d[i+1][j] {
						a++
					}
					if d[i-1][j] == d[i][j+1] {
						a++
					}
				} else if d[i][j-1] != d[i][j] {
					b++
					if d[i][j-1] == d[i+1][j] {
						b++
					}
					if d[i][j-1] == d[i][j+1] {
						b++
					}
				}

				if a == 0 && b == 0 {
					continue
				}

				var c byte
				if a >= b {
					c = d[i-1][j]
				} else {
					c = d[i][j-1]
				}
				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, c))
				fill(d, i, j, c, d[i][j])
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] == x {
					continue
				}

				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, x))
				fill(d, i, j, x, d[i][j])
			}
		}
		result = append(result, t)
	}

	best := math.MaxInt64
	for i := 0; i < K; i++ {
		best = min(best, len(result[i]))
	}

	bestIndex := -1
	for i := 0; i < K; i++ {
		if len(result[i]) == best {
			bestIndex = i
			break
		}
	}

	println(best)
	for i := 0; i < best; i++ {
		println(result[bestIndex][i])
	}
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

- [00:07] 上述のバグを直して提出し、49657497点をゲット. バグってなかったら2つ上がって238位だった.

```go
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
)

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func fill(d [][]byte, i, j int, x, y byte) {
	q := [][2]int{{i, j}}
	for len(q) != 0 {
		m, n := q[0][0], q[0][1]
		q = q[1:]
		d[m][n] = x
		if m > 0 {
			if d[m-1][n] == y {
				q = append(q, [2]int{m - 1, n})
			}
		}
		if m < N-1 {
			if d[m+1][n] == y {
				q = append(q, [2]int{m + 1, n})
			}
		}
		if n > 0 {
			if d[m][n-1] == y {
				q = append(q, [2]int{m, n - 1})
			}
		}
		if n < N-1 {
			if d[m][n+1] == y {
				q = append(q, [2]int{m, n + 1})
			}
		}
	}
}

//
var (
	N int
)

func main() {
	defer flush()

	_ = readInt()
	N = readInt()
	K := readInt()

	S := make([]string, N)
	for i := 0; i < N; i++ {
		S[i] = readString()
	}

	d := make([][]byte, N)
	for i := 0; i < N; i++ {
		d[i] = make([]byte, N)
	}

	result := make([][]string, 0, K)
	for n := 1; n <= K; n++ {
		x := byte(n + '0')
		t := make([]string, 0, 10000)

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				d[i][j] = S[i][j]
			}
		}

		for i := 1; i < N-1; i++ {
			for j := 1; j < N-1; j++ {
				if d[i][j] == x {
					continue
				}

				a := 0
				b := 0
				if d[i-1][j] != d[i][j] {
					a++
					if d[i-1][j] == d[i+1][j] {
						a++
					}
					if d[i-1][j] == d[i][j+1] && d[i-1][j] != d[i-1][j+1] {
						a++
					}
				}
				if d[i][j-1] != d[i][j] {
					b++
					if d[i][j-1] == d[i+1][j] && d[i][j-1] != d[i+1][j-1] {
						b++
					}
					if d[i][j-1] == d[i][j+1] {
						b++
					}
				}

				if a == 0 && b == 0 {
					continue
				}

				var c byte
				if a >= b {
					c = d[i-1][j]
				} else {
					c = d[i][j-1]
				}
				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, c))
				fill(d, i, j, c, d[i][j])
			}
		}

		for i := 0; i < N; i++ {
			for j := 0; j < N; j++ {
				if d[i][j] == x {
					continue
				}

				t = append(t, fmt.Sprintf("%d %d %c", i+1, j+1, x))
				fill(d, i, j, x, d[i][j])
			}
		}
		result = append(result, t)
	}

	best := math.MaxInt64
	for i := 0; i < K; i++ {
		best = min(best, len(result[i]))
	}

	bestIndex := -1
	for i := 0; i < K; i++ {
		if len(result[i]) == best {
			bestIndex = i
			break
		}
	}

	println(best)
	for i := 0; i < best; i++ {
		println(result[bestIndex][i])
	}
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
