# Goでのスタックもただのスライス＋appendで実装できる

[Goでのキューはただのスライス＋appendで実装できる](https://qiita.com/ruiu/items/8edf22cd5fc8f7511687)を見て、スライスでキューを実装できることに気づき、

```go
type intQueue []int

func (queue *intQueue) enqueue(i int) {
	*queue = append(*queue, i)
}

func (queue *intQueue) dequeue() int {
	result := (*queue)[0]
	*queue = (*queue)[1:]
	return result
}
```

みたいなコードを書いて、

```go
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
```

みたいに使っていたのだが、キューもスライスで実装できることに気づいた.

```go
package main

import (
	"fmt"
)

type intStack []int

func (stack *intStack) push(i int) {
	*stack = append(*stack, i)
}

func (stack *intStack) pop() int {
	result := (*stack)[len(*stack)-1]
	*stack = (*stack)[:len(*stack)-1]
	return result
}

func main() {
	var stack intStack = make([]int, 0)
	stack.push(1)
	stack.push(2)
	fmt.Println(stack.pop())
	stack.push(3)
	fmt.Println(stack.pop())
	fmt.Println(stack.pop())
}
```

実行してみた.

```
2
3
1
```

問題なさそう.
