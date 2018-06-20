# 長さ 0 のスライスとしての nil (Go)

長さ 0 のスライスは`make([]T, 0)`でも作れますが、基本的には`nil`を使えば良いようです.

## 確認コード

```
package main

import (
	"fmt"
)

func testNil(p []byte) {
	fmt.Printf("p: %v\n", p)
	fmt.Printf("len(p): %d\n", len(p))
	fmt.Printf("cap(p): %d\n", cap(p))
	for i := range p {
		fmt.Println("NG: %d\n", i)
	}
	fmt.Printf("p[:]: %v\n", p[:])
	fmt.Printf("append(p, 0): %v\n", append(p, 0))
}

func main() {
	testNil(nil)
}
```

## 確認結果

```
p: []
len(p): 0
cap(p): 0
p[:]: []
append(p, 0): [0]
```
