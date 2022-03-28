 

defer压栈

函数退出在弹stack 







![](https://raw.githubusercontent.com/imattdu/img/main/img/202110012022658.png)

  

  

# channel



## 基础知识



### 进程线程



![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061053214.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061055653.png)





### 并发 并行

并发：同一时间段有多个任务在执行

并行：同一时刻有多个任务在执行









## go 协程

go主线程，可以起多个协程





![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061105919.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061101474.png)







```go
func test() {
	for i := 0; i < 10; i++ {
		fmt.Println("HELLO WORD")
		time.Sleep(time.Second)
	}
}

func main()  {

	runtime.GOMAXPROCS(8)
	// 启动一个协程
    // 如果主线程推出了，但是协程还没有执行完毕也会退出得
	go test()
	for i := 0; i < 10; i++ {
		fmt.Println("HELLO")
		time.Sleep(time.Second)
	}
}
```





### MPG

![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061107321.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061107715.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061108510.png)











# channel



## 使用lock



```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	m1 map[int]int = make(map[int]int, 10)
	// 互斥锁
	lock sync.Mutex
)

func test(i int) {
	var res int = 1
	for t := 1; t <= i; t++ {
		res *= t
	}
	lock.Lock()
	m1[i] = res
	lock.Unlock()
}

func main() {
	for i := 1; i <= 200; i++ {
		go test(i)
	}

	time.Sleep(10 * time.Second)
	// 主线程并不知道啥时候结束
	// 目前不知道什么原因为啥会警告
	lock.Lock()
	for index, val := range m1 {
		fmt.Println(index, val)
	}
	lock.Unlock()
}

```





## 使用channel



channel可以理解成有界得一个队列，先进先出





![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061112475.png)







```go
package main

import "fmt"

func main() {
	// 声明并初始化
	var intchan chan int = make(chan int, 3)

	fmt.Println(intchan)

	// 写入
	intchan <- 1
	num := 11
	intchan <- num
	intchan <- 985

	fmt.Println(len(intchan), cap(intchan))

	// 读取
	fmt.Println(<-intchan)
	fmt.Println(<-intchan)
	fmt.Println(len(intchan), "len cap")

}

```



​    

![](https://raw.githubusercontent.com/imattdu/img/main/img/202110061115005.png)





遍历

使用for range遍历需要先关闭

```go
package main

import "fmt"

func main() {
	var intchan chan int = make(chan int, 10)
	intchan <- 1
	intchan <- 2
	intchan <- 3
	intchan <- 4
	intchan <- 5

	close(intchan)

	for v := range intchan {
		fmt.Println("intchan:", v)
	}

}

```







```go
package main

import (
	"fmt"
	"time"
)

// 有自己的阻塞机制
// v , ok := <- intchan
// 不是现在intchan没有值立即报异常
func main() {

	intchan := make(chan int, 10)
	exitchan := make(chan bool, 1)
	go writeData(intchan)
	go readData(intchan, exitchan)
    // 防止主线程立即退出
	for {
		_, ok := <-exitchan
		if !ok {
			break
		}
	}
}

func writeData(intchan chan int) {
	for i := 0; i < 50; i++ {
		intchan <- i
		fmt.Println("写入数据", i)
		time.Sleep(200)
	}
	close(intchan)
}

func readData(intchan chan int, exitchan chan bool) {
	for {
		/*if len(exitchan) != 0 {
			close(intchan)
			for v := range intchan {
				fmt.Println(v)
			}
			close(exitchan)
			break
		}*/
		v, ok := <-intchan
		if !ok {
			break
		} else {
			fmt.Println("读取数据", v)
			time.Sleep(time.Second * 2)
		}
	}
	exitchan <- true
	close(exitchan)
}

```













```go
package main

import (
	"fmt"
)

func main() {

	intchan := make(chan int, 1000)
	primechan := make(chan int, 2000)
	exitchan := make(chan bool, 4)

	go putNum(intchan, 8000)
	for i := 0; i < 4; i++ {
		go primeNum(intchan, primechan, exitchan)
	}

	go func() {
		for i := 0; i < 4; i++ {
			<-exitchan
		}
		close(exitchan)
		close(primechan)
	}()
	fmt.Println(len(primechan))
	for v := range primechan {
		fmt.Println(v)
	}

}

func putNum(intchan chan int, n int) {

	for i := 1; i <= n; i++ {
		intchan <- i
	}
	close(intchan)
}

func primeNum(intchan chan int, primechan chan int, exitchan chan bool) {

	for {
		var num int
		var isSuccess bool
		var isSuShu bool
		num, isSuccess = <-intchan
		if isSuccess {
			isSuShu = true
			for i := 2; i < num; i++ {
				if num%i == 0 {
					isSuShu = false
				}
			}
			if isSuShu {
				primechan <- num
			}
		} else {
			exitchan <- true
			break
		}
	}
}

```

