+++
title = "jetbrain快捷键"
date = 2022-01-20T17:25:20+08:00
featured = true
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
images = [
]
aliases = [
]

+++

jetbrain 快捷键

<!--more-->




## win



CTRL+ALT+ENTER:上衣行

SHIFT+ENTER:下一行



ctrl+d:复制一行

ctrl+x:删除一行，本来是ctrl+y,但是系统冲突



shift+shift：全局搜索





ctrl+h:查看继承关系




CTRL+ALT+B: 查看类的实现




## mac

参考：[mac-idea-keymap](https://www.cnblogs.com/shundong106/p/11141138.html)





### 快捷键组合（编码相关）

|        快捷键组合（编码相关）         |          快捷键作用           |
| :-----------------------------------: | :---------------------------: |
|               command+C               |             复制              |
|               command+X               |        删除一行／剪切         |
|               command+V               |             粘贴              |
|               command+D               |          复制上一行           |
|               command+W               |        关闭当前类窗口         |
|               command+O               |        查找打开类文件         |
|               command+F               |  当前文本查找(可上下键切换)   |
|              command+⇧+F              |           全局查找            |
|               command+R               |         当前文本替换          |
|              command+⇧+R              |           全局替换            |
|               command+N               |  生产代码 getter、setter等等  |
|               command+S               |             保存              |
|               command+Z               |           撤销一步            |
|               command+G               |   定位，配合查找使用更方便    |
|  command+/（注意输入法要在英文状态）  |        注释／解除注释         |
| control+⇧+/（注意输入法要在英文状态） |           注释:/**/           |
|           command+option+L            |          格式化代码           |
|     control+空格（注意默认冲突）      |           代码补全            |
|  control+option+空格（注意默认冲突）  |         智能代码补全          |
|           control+option+O            |        去除无效import         |
|     Command+Option+方向键左 / 右      | 退回 / 前进到上一个操作的地方 |
|        command+鼠标悬停，点击         |    查看方法说明，进入方法     |
|                                       |                               |
|         command + shift + +/-         |      展开/折叠 所有方法       |
|     command + option + shifit + -     |         折叠当前方法          |
|         command + option + +          |         展开当前方法          |



















### 快捷键组合（工具相关）

|  快捷键组合（工具相关）  |      作用       |
| :----------------------: | :-------------: |
|        command+，        | 打开preferences |
| command+回车（选择工程） |  打开resource   |
|           ⇧+F6           |     重命名      |
|           ⇧+F9           |  编译当前工程   |
|       Control + R        |    运行工程     |
|       Control + D        |    debug工程    |





### 快捷键组合（调试相关）

| 快捷键组合（调试相关） |                             作用                             |
| :--------------------: | :----------------------------------------------------------: |
|           F8           |  进入下一步，如果当前行断点是一个方法，则不进入当前方法体内  |
|           F7           | 进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中 |
|       Shift + F7       |   智能步入，断点所在行上有多个方法调用，会弹出进入哪个方法   |
|      Option + F9       |       运行到光标处，如果光标前有其他断点会进入到该断点       |
|  Command + Option + R  |  恢复程序运行，如果该断点下面代码还有断点则停在下一个断点上  |
|      Command + F8      |   切换断点（若光标当前行有断点则取消断点，没有则加上断点）   |
|  Command + Shift + F8  |                          查看断点信                          |
