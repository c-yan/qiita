# Go で int スライスの二分探索

Go 言語でスライスの二分探索をするには sort.Search を使う.

int スライスについては、Python の bisect_left 相当の sort.SearchInts があるのでそれを使えば良い. bisect_right 相当はないので sort.Search で書くことになる.

```go
package main

import (
	"fmt"
	"sort"
)

func bisectLeft(a []int, x int) int {
	return sort.SearchInts(a, x)
}

func bisectRight(a []int, x int) int {
	return sort.Search(len(a), func(i int) bool { return a[i] > x })
}

func main() {
	a := []int{1, 2, 2, 3}
	fmt.Println(bisectLeft(a, 0))
	fmt.Println(bisectRight(a, 0))
	fmt.Println(bisectLeft(a, 2))
	fmt.Println(bisectRight(a, 2))
	fmt.Println(bisectLeft(a, 4))
	fmt.Println(bisectRight(a, 4))
}
```

出力結果

```
0
0
1
3
4
4
```

比較の度に関数呼び出しをするためこの実装はすこぶる遅い. 以下の実装は倍くらい速い.

```go
func bisectLeft(a []int, x int) int {
	lo := 0
	hi := len(a)
	for lo < hi {
		mid := (lo + hi) / 2
		if a[mid] < x {
			lo = mid + 1
		} else {
			hi = mid
		}
	}
	return lo
}

func bisectRight(a []int, x int) int {
	lo := 0
	hi := len(a)
	for lo < hi {
		mid := (lo + hi) / 2
		if x < a[mid] {
			hi = mid
		} else {
			lo = mid + 1
		}
	}
	return lo
}
```
