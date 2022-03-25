+++
title = "jetbrain"
date = 2022-01-20T17:25:20+08:00
featured = false
comment = true
toc = true
reward = true
weight = 9
categories = [
  "tool"
]
tags = [
]
series = [
]
images = []
aliases = [
]

+++

jetbrain

<!--more-->









# 安装



下一步下一步安装即可，安装时选择要安装的组件，注意存储位置







# 配置





## IDEA配置

### 常规配置

#### 视图显示

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108151532.png)



#### 显示菜单字体

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108151642.png)



#### 控制台字体

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210829223306.png)







#### 设置鼠标滚轮修改字体大小

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108151755.png)



#### 设置自动导包

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152055.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152210.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152315.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152413.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152631.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152743.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108152848.png)



忽略大小写

![](https://raw.githubusercontent.com/imattdu/img/main/img/20201228143742.png)



### Git配置

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108153027.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108153153.png)



### maven配置

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108153447.png)





### **注释**

类的注释

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108161320.png)





```
/** 
@author matt 
@create ${YEAR}-${MONTH}-${DAY} ${TIME} 
*/
```

方法的注释



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108161611.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108161502.png)





```
**
 * 功能：
 * @author matt
 * @date $date$
$param$
 * @return $return$
*/
```



```
groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+=' * @param ' + params[i] + ((i < params.size() - 1) ? '\\n':'')}; return result", methodParameters()) 
```



### **注释顶格**

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210109161331.png)





### 取消自动更新

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108153327.png)





### 终端配置



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201227194012.png)



### 插件



![](https://raw.githubusercontent.com/imattdu/img/main/img/202110292244487.png)





### 关闭拼写检查

**Spelling**



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210112143609.png)





### spring

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108162349.png)



复制到idea中的文件需要重构项目否则无法访问404

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210108175428.png)





### junit无法从控制台输入

```
help ->Edit Custom VM Options

-Deditable.java.test.console=true
```



### **properties文件乱码**

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210501112734.png)



**如果还是不起作用删除文件重新创建即可**









## pycharm配置



### pycharm取消波浪线提示



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210106223216.png)





进入波浪线设置界面看看到上方有三个设置项None、Syntax、Inspections，可以拖动箭头设置。

1.None表示没有波浪线；

2.Syntax表示只有语法错误显示波浪线；

3.Inspections表示语法错误和不符合PEP8规范显示波浪线。







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210107001328.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210107001418.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210107001646.png)



# 使用

## IDEA

### 打开一个项目



对于maven项目我们打开选择pom.xml即可

## 常规使用

### 如何重启

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210319231913.png)



## pycharm如何创建一个项目



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201227233243.png)



1.设置新项目名称和存储路径（untitled可以修改）；

2.**Project Interpreter**设置新建项目所依赖的python环境；

​	2.1 **New environment using** 设置新的依赖环境。在项目中新建一个venv（virtualenv）目录，用于存放虚拟的python环境，这里所有的类库依赖都可以直接脱离系统安装的python独立运行；

​		2.1.1 勾选上**Inherit global site-packages**则可以使用base interpreter（基础解释器）中安装的第三方库（即本地Python的**site-packages目录中的类库**）；不选将和外界完全隔离（会在base interpreter的基础上创建一个新的虚拟解释器）；

​		2.1.2 勾选上**Make available to all projects**则可以将此项目的虚拟环境提供给其他项目使用；

​	2.2 **Existing Interpreter**关联已经存在的python解释器，可以使用该解释器所安装的Python库；

建议选择 **New environment using** 可以在Base Interpreter选择系统中安装的Python解释器，这样可以让项目独立部署运行，也可以避免一台服务器部署多个项目之间存在类库的版本依赖问。





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210107004019.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/20210320210246.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210320210538.png)







**压缩包引入到IDEA，help->reset**

**auto -> reset**





## goland



### 配置



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103340.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103319.png)





### 使用goland创建项目



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103117.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103217.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103908.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210521103926.png)















## clion







### 如何 多个main函数

**CMakeLists.txt**

```sh
cmake_minimum_required(VERSION 3.19)
project(demo)

set(CMAKE_CXX_STANDARD 14)

# 遍历项目根目录下所有的 .cpp 文件
file (GLOB_RECURSE  files *.cpp)
foreach (file ${files})
    string(REGEX REPLACE ".+/(.+)/(.+)\\..*" "\\1-\\2" exe ${file})
    add_executable (${exe} ${file})
    message (\ \ \ \ --\ src/${exe}.cpp\ will\ be\ compiled\ to\ bin/${exe})
endforeach ()
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/23/20211223014018.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/23/20211223014406.png)

