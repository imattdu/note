# groutinue



**TODO：**

​	进程线程协程区别 分别是啥

​	底层原理





线程：需要维护线程池，线程的调度去执行任务

协程：go语言已经帮我们实现了，我们只需定义任务即可,让系统将我们的任务分配给cpu执行



```go
go 函数()
```



main 协程结束 整体程序就会结束 不会等待其他协程



### time.Sleep



```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		fmt.Println("start...")
		time.Sleep(time.Second * 10)
		fmt.Println("end...")
	}()

	fmt.Println("main start...")
	time.Sleep(time.Second * 5)
	fmt.Println("main end...")
}

```







### wg



```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func w1() {
	defer wg.Done()
	fmt.Println("wg1")
}


func main() {


	for  i := 0; i < 10; i++ {
		wg.Add(1)
		go w1()
	}
	wg.Wait()
	fmt.Println("main")
}


```



