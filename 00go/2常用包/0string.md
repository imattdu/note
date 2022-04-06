字符串常用的函数

#### 1.len():统计字符串字节的长度

一个中文三个字节

```go
var str string = "hello 中文"
fmt.Println(len(str))
```

#### 2.字符串遍历，根据字符进行遍历

```go
str1 := []rune(str)
for i := 0; i < len(str1); i++ {
    fmt.Printf("%c", str1[i])
}
```

#### 3.字符串转整数

```go
str2 := "1111111"
var i int64
// bitsize指定8， 如果是3333 则返回128
i, _ = strconv.ParseInt(str2, 10, 64)
fmt.Println(i)

str3 := "123"
//var k int
k, err := strconv.Atoi(str3)
if err != nil {
    fmt.Println("转换错误")
} else {
    fmt.Println(k)
}
```

#### 4.整数转字符串

```go
var str4 string = strconv.Itoa(11)
fmt.Println(str4)
```

#### 5.字符串 转 []byte: var bytes = []byte("hello go")

```go
var str string = "hello word"
bytes := []byte(str)
fmt.Println(bytes)
```

#### 6.字节转字符串

```go
fmt.Println(string(bytes))
```

#### 7.十进制转任意进制

```go
// 十进制转二进制
fmt.Println(strconv.FormatInt(67,2))
```

#### 8.查找子串是否在指定的字符串中: strings.Contains("seafood", "foo") //true

```go
// strings Contains
fmt.Println(strings.Contains("hello", "ele"))
```

#### 9.统计一个字符串有几个指定的字符串

```go
// strings Count
fmt.Println("strings Count:" ,strings.Count("hello", "e"))
```

#### 10.字符串比较

EqualFold:忽略大小写

```go
// strings.EqualFold
fmt.Println(strings.EqualFold("ab", "Ab"))
fmt.Println("ab" == "Ab")
```

#### 11返回子串第一次在字符串出现的位置

```go
fmt.Println("Index")
fmt.Println(strings.Index("abcdef mcatt", "c"))
fmt.Println(strings.LastIndex("abcdef mcactt", "c"))
```

#### 12返回子串最后一次在字符串出现的位置

```go
fmt.Println(strings.LastIndex("abcdef mcactt", "c"))
```

#### 13将指定的子串替换成 另外一个子串: strings.Replace("go go hello", "go", "go 语言", n) n 可以指 定你希望替换几个，如果 n=-1 表示全部替换

```go
fmt.Println(strings.Replace("aa bb ccbc bb bb ", "bb", "BB", -1))
```

#### 14字符串分割

```go
str := "aabcbebeeee"
strArr := strings.Split(str, "b")
for i := 0; i < len(strArr); i++ {
    fmt.Print(strArr[i])
}
```

#### 15字符串大小写转换

```go
fmt.Println(strings.ToLower("Abc"))
fmt.Println(strings.ToUpper("Abc"))
```

#### 16去除字符串俩边的空格

```go
fmt.Println(strings.TrimSpace("   aa  dfer dfdf    "))
```

#### 17字符串指定的字符去除

```go
fmt.Println(strings.Trim("aabbccaa", "aa"))
```

```go
fmt.Println(strings.TrimLeft("aabbccaa", "aa"))

fmt.Println(strings.TrimRight("aabbcccaa", "aa"))
```

#### 18字符串是否包含某个前缀、后缀

```go
fmt.Println(strings.HasPrefix("aabbcc", "aa"))
fmt.Println(strings.HasSuffix("aabbcc", "aa"))
```






















<script src="https://giscus.app/client.js"
        data-repo="iweiwan/iweiwan.github.io"
        data-repo-id="R_kgDOHC6xSw"
        data-category="Comment"
        data-category-id="DIC_kwDOHC6xS84COTo_"
        data-mapping="pathname"
        data-reactions-enabled="1"
        data-emit-metadata="1"
        data-input-position="top"
        data-theme="light"
        data-lang="zh-CN"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>