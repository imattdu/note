



# select



### 基础

可以同时判断多个管道是否可以发送、接收



**如果同时有多个管道可以执行，那么随机选择一个**

**如果都有准备好，那么执行default**



也可以判断管道是否存满

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {

	t1 := make(chan int, 10)
	t2 := make(chan int, 10)

	for i := 0; i < 10; i++ {
		t1 <- i
		t2 <- rand.Int()
	}
b1:
	for {
		select {
		case v := <-t1:
			fmt.Println(v)
		case v := <-t2:
			fmt.Println("t2:", v)
		default:
			fmt.Println("else")
			break b1
		}
	}

}

```

