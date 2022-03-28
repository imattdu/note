# runtime





- runtime.Gosched()
- runtime.Goexit()
- runtime.GOMAXPROCS(1)





```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	// 1.15 默认是cpu核心数 无需配置
	runtime.GOMAXPROCS(1)
	go func() {
		for i := 0; i < 10; i++ {
			if i == 5 {
				// 退出当前协程
				runtime.Goexit()
			}
			fmt.Println("son...")
		}
	}()

	for i := 0; i < 2; i++ {
		// 让出CPU时间片，重新等待安排任务
		runtime.Gosched()
		fmt.Println("main...")


	}

}

```

