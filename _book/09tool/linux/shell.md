



## shell解释器



```bash
sudo cat /etc/shells
```



```bash
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
/bin/tcsh
/bin/csh
```



### sh是bash 的软链接

/bin

```bash
[matt@matt05 bin]$ ll | grep bash
-rwxr-xr-x. 1 root root     964536 4月   1 2020 bash
lrwxrwxrwx. 1 root root         10 8月   3 00:12 bashbug -> bashbug-64
-rwxr-xr-x. 1 root root       6964 4月   1 2020 bashbug-64
lrwxrwxrwx. 1 root root          4 8月   3 00:12 sh -> bas
[matt@matt05 bin]$ ll /bin | grep bash
[matt@matt05 bin]$ ll /bin/ | grep bash
-rwxr-xr-x. 1 root root     964536 4月   1 2020 bash
lrwxrwxrwx. 1 root root         10 8月   3 00:12 bashbug -> bashbug-64
-rwxr-xr-x. 1 root root       6964 4月   1 2020 bashbug-64
lrwxrwxrwx. 1 root root          4 8月   3 00:12 sh -> bas
```



### Centos默认的解析器是bash



## demo



### helloword.sh

#### 代码

```bash
#!/bin/bash

# echo 输出
echo "helloword"
```

#### 运行

1 

```bash
sh helloword.sh
```



```bash
sh /home/matt/stu/helloword.sh
```







2 这种方式需要指定执行权限x

```bash
./helloword.sh
```



```bash
/home/matt/stu/helloword.sh
```







### myadd.sh



```bash
#!/bin/bash

cd /home/matt/stu

touch add.txt
# 追加
echo "hello matt" >> add.txt
```





## 变量



### 系统变量

```bash
$HOME $USER $PWD $SHELL
```





```bash
[matt@matt05 stu]$ echo $SHELL
/bin/bash
[matt@matt05 stu]$ echo $HOME
/home/matt
[matt@matt05 stu]$ echo $USER
matt
[matt@matt05 stu]$ echo $PWD
/home/matt/stu
[matt@matt05 stu]$ 
```





### 自定义变量







```bash
# = 之间不可以有空格
a=1
# 撤销声明
unset a
# 只读
readonly a=1
# 提升为全局变量
export a
```



- 命名规范：数字字母下划线，不能以数字开头
- 等号俩侧不可以有空格
- 默认是字符串，如果有空格则加双引号或者单引号









### $n

$n  （功能描述：n为数字，$0代表该脚本名称，$1-$9代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如${10}）







### $# 输入参数的个数





### #* #@ 所有的输入参数







### #？ 上一条命令是否执行成功 成功是0 失败非0







## 运算符



### $[]    $(())

```bash
echo $[2+3*1]
```





### expr 



```bash
 + , - , \*,  /,  % 
```





```bash
expr 1 + 2
```

**注意：expr运算符间要有空格**





## 条件判断



### 基本语法

[ condition ]（注意condition前后要有空格）

注意：条件非空即为true，[ atguigu ]返回true，[] 返回false。



### 符号

#### 比较符号



```bash
== 字符串比较
```

#### 整数

```bash
// <
-lt 
// <=
-le
// >
-gt
// >=
-ge
// ==
-eq
// !=
-ne
```



#### 文件

```bash
// 读的权限
-r
// 写的权限
-w
// 执行的权限
-x
```



#### 文件类型



```bash
-f 文件存在并且是一个常规的文件（file）
-e 文件存在（existence）		
-d 文件存在并是一个目录（directory）
```



### 案例

```bash
[matt@matt05 stu]$ [ 23 -eq 22 ]
[matt@matt05 stu]$ echo $?
1
[matt@matt05 stu]$ [ 23 -eq 23 ]
[matt@matt05 stu]$ echo $?
0
[matt@matt05 stu]$ 
```



### 注意

**多条件判断（&& 表示前一条命令执行成功时，才执行后一条命令，|| 表示上一条命令执行失败后，才执行下一条命令）**





```bash
[matt@matt05 stu]$ [ -e a.txt ] || echo 'matt'
matt
[matt@matt05 stu]$
```







