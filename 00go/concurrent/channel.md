

# channel



### 声明+初始化



```go
package main

import "fmt"

func main() {

  // 需要make 
	var t1 chan int = make(chan int, 5)
  // var t1 = make(chan int, 5)
  // 写入
	t1 <- 11
  // 读取
	fmt.Println(<- t1)
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





