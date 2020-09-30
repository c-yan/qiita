# AtCoder Beginner Contest 170 参戦記

## [ABC170A - Five Variables](https://atcoder.jp/contests/abc170/tasks/abc170_a)

2分で突破. 書くだけ. 最初から x<sub>i</sub> が0の場合はどうすれば良いんだと思ったが、x<sub>i</sub> = i だった、あはは.

```python
x = list(map(int, input().split()))

for i in range(5):
    if x[i] == 0:
        print(i + 1)
        break
```

## [ABC170B - Crane and Turtle](https://atcoder.jp/contests/abc170/tasks/abc170_b)

2分半で突破. 書くだけ. O(1) で書けない辺りが弱い.

```python
X, Y = map(int, input().split())

for i in range(X + 1):
    if i * 2 + (X - i) * 4 == Y:
        print('Yes')
        break
else:
    print('No')
```

## [ABC170C - Forbidden List](https://atcoder.jp/contests/abc170/tasks/abc170_c)

3分で突破. 差の絶対値が同じものが2つあった時に小さい方が優先されることだけ気にして書くだけ. 解説 PDF の「本問が問題Bとして出題されていたなら以下に具体的な実装方法を述べるところですが、問題Cを解かれる方にそのような説明は不要と考え、省略します。」にはウケた.

```python
X, N = map(int, input().split())
p = list(map(int, input().split()))

p = set(p)
for i in range(200):
    if X - i not in p:
        print(X - i)
        break
    if X + i not in p:
        print(X + i)
        break
```

## [ABC170D - Not Divisible](https://atcoder.jp/contests/abc170/tasks/abc170_d)

56分半で突破. RE1、WA3. エラトステネスの篩っぽくやれば計算量が下がるんだって気づくまでが長かった.

```python
N = int(input())
A = list(map(int, input().split()))

d = {}
for a in A:
    d.setdefault(a, 0)
    d[a] += 1

A = sorted(set(A))
t = [True] * (10 ** 6 + 1)
for a in A:
    if d[a] > 1:
        t[a] = False
    for i in range(a + a, 10 ** 6 + 1, a):
        t[i] = False
print(sum(1 for a in A if t[a]))
```

## [ABC170E - Count Median](https://atcoder.jp/contests/abc170/tasks/abc170_e)

突破できず. multiset だよなあと思ったけど、Python にはビルトインで multiset ないし、残り時間も足りなかった……. 自前の Treap をもうちょっと汎用性があるように書き直しておくべきか…….

追記: Python で書いたら TLE になったので、仕方がなく C++ で書いた. まあ、multiset があれば特に難しいところはないよね…….

```c++
#include <bits/stdc++.h>
#define rep(i, a) for (int i = (int)0; i < (int)a; ++i)
using namespace std;

int main()
{
    int N, Q;
    cin >> N >> Q;

    multiset<int> max_ratings;
    vector<multiset<int>> kindergartens(2 * pow(10, 5) + 1);
    vector<int> ratings(N + 1), belongs(N + 1);

    rep(i, N) {
        int A, B;
        cin >> A >> B;
        ratings[i + 1] = A;
        belongs[i + 1] = B;
        kindergartens[B].insert(A);
    }

    rep(i, 2 * pow(10, 5) + 1) {
        if (kindergartens[i].size() != 0) {
            max_ratings.insert(*kindergartens[i].rbegin());
        }
    }

    rep(_, Q) {
        int C, D;
        cin >> C >> D;
        int b = belongs[C];
        belongs[C] = D;
        max_ratings.erase(max_ratings.find(*kindergartens[b].rbegin()));
        kindergartens[b].erase(ratings[C]);
        if (kindergartens[b].size() != 0) {
            max_ratings.insert(*kindergartens[b].rbegin());
        }
        if (kindergartens[D].size() != 0) {
            max_ratings.erase(max_ratings.find(*kindergartens[D].rbegin()));
        }
        kindergartens[D].insert(ratings[C]);
        max_ratings.insert(*kindergartens[D].rbegin());
        cout << *max_ratings.begin() << endl;
    }

    return 0;
}
```

go でも通した.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

var (
	y uint = 88172645463325252
)

func xorshift() uint {
	y ^= y << 7
	y ^= y >> 9
	return y
}

