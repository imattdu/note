

```
package main

import "fmt"

// 自己抛出异常
func b(i int) {
	if i < 0 {
		panic("i 不能小于 0")
	}
}

func main() {


	// 1.函数正常返回执行 2.发生异常也会执行
	// 注意：需要在异常前面写defer
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err, "matt")
		}
	}()

	var arr = [3]int{0, 1, 3}
	var i int
	fmt.Scanf("%d", &i)
	// 系统抛出异常
	fmt.Println(1 / arr[i])
}
```



```
package main

import (
	"errors"
	"fmt"
)

func c() (e error) {
	// 自定义error对象
	e = errors.New("aaa")
	return e
}


func main() {
	if e := c(); e != nil {
		fmt.Println("出错了")
	}
}

```


```

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