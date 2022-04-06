# 时间

## 时间类型

time.Time类型表示时间。

time.Now()函数获取当前的时间对象，然后获取时间对象的年月日时分秒等信息

```GO
package main

import (
    "fmt"
    "time"
)

func main() {

    // 1获取当前时间
    t := time.Now()
    fmt.Println(t)

    year := t.Year()
    month := t.Month()
    day := t.Day()
    hour := t.Hour()
    minute := t.Minute()
    second := t.Second()


    fmt.Println(year, month, day, hour, minute, second)


    // 2.获取时间戳
    // s
    timestamp1 := t.Unix()
    // ns
    timestamp2 := t.UnixNano()
    fmt.Println(timestamp1, timestamp2)
    // 时间戳 -> 时间对象
    t1 := time.Unix(timestamp1, 0)
    fmt.Println(t1)

    // 3.时间间隔
    fmt.Println(time.Second, time.Minute)

    // 4.时间操作
    op := time.Now()
    opAdd := op.Add(time.Hour)
    fmt.Println(op, opAdd)

    fmt.Println(op.Sub(opAdd))

    fmt.Println(op.Equal(opAdd), "equal")
    // op 是否在opAdd之前 是：true
    fmt.Println(op.Before(opAdd), "before")
    fmt.Println(op.After(opAdd), "after")

    // 定时器 可参考并发


    fmt.Println("时间格式化--------------")
    // 5 时间格式化
    ft := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(ft.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(ft.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(ft.Format("2006/01/02 15:04"))
    fmt.Println(ft.Format("15:04 2006/01/02"))
    fmt.Println(ft.Format("2006-01-02"))


    // 加载时区
    loc, err := time.LoadLocation("Asia/Shanghai")
    if err != nil {
        fmt.Println(err)
        return
    }
    // 按照指定时区和指定格式解析字符串时间
    pT, err := time.ParseInLocation("2006/01/02 15:04:05", "2018/08/04 14:15:20", loc)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(pT)

}
```

## 时间戳

时间戳是自1970年1月1日（08:00:00GMT）至当前时间的总秒数。它也被称为Unix时间戳（UnixTimestamp）。

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    // 2.获取时间戳
    // s
    timestamp1 := t.Unix()
    // ns
    timestamp2 := t.UnixNano()
    fmt.Println(timestamp1, timestamp2)
    // 时间戳 -> 时间对象
    t1 := time.Unix(timestamp1, 0)
    fmt.Println(t1)



}
```

## 时间间隔

时间间隔

```go
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```

## 时间操作

### add

t1.Add(time.Second)

### sub

返回一个时间段t-u。**如果结果超出了Duration可以表示的最大值/最小值，将返回最大值/最小值。**

要获取时间点t-d（d为Duration），可以使用t.Add(-d)。

time.Sub(time.Second)

### Equal

判断俩个时间是否相等， 和t1 == t2不同，这个方法还会比较地区和时间

t1.Equal(t2)

### Before

### After

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    // 4.时间操作
    op := time.Now()
    opAdd := op.Add(time.Hour)
    fmt.Println(op, opAdd)

    fmt.Println(op.Sub(opAdd))

    fmt.Println(op.Equal(opAdd), "equal")
    // op 是否在opAdd之前 是：true
    fmt.Println(op.Before(opAdd), "before")
    fmt.Println(op.After(opAdd), "after")

    // 定时器 可参考并发




}
```

## 时间格式化

go诞生时间： 2006 0102 15:04

2006 1234

**如果想格式化为12小时方式，需指定PM。**

```go
package main

import (
    "fmt"
    "time"
)

func main() {



    fmt.Println("时间格式化--------------")
    // 5 时间格式化
    ft := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(ft.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(ft.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(ft.Format("2006/01/02 15:04"))
    fmt.Println(ft.Format("15:04 2006/01/02"))
    fmt.Println(ft.Format("2006-01-02"))


    // 加载时区
    loc, err := time.LoadLocation("Asia/Shanghai")
    if err != nil {
        fmt.Println(err)
        return
    }
    // 按照指定时区和指定格式解析字符串时间
    pT, err := time.ParseInLocation("2006/01/02 15:04:05", "2018/08/04 14:15:20", loc)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(pT)

}
```
