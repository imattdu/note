### 问题

同时有多个协程对共享数据修改，可能会产生问题





### 互斥锁、读写锁



```go
package main

import (
	"fmt"
	"sync"
)

var (
	cnt int
  // 互斥锁
	lock sync.Mutex
  // 读写锁
	rwLock sync.RWMutex
	w sync.WaitGroup

)


func add() {

  // 互斥锁
	for i:= 0; i < 1000; i++ {
		lock.Lock()
		cnt++
		lock.Unlock()
	}
	w.Done()
}

func get() {
	// 读锁
	rwLock.RLock()
	rwLock.RUnlock()
	// 写锁
	rwLock.Lock()
	rwLock.Unlock()
}

func main() {
	// 加锁 2000 不加锁 1220
	w.Add(2)
	go add()
	go add()
	w.Wait()
	fmt.Println(cnt)


}

```





互斥锁：如果一个协程获得互斥锁，其他协程只能等待，如果该协程释放锁，那么会随机环形等待该锁的一个协程





读写锁：如果有一个协程获取读锁，其他协程仍然可以获取读锁，但是不能获取写锁，只能等待；

如果有一个协程获取写锁，其他协程只能等待

