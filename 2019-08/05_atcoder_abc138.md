# AtCoder Beginner Contest 138 参戦記

## ABC138A - Red or Not

2分で突破. 書くだけ.

```python
a = int(input())
s = input()
if a >= 3200:
  print(s)
else:
  print('red')
```

## ABC138B - Resistors in Parallel

4分で突破. 書くだけ.

```python
n = int(input())
a = [int(e) for e in input().split()]
v = 0
for i in range(n):
  v += 1 / a[i]
print(1 / v)
```

## ABC138C - Alchemist

3分半で突破. 書くだけ. 過去の経験からも sort が絡んだC問題ってだいたい簡単だよなあ.

```python
n = int(input())
v = [int(e) for e in input().split()]
v.sort()
result = v[0]
for i in range(1, n):
  result = (result + v[i]) / 2
print(result)
```

## ABC138D - Ki

74分半で突破. 最初は真面目に Node クラスを書いていたが14テストケース中 TLE 8個で諦め. Node のフィールドをリストにバラしたけど TLE 8個のまま!? x<sub>j</sub> を取得するたびに子孫に足し込むのをやめても TLE 7個!? 絶望に陥りつつ、ここまで PyPy 3 で流していたので、Python 3 で流したらなんと TLE 1個!? 慌ててローカル変数最適化だけして流したら見事 AC. 4回目にしてようやくABCD完. PyPy で Python と似たような速度になるのはどういうコードかはだいたい把握できているけど、PyPy のほうが遅くなるパターンはよく分からないなあ.

```python
import sys
sys.setrecursionlimit(1000000)
def main():
  _map = map
  _input = input
  _int = int
  def calc_results(results, pvalue, i):
    results[i] = pvalue + values[i]
    for j in childrens[i]:
      calc_results(results, results[i], j)
  n, q = _map(_int, _input().split())
  values = [0] * (n + 1)
  results = [0] * (n + 1)
  childrens = [[] for _ in range(n + 1)]
  for i in range(n - 1):
    a, b = _map(_int, _input().split())
    childrens[a].append(b)
  for _ in range(q):
    p, x = _map(_int, _input().split())
    values[p] += x
  calc_results(results, 0, 1)
  print(*[results[i] for i in range(1, n + 1)])
main()
```

この問題、コンテスト終了後にテストケースが増えたのは [AtCoder Beginner Contest 138 D - Ki のテストケースがコンテスト後に増えている件について (修正版) - Qiita](https://qiita.com/c-yan/items/887e2c2f410ecec60650) に書いたとおりなのだが、追加テストケースがあってDらしい難易度だなあというのが正直な感想. 追加テストケースが通るようにして、なおかつ再帰関数部分をキュー回しに書き換えたりなどして清書したのが以下.

```python
from collections import deque
def main():
  _map, _input, _int = map, input, int
  n, q = _map(_int, _input().split())
  values = [0] * n
  links = [[] for _ in range(n)]
  for _ in range(n - 1):
    a, b = _map(_int, _input().split())
    links[a - 1].append(b - 1)
    links[b - 1].append(a - 1)
  for _ in range(q):
    p, x = _map(_int, _input().split())
    values[p - 1] += x
  d = deque()
  d.append((0, -1))
  while len(d) != 0:
    i, p = d.popleft()
    for j in links[i]:
      if j == p:
        continue
      values[j] += values[i]
      d.append((j, i))
  print(*values)
main()
```

重要なのが a<sub>i</sub> と b<sub>i</sub> がどっちが根に近い(= 親)かは分からないこと. なので、最後は根から走査して、「部分木に含まれるすべての頂点のカウンターの値に x<sub>j</sub> を足す。」を、親に戻らないように判定しながら(`j == p` がそれ)実施していく. 親の情報を持ち回さず、既に処理したかマーキングしておくだけでも良い. 以下はそのやり方で書いた go 言語の例.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

type intQueue []int

func (queue *intQueue) enqueue(i int) {
	*queue = append(*queue, i)
}

func (queue *intQueue) dequeue() int {
	result := (*queue)[0]
	*queue = (*queue)[1:]
	return result
}

func main() {
	n := readInt()
	q := readInt()

	results := make([]int, n)
	links := make([][]int, n)

	for i := 0; i < n-1; i++ {
		a := readInt() - 1
		b := readInt() - 1
		links[a] = append(links[a], b)
		links[b] = append(links[b], a)
	}

	for i := 0; i < q; i++ {
		p := readInt() - 1
		x := readInt()
		results[p] += x
	}

	processed := make([]bool, n)
	processed[0] = true
	var queue intQueue = make([]int, 0)
	queue.enqueue(0)
	for len(queue) != 0 {
		i := queue.dequeue()
		for _, j := range links[i] {
			if processed[j] {
				continue
			}
			results[j] += results[i]
			processed[j] = true
			queue.enqueue(j)
		}
	}

	printIntln(results...)
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

func printIntln(v ...int) {
	b := make([]byte, 0, 4096)
	for i := 0; i < len(v)-1; i++ {
		b = append(b, strconv.Itoa(v[i])...)
		b = append(b, " "...)
	}
	b = append(b, strconv.Itoa(v[len(v)-1])...)
	fmt.Println(string(b))
}
```

## ABC138E - Strings of Impurity

敗退. まあ、17分しか残ってなかったしね. ナイーブな実装は12分で書けたけど. この問題、自分的には最適化の見通しが完全に立っていたので、あと10分追加で残ってたらと悔しさしかない. D より E のほうが簡単なんて.

ナイーブ版.

```python
import sys
s = input()
t = input()
for c in set(t):
  if s.find(c) == -1:
    print(-1)
    sys.exit()
p = 0
slen = len(s)
for i in range(1, 10 ** 5 + 1):
  for j in range(slen):
    if s[j] == t[p]:
      p += 1
      if p == len(t):
        print((i - 1) * slen + j + 1)
        sys.exit()
```

コンテスト終了後に書いた、LUT 使用高速版. 現在位置とアルファベットから次の位置が一発で引ける LUT を構築すれば、t の長さと同じ回数だけ表を引けば答えが求まる寸法. 次の位置が、次のsの繰り返しにあるかもしれないので、sを2作っつけて、進行方向と逆からスキャンして幾つ後に出現するかを LUT に詰めていくだけ. もちろん、t に出現しないアルファベットは放置で.

```python
import sys
s = input()
t = input()
tset = set(t)
slen = len(s)
if len(tset - set(s)) != 0:
    print(-1)
    sys.exit()
s2t = [i - ord('a') for i in (s + s).encode('ascii')]
lut = [[-1 for _ in range(slen)] for _ in range(26)]
for c in tset:
  b = ord(c) - ord('a')
  j = 0
  for i in range(slen * 2 - 1, -1, -1):
    if s2t[i] == b:
      j = 0
    else:
      j += 1
    if i < slen:
      lut[b][i] = j
p = 0
for c in t:
  b = ord(c) - ord('a')
  p += lut[b][p % slen] + 1
print(p)
```
