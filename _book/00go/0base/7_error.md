



# 异常处理





### panic recover

panic:抛出异常

recover:捕获异常



**只对运行时异常进行捕获，编译异常不会捕获**



recover：一般用于捕获一个协程的packing行为，捕获panic， 如果同时有多个那么只对最后一个异常进行捕获



recover只有在延迟(defer)调用才有效，不可以直接调用 



推荐recover写在panic之前



```go
func main() {
	//t1()
	// 只对运行时异常进行捕获
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()

	var ch chan int = make(chan int, 10)
	close(ch)
	ch <- 1

	fmt.Println("hello word")


}
```



### error

也可以通过返回error 





```go
package main

import (
	"errors"
	"fmt"
)

func t4(i int) error {
	if i == 0 {
		return errors.New("this is 自定义错误")
	}
	return nil
}

func t5() error {

	return fmt.Errorf("this is 自定义错误")
}

func main() {
	i := 1
	fmt.Scanf("%d", &i)
	err := t4(i)
	fmt.Println(err)

}

```





```go

this is 自定义错误
```






















```go

package main

import "fmt"

type AError struct {
	name string
}

func (e *AError) Error(str string) string {
	e.name = str
	return e.name
}

func main() {
	var i int
	fmt.Scanf("%d", &i)
	if i < 0 {
		e := &AError{}
		fmt.Println(e.Error("aaaa"))
	}
}
```







### 自定义error





```go
package main

import "fmt"

type MyError struct {
	Name string
	Id   int
}

// 写 Error 方法
func (p *MyError) Error() string {
	return fmt.Sprintf("name=%s, id=%d", p.Name, p.Id)
}

func s1(i int) error {
	if i > 0 {
		return nil
	}
	return &MyError{
		Name: "e1",
		Id:   11,
	}
}

func main() {
	i := 1
	fmt.Scanf("%d", &i)
	fmt.Println(s1(i))

}

```

