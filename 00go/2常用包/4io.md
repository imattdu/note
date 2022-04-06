# io





- `os.Stdin`：标准输入的文件实例，类型为`*File`
- `os.Stdout`：标准输出的文件实例，类型为`*File`
- `os.Stderr`：标准错误输出的文件实例，类型为`*File`





- ```
  func Create(name string) (file *File, err Error)
  ```

  - 根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666

- ```
  func NewFile(fd uintptr, name string) *File
  ```

  - 根据文件描述符创建相应的文件，返回一个文件对象

- ```
  func Open(name string) (file *File, err Error)
  ```

  - 只读方式打开一个名称为name的文件

- ```
  func OpenFile(name string, flag int, perm uint32) (file *File, err Error)
  ```

  - 打开名称为name的文件，flag是打开的方式，只读、读写等，perm是权限

- ```
  func (file *File) Write(b []byte) (n int, err Error)
  ```

  - 写入byte类型的信息到文件

- ```
  func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
  ```

  - 在指定位置开始写入byte类型的信息

- ```
  func (file *File) WriteString(s string) (ret int, err Error)
  ```

  - 写入string信息到文件

- ```
  func (file *File) Read(b []byte) (n int, err Error)
  ```

  - 读取数据到b中

- ```
  func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
  ```

  - 从off开始读取数据到b中

- ```
  func Remove(name string) Error
  ```

  - 删除文件名为name的文件





### 打开关闭文件



```go
package main

import (
	"fmt"
	"os"
)

func main() {

	f, err := os.Open("./f.go")
	if err != nil {
		fmt.Println("open file failed! err:", err)
		return
	}
	defer f.Close()
}

```



### 写文件



```go
package main

import (
	"fmt"
	"os"
)

func main() {
	f, err := os.Create("./a.txt")
	if err != nil {
		fmt.Println("create file failed, err = ", err)
		return
	}
	defer f.Close()
	for i := 0; i < 100; i++ {

		f.WriteString("matt")
		f.Write([]byte("abcd"))
	}

}

```



### 读文件



```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {

	//var a1 [10]int = [10]int{1, 2, 3, 4}
	//var a2 []int
	//// 用于一次添加多个
	//a2 = append(a2, a1[:10]...)
	//fmt.Println(a2)

	f, err := os.Open("./m.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer f.Close()
	var buf [128]byte
	var content []byte
	for {
		n, err := f.Read(buf[:])
		if err == io.EOF {
			break
		}

		if err != nil {
			fmt.Println("read file err ", err)
			return
		}

		content = append(content, buf[:n]...)
	}

	fmt.Println(string(content))


}

```



### 拷贝文件

```go
package main

import (
	"fmt"
	"io"
	"os"
)

// 拷贝文件
func main() {

	srcF, err := os.Open("./m.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	dstF, err := os.Create("./m1.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	buf := make([]byte, 1024)
	for {
		n, err := srcF.Read(buf)
		if err == io.EOF {
			break
		}
		if err != nil {
			fmt.Println(err)
			return
		}

		dstF.Write(buf[:n])

	}

	srcF.Close()
	dstF.Close()

}

```





## bufio





| 模式        | 含义     |
| ----------- | -------- |
| os.O_WRONLY | 只写     |
| os.O_CREATE | 创建文件 |
| os.O_RDONLY | 只读     |
| os.O_RDWR   | 读写     |
| os.O_TRUNC  | 清空     |
| os.O_APPEND | 追加     |



```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func w() {
	f, err := os.OpenFile("./m.txt", os.O_CREATE|os.O_WRONLY, 0777)
	if err != nil {
		return
	}
	defer f.Close()

	writer := bufio.NewWriter(f)
	for i := 0; i < 100; i++ {
		writer.WriteString("matt\n")
	}

	writer.Flush()

}

func r() {
	f, err := os.Open("./m.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer f.Close()
	reader := bufio.NewReaderSize(f, 100)
	for {
		line, prefix, err := reader.ReadLine()
		if err == io.EOF {
			break
		}
		if err != nil {
			return
		}
		fmt.Println(string(line), prefix)
	}
}

func main() {
	//w()
	r()
}

```





## ioutil





```go
package main

import (
	"fmt"
	"io/ioutil"
)

func iw()  {
	err := ioutil.WriteFile("m.txt", []byte("www.baidu.com"), 0777)
	if err != nil {
		fmt.Println(err)
		return
	}
}

func ir()  {
	content, err := ioutil.ReadFile("m.txt")
	if err != nil {
		return
	}
	fmt.Println(string(content))
}

func main() {
	//iw()
	ir()
}

```