type treapNode struct {
	value    int
	priority uint
	count    int
	left     *treapNode
	right    *treapNode
}

func newTreapNode(v int) *treapNode {
	return &treapNode{v, xorshift(), 1, nil, nil}
}

func treapRotateRight(n *treapNode) *treapNode {
	l := n.left
	n.left = l.right
	l.right = n
	return l
}

func treapRotateLeft(n *treapNode) *treapNode {
	r := n.right
	n.right = r.left
	r.left = n
	return r
}

func treapInsert(n *treapNode, v int) *treapNode {
	if n == nil {
		return newTreapNode(v)
	}
	if n.value == v {
		n.count++
		return n
	}
	if n.value > v {
		n.left = treapInsert(n.left, v)
		if n.priority > n.left.priority {
			n = treapRotateRight(n)
		}
	} else {
		n.right = treapInsert(n.right, v)
		if n.priority > n.right.priority {
			n = treapRotateLeft(n)
		}
	}
	return n
}

func treapDelete(n *treapNode, v int) *treapNode {
	if n == nil {
		panic("node is not found!")
	}
	if n.value > v {
		n.left = treapDelete(n.left, v)
		return n
	}
	if n.value < v {
		n.right = treapDelete(n.right, v)
		return n
	}

	// n.value == v
	if n.count > 1 {
		n.count--
		return n
	}

	if n.left == nil && n.right == nil {
		return nil
	}

	if n.left == nil {
		n = treapRotateLeft(n)
	} else if n.right == nil {
		n = treapRotateRight(n)
	} else {
		// n.left != nil && n.right != nil
		if n.left.priority < n.right.priority {
			n = treapRotateRight(n)
		} else {
			n = treapRotateLeft(n)
		}
	}
	return treapDelete(n, v)
}

func treapCount(n *treapNode) int {
	if n == nil {
		return 0
	}
	return n.count + treapCount(n.left) + treapCount(n.right)
}

func treapString(n *treapNode) string {
	if n == nil {
		return ""
	}
	result := make([]string, 0)
	if n.left != nil {
		result = append(result, treapString(n.left))
	}
	result = append(result, fmt.Sprintf("%d:%d", n.value, n.count))
	if n.right != nil {
		result = append(result, treapString(n.right))
	}
	return strings.Join(result, " ")
}

func treapMin(n *treapNode) int {
	if n.left != nil {
		return treapMin(n.left)
	}
	return n.value
}

func treapMax(n *treapNode) int {
	if n.right != nil {
		return treapMax(n.right)
	}
	return n.value
}

type treap struct {
	root *treapNode
	size int
}

func (t *treap) Insert(v int) {
	t.root = treapInsert(t.root, v)
	t.size++
}

func (t *treap) Delete(v int) {
	t.root = treapDelete(t.root, v)
	t.size--
}

func (t *treap) String() string {
	return treapString(t.root)
}

func (t *treap) Count() int {
	return t.size
}

func (t *treap) Min() int {
	return treapMin(t.root)
}

func (t *treap) Max() int {
	return treapMax(t.root)
}

func main() {
	defer flush()

	N := readInt()
	Q := readInt()

	maxRatings := &treap{}
	kindergartens := make([]*treap, 200001)
	for i := 0; i < 200001; i++ {
		kindergartens[i] = &treap{}
	}
	ratings := make([]int, N+1)
	belongs := make([]int, N+1)

	for i := 1; i < N+1; i++ {
		A := readInt()
		B := readInt()
		ratings[i] = A
		belongs[i] = B
		kindergartens[B].Insert(A)
	}

	for i := 1; i < 200001; i++ {
		if kindergartens[i].Count() != 0 {
			maxRatings.Insert(kindergartens[i].Max())
		}
	}

	for i := 0; i < Q; i++ {
		C := readInt()
		D := readInt()

		from := kindergartens[belongs[C]]
		to := kindergartens[D]
		rating := ratings[C]
		belongs[C] = D

		maxRatings.Delete(from.Max())
		from.Delete(rating)
		if from.Count() != 0 {
			maxRatings.Insert(from.Max())
		}

		if to.Count() != 0 {
			maxRatings.Delete(to.Max())
		}
		to.Insert(rating)
		maxRatings.Insert(to.Max())

		println(maxRatings.Min())
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
