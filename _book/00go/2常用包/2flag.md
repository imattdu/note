

# flag

## os.Args





```go
package main

import (
    "fmt"
    "os"
)

func main() {

    if len(os.Args) > 0 {
        for index, arg := range os.Args {
            fmt.Println(index, arg)
        }
    }

}
```



```go
go build -o "mflag" mflag.go
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/202204051053687.png)

**os.Args是一个存储命令行参数的字符串切片，它的第一个元素是执行文件的名称。**









## flag

### 支持的类型

flag包支持的命令行参数类型有bool、int、int64、uint、uint64、float float64、string、duration。



| flag参数     | 有效值                                                                              |
|:---------- |:-------------------------------------------------------------------------------- |
| 字符串flag    | 合法字符串                                                                            |
| 整数flag     | 1234、0664、0x1234等类型，也可以是负数。                                                      |
| 浮点数flag    | 合法浮点数                                                                            |
| bool类型flag | 1, 0, t, f, T, F, true, false, TRUE, FALSE, True, False。                         |
| 时间段flag    | 任何合法的时间段字符串。如”300ms”、”-1.5h”、”2h45m”。<br>合法的单位有”ns”、”us” /“µs”、”ms”、”s”、”m”、”h”。 |





### 定义

#### flag.Type()

```go
name := flag.StringVar("name", "jack", "姓名")
```

name:对应类型的指针

#### flag.TypeVar() 推荐使用

```go
flag.StringVar(&name, "name", "jack", "姓名")
flag.IntVar(&age, "age", 17, "年龄")
flag.BoolVar(&married, "married", false, "是否结婚")
flag.DurationVar(&delay, "delay", 0, "时间间隔")
```



### 解析

flag.Parse()



- -flag xxx （使用空格，一个-符号）
- --flag xxx （使用空格，两个-符号）
- -flag=xxx （使用等号，一个-符号）
- --flag=xxx （使用等号，两个-符号）



Flag解析在第一个非flag参数（单个”-“不是flag参数）之前停止





### 其他函数

- flag.Args() ////返回命令行参数后的其他参数，以[]string类型
- flag.NArg() //返回命令行参数后的其他参数个数
- flag.NFlag() //返回使用的命令行参数个数







### 使用



```go
package main

import (
	"flag"
	"fmt"
	"os"
	"time"
)

func main() {

	if len(os.Args) > 0 {
		for index, arg := range os.Args {
			fmt.Println(index, arg)
		}
	}

	var name string
	var age int
	var married bool
	var delay time.Duration

	flag.StringVar(&name, "name", "jack", "姓名")
	flag.IntVar(&age, "age", 17, "年龄")
	flag.BoolVar(&married, "married", false, "是否结婚")
	flag.DurationVar(&delay, "delay", 0, "时间间隔")

	// 解析
	flag.Parse()
	fmt.Println(name, age, married, delay)

	fmt.Println(flag.Args())
	fmt.Println(flag.NArg())
	fmt.Println(flag.NFlag())

}

```





![](https://raw.githubusercontent.com/imattdu/img/main/img/202204051121111.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/202204051124204.png)