## 流程控制



### if

```bash
#!/bin/bash

# if后面有空格
# [] 里面各个符号之间的空格
if [ 121 -eq 1 ]; then
    echo 'bb'
elif [ 1 -le 2 ]; then
    echo 'aaa'
fi

if [ 'aaaa' == 'aa' ]; then
    echo '=='
else
    echo '!='
fi

```







### case



```bash
#!/bin/bash

case $1 in
"1")
    echo "1"
# ;; 相当于break    
;;
"2")
    echo "2"
;;
# * 相当于default
*)
    echo default
;;
esac

```







### for





```bash
#!/bin/bash

sum=0
for((i=1;i<10;i++)) do
    sum=$[$sum+$i]
done
echo $sum

```











```bash
#!/bin/bash

for i in $*
# do写在下一行
do
    echo "134343 $i"
done

for i in $@
do
    echo "@@ $i"
done


for i in "$*"
do
    echo "$i"

done

for j in "$@"
do
    echo "$j"
done

```







```bash
[matt@matt05 stu]$ sh for2.sh  2 3
134343 2
134343 3
@@ 2
@@ 3
2 3
2
3
```





$*和$@都表示传递给函数或脚本的所有参数，不被双引号“”包含时，都以$1 $2 …$n的形式输出所有参数



当它们被双引号“”包含时，“$*”会将所有的参数作为一个整体，以“$1 $2 …$n”的形式输出所有参数；“$@”会将各个参数分开，以“$1” “$2”…”$n”的形式输出所有参数。







### while



```bash
#!/bin/bash


i=1
while [ $i -lt 10 ]
do
    echo $i
    i=$[$i+1]
done

```









## read





```bash
#!/bin/bash

# -t 写在前面 3s 没输入就会终止
read -t 3 -p "3 s内please your name" name
echo $name

```





## 函数







### basename

```bash
# .sh 参数是可选的 他是去除后缀
basename /home/matt/stu/for2.sh .sh



for2
```





### dirname



```bash
[matt@matt05 stu]$ dirname /home/matt/stu/for2.txt
/home/matt/stu
```







### 自定义函数





```bash
#!/bin/bash

function sum()
{
    s=0
    s=$[$1+$2]
    echo $s
    $?
}
# 需要指定参数
sum $1 $2
if [ 0 -eq 0 ]
then
    echo 111
fi
```



**函数返回值，只能通过$?系统变量获得，可以显示加：return返回，如果不加，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)**







空格分开 第一列数据

```bash
cut -d " " -f 1 cut.txt



cut -d " " -f 2,3 cut.txt 
```







```bash
[matt@matt05 stu]$ sed "2a matt siry" sed.txt 
dong shen
guan zhen
matt siry
wo  wo
lai  lai

le  le
```







```bash
[matt@matt05 stu]$ sed "/do/d" sed.txt
guan zhen
wo  wo
lai  lai

le  le


```





```bash
# /g 全局替换
[matt@matt05 stu]$ sed "s/wo/ma/g" sed.txt 
dong shen
guan zhen
ma  ma
lai  lai

le  le

```





删除第二行

```bash
[matt@matt05 stu]$ sed -e '2d' -e 's/wo/ni/g' sed.txt 
dong shen
ni  ni
lai  lai

le  le


```





第7列

```bash
[matt@matt05 awk]$ awk -F ':' '/^root/{print $7}' passwd 
/bin/bash
```





```bash
[matt@matt05 awk]$ awk -F ":" '/^root/{print $1,$7}' passwd 
root /bin/bash
```





```sh
awk -F ':' 'BEGIN{print "user,matt"} {print $1"."$7} END{print "matt"}' passwd
```





```sh
awk -F ':' -v i=1 '{print $3+i}' passwd
```



这三个变量不能在BEGIN中使用

```sh
awk -F ':' '{print FILENAME} {print NR"-"NF}' passwd
```





```sh
ifconfig ens33 | awk -F : '{print $1}'
```









```bash
sort -t : -nrk 3 sort.txt
```

