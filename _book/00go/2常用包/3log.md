# log



## logger

### 使用



**Print系列(Print|Printf|Println）、Fatal系列（Fatal|Fatalf|Fatalln）、和Panic系列（Panic|Panicf|Panicln）**





```go
package main

import (
	"fmt"
	"log"
)

func main() {

	
	v := "this is log"
	log.Print(v)
	log.Println(v)
	log.Printf("普通日志%s", v)
	// Fatal系列函数会在写入日志信息后调用os.Exit(1)
	log.Fatalln("fatal 日志")
	log.Panicln("panic 日志")

}

```





### 配置



**log标准库中的Flags函数会返回标准logger的输出配置，而SetFlags函数用来设置标准logger的输出配置。**





```go
const (
    // 控制输出日志信息的细节，不能控制输出的顺序和格式。
    // 输出的日志在每一项后会有一个冒号分隔：例如2009/01/23 01:23:23.123123 /a/b/c/d.go:23: message
    Ldate         = 1 << iota     // 日期：2009/01/23
    Ltime                         // 时间：01:23:23
    Lmicroseconds                 // 微秒级别的时间：01:23:23.123123（用于增强Ltime位）
    Llongfile                     // 文件全路径名+行号： /a/b/c/d.go:23
    Lshortfile                    // 文件名+行号：d.go:23（会覆盖掉Llongfile）
    LUTC                          // 使用UTC时间
    LstdFlags     = Ldate | Ltime // 标准logger的初始值
)
```

基础配置

配置前缀

配置输出的位置



```go
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {

	logFile, err := os.OpenFile("./imatt.log", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
	if err != nil {
		fmt.Println("open log file failed, err:", err)
		return
	}
	log.SetFlags(log.Llongfile | log.Lmicroseconds | log.Ldate)
	log.SetPrefix("[imatt]")
	log.SetOutput(logFile)
	v := "this is log"
	log.Print(v)
	log.Println(v)
	log.Printf("普通日志%s", v)
	// Fatal系列函数会在写入日志信息后调用os.Exit(1)
	log.Fatalln("fatal 日志")
	log.Panicln("panic 日志")



}

```







### 创建logger



```
    func New(out io.Writer, prefix string, flag int) *Logger
```





```go
package main

import (
	"log"
	"os"
)

func main() {

	logger := log.New(os.Stdout, "<matt>", log.Lshortfile|log.Ldate|log.Ltime)
	logger.Println("this is log")


}

```





## 第三方日志库



**logrus、zap**

