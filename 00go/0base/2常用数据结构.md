

# 数组

## 介绍

存放多个同一类型的数据

## 使用

### 声明

```go
var arr1 [3]int
```

### 四种初始化方法

```go
var arr1 [3]int = [3]int{1, 2, 3}
fmt.Println(arr1)

var arr2 = [5]int{1, 2, 3, 4, 5}
fmt.Println(arr2)

arr3 := [...]float64{1, 2, 3}
fmt.Println(arr3)

var arr4 = [...]string{1: "aa", 0: "bb"}
fmt.Println(arr4)
```



### 遍历

```go
for index, val := range arr4 
{	
    fmt.Println(index, val)
}
```



```go
for i := 0; i < len(arr4); i++ {	
    fmt.Println(arr4[i])
}
```

## 注意

1.数组一旦声明，其长度不能发生改变；数组创建后，没有赋值，会有默认值

2.go 语言中数组是值类型，而不是引用类型

3.传递参数需要指定数组长度

```go
// [3]int [4]int 认为是不同数据类型
func valueTest(arr [3]int) {	
    arr[0] = 1
}
```





## 二维数组



```go
func main() {	
    var arr = [2][2]int{{1, 2}, {3, 4}}	
    fmt.Println(arr)	
    // 只有第一个可以为...	
    var arr1 = [...][3]int{{1, 2}, {3, 4}}	
    fmt.Println(arr1)
}
```





# 切片



## 基础

### 初始化

数组的切 - make - 直接初始化 - 直接声明

1

```go
var arr = [5]int{1, 2, 3, 45, 5}
slice := arr[2:4]
arr[2] = -1
fmt.Println(slice)
// 元素数量fmt.Println(len(slice))
// 容量fmt.Println(cap(slice))
```

2

```go
var slice1 []int = make([]int, 2, 10)
slice1[0] = 0
slice1[1] = 1
fmt.Println(slice1)
```

3

```go
fmt.Println("方式三")
var slice2 []float64 = []float64{1, 2, 3}
fmt.Println(cap(slice2))
```

4

```go
var s []int
```







```go
type slice struct {     
    ptr *[2]int     
    len int     
    cap int
}
```

引用一个数组

数量

容量





### 遍历



```go
for index, value := range slice2 {	
    fmt.Printf("---%v:%v ", index, value)
}
```

## 使用



### 数组切片分割



```go
var slieceDemo = arr[startIndex: endIndex]
```

包含左边，不包含右边

var slice = arr[0:end] 可以简写 var slice = arr[:end] 

var slice = arr[start:len(arr)] 可以简写： var slice = arr[start:] 

var slice = arr[0:len(arr)] 可以简写: var slice = arr[:]



cap 是一个内置函数，用于统计切片的容量，即最大可以存放多少个元素。



**切片仍然可以切片**



```go
// 元素数量fmt.Println(len(slice))// 容量fmt.Println(cap(slice))
```

### append

用 append 内置函数，可以对切片进行动态追加



```go
var arr = [5]int{1, 2, 3, 4, 5}
var slice []int = arr[:]
fmt.Println(slice)// 返回
slice = append(slice, 10)
fmt.Println(slice)
```

### 切片的拷贝

```go
package mainimport "fmt"
func main() {	
    var slice1 []int = make([]int, 5)	
    var slice2 []int = make([]int, 10)	
    slice1[0] = 1	
    copy(slice2, slice1)	
    fmt.Println(slice2)
}
```



### **注意**

**切片是引用传递**



string底层也是数组，可以使用切片



# 



















# Map使用

## 介绍

key-value 的数据结构，类似于 java 中的 map



key 的类型：bool, 数字，string, 指针, channel , 还可以是只包含前面几个类型的 接口, 结构体, 数组，通常 key 为 int 、string 注意: slice， map 还有 function 不可以，因为这几个没法用 == 来判断
valuetype 的类型和 key 基本一样，通常为: 数字(整数,浮点数),string,map,struct



## 使用

### 声明



```go
var map1 map[int]int
```

1.map 在使用前一定要make(需要分配内存)

2.map 的 key 是不能重复，v 可以重复

map 的 key-value 是无序



### 三种使用方式



```go
var map1 map[int]int
map1 = make(map[int]int, 10)
fmt.Println(map1)
```



```go
map2 := make(map[int]int, 10)
fmt.Println(map2)
```



```go
map3 := map[int]int {
	1: 1,
	2: 2,
}
fmt.Println(map3)
```



### map中的方法

#### 添加和更新



```go
var a = make(map[int]string)
a[0] = "matt"
a[1] = "pony"
a[0] = "jack"
a[100] = "aa"
```

#### 删除



```go
// 删除不存在的 key 也不会出错
delete(a, 1)
```



#### 查找

```go
// val:值 ok:是否找到
val, ok := a[3]
fmt.Println(ok, val)
```

#### 遍历



```go
a1 := make(map[string]string)
a1["1"] = "a"
a1["2"] = "b"
a1["3"] = "c"
a1["4"] = "d"
for k, v := range a1 {
	fmt.Println(k, v)
}
```



#### 长度

```go
fmt.Println(len(a1), "map的长度")
```



## 注意

map是引用类型

**map会自动扩容**

map的 value 经常是struct
