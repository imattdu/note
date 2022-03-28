

# channel



### 声明+初始化



```go
package main

import "fmt"

func main() {

  // 需要make 5：容量
	var t1 chan int = make(chan int, 5)
  // var t1 = make(chan int, 5)
  // 写入
	t1 <- 11
  // 读取
	fmt.Println(<-t1)
  // 关闭
	close(t1)
	t2 := make(chan int)
	go func() {
		fmt.Println(len(t2))
		fmt.Println(cap(t2))
		<- t2
	
	}()
	


}

```



### **注意**



1.对一个关闭的通道再发送值就会导致panic。
2.对一个关闭的通道进行接收会一直获取值直到通道为空。
3.对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
4.关闭一个已经关闭的通道会导致panic。





### 无缓冲管道

不使用协程就会报错，使用协程不会报错



报错

```go
package main

func main() {
	c1 := make(chan int)
	c1 <- 1
	<-c1
}

```

阻塞

![](http://raw.githubusercontent.com/imattdu/img/main/img/202203282205561.png)



使用协程

```go
package main

import "fmt"

func main() {
	//c1 := make(chan int)
	//c1 <- 1
	//<-c1

	c2 := make(chan int)
	go func() {
		c2 <- 1
	}()
	fmt.Println(<-c2)
}

```



### 有缓冲管道





```go
package main

import "fmt"

func main() {
	//c1 := make(chan int)
	//c1 <- 1
	//<-c1

	c2 := make(chan int)
	go func() {
		c2 <- 1
	}()
	fmt.Println(<-c2)

	c3 := make(chan int, 10)
	c3 <- 11
	fmt.Println(len(c3), cap(c3))
}

```









### 读取

俩种读取





```go
package main

import (
	"fmt"
)

func main() {

	t3 := make(chan int, 100)
	for i := 0; i < 1; i++ {
		t3 <- i
	}
	// close(t3)
	<-t3
	_, ok := <-t3
	fmt.Println(ok)

	v, ok := <-t3
	fmt.Println(ok, v)
	close(t3)
	// range 需要先关闭， 使用协程就不用close
	for v := range t3 {
		fmt.Println(v)
	}

	// ok 判断：它会不断从管道读取数据， 管道为空就会报错， 如果关闭就不会报错
	for {
		v, ok := <-t3
		fmt.Println(ok)
		if ok {
			fmt.Println(v)
		} else {
			break
		}
	}

}

```





```go
package main

import (
	"fmt"
	"time"
)

func main() {

	c1 := make(chan int, 3)
	for i := 0; i < 3; i++ {
		c1 <- i
	}
	go func() {
		for v :=  range c1 {
			fmt.Println(v)
		}
	}()
	fmt.Println("hello word")
	time.Sleep(time.Second*1)

}

```







### 单向管道

可以把双向管道赋值给单向管道，比如函数的参数是单向管道

```go
package main

func main() {
	t1 := make(chan<- int, 5)
	t1 <- 1
	// 报错
	//fmt.Println(<-t1)


}

```





### 异常



| channel |  nil  |            非空            |             空             |             满             |            非满            |
| :-----: | :---: | :------------------------: | :------------------------: | :------------------------: | :------------------------: |
|  接收   | 阻塞  |           接收值           |            阻塞            |           接收值           |           接收值           |
|  发送   | 阻塞  |           发送值           |           发送值           |            阻塞            |           发送值           |
|  关闭   | panic | 关闭成功，读完数据返回零值 | 关闭成功，读完数据返回零值 | 关闭成功，读完数据返回零值 | 关闭成功，读完数据返回零值 |





