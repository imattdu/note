



# sync





### sync.WaitGroup



sync.WaitGroup内部维护着一个计数器，计数器的值可以增加和减少。

Add(n):+n

Done:-1

Wait:等待计数器为0



**注意sync.WaitGroup是一个结构体，传递的时候要传递指针。**



```go
package main

import (
	"fmt"
	"sync"
)

var (
	wg1 sync.WaitGroup
)

func t1() {
	fmt.Println("t1...")
	// 计数器-2
	defer wg1.Done()
}

func main() {
	// 计数器+2
	wg1.Add(2)
	go add()
	go add()
	// 让main协程等待wg计数器为0在执行
	wg1.Wait()
}

```



### sync.Once

只执行一次





sync.Once其实内部包含一个互斥锁和一个布尔值，互斥锁保证布尔值和数据的安全，而布尔值用来记录初始化是否完成。这样设计就能保证初始化操作的时候是并发安全的并且初始化操作也不会被执行多次。



```go
package main

import (
	"fmt"
	"sync"
)

var (
	so sync.Once
	m  map[int]string
)

func a2() {
	m = make(map[int]string)
}

func a1() {
	// 不使用 so 通过判断 m != nil 可能没有初始化完成但是 != nil
	// 使用协程又会有安全问题
	so.Do(a2)
	fmt.Println("this is good")
}

func main() {
  go a1()
  go a1()
	
}

```





### sync.Map



系统内置的map不安全



```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	m1 := sync.Map{}

  // 添加
	m1.Store("name", "matt")
	m1.Store("age", 22)

  // 获取
	fmt.Println(m1.Load("name"))

  // 存储或者更新
	v, ok := m1.LoadOrStore("name", "matt")
	if ok {
		fmt.Println(v)
	}

  // 删除
	m1.Delete("name")

	// 存在返回false
	v, ok = m1.LoadOrStore("name", "aaa")
	if !ok {
		fmt.Println(v, "2222load and store")
	}

	//m1.Delete("age")

	// 不存在返回 false
	v, ok = m1.Load("ccc")
	fmt.Println(v, ok)

  // 遍历
	m1.Range(func(k, v interface{}) bool {
		fmt.Println(k, v)
		return true
	})




}

```

