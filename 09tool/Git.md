+++
title = "git"
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

git使用

<!--more-->







## 安装

### Mac

### 安装

https://www.git-scm.com/download/mac

下载安装



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120757953.png)



#### 配置



```sh

git config --global user.name "matt"              # 你的名字
git config --global user.email "imattdu@gmail.com"     # 你的邮箱


# 配置git代理
git config --global http.proxy 'http://127.0.0.1:7890/' 
git config --global https.proxy 'http://127.0.0.1:7890/'

# 配置github代理
git config --global http.https://github.com.proxy 'http://127.0.0.1:7890/'

# 取消代理
git config --global --unset http.https://github.com.proxy 'http://127.0.0.1:7890/'

```



可以先安装brew

```sh
brew install git
```





#### 将本地安装的git卸载

```sh
cd ／usr／bin
sudo rm －rf git＊
```







### Windows













## SSH登录

```python
cd ~ # 进入家目录

rm -rvf .ssh # 删除.ssh 目录

ssh-keygen -t rsa -C matt17@qq.com # 运行命令生成.ssh 密钥目录
```



```python
cd .ssh

cat id_rsa.pub
```

复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys

New SSH Key

输入复制的密钥信息









1.failed to push some refs to 'git@github.com:pavi-du/note.git'

问题：自己在远程仓库更改了内容，而本地不知道，直接提交

解决：

```
// git fetch+merge
git pull origin master 
```



## 命令



### 基本

```bash
// 第一下克隆下来
git clone https://github.com/imattdu/note.git
git remote -v
// 设置别名
git remote add origin https://github.com/imattdu/note.git
// 拉取
git pull origin
    
// git add --all
git add --all
    
git commit -m "xxxm"
    
// 推送
//git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master:master
    

```



### 分支

```java
git branch // 查看本地分支

git branch -r // 查看远程分支

git branch -a // 查看本地分支和远程分支
    
git branch test // 创建test分支


git checkout test // 切换test分支

git merge test // 合并test分支
```









### **修改远程仓库地址**

```sh
git remote rm origin

git remote add origin git@github.com:imattdu/studyhdfs.git
```




## 错误

### 由于第一次使用不是git clone 然后直接推送



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210724111931.png)



```java
git pull origin master --allow-unrelated-histories
// 合并俩个独立的仓库
```

IDEA如何第一次创建项目提交

首先在githup中创建项目



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201219202633.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201219202929.png)



之后的俩步

![](https://raw.githubusercontent.com/imattdu/img/main/img/20201219205240.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201219205341.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201219220054.png)





**这里记得和远程仓库的名字相同，如果远程是master,这里也得是master**



之后可能会出现 **Push rejected: Push to origin/master was rejected**



我们输入以下内容即可

```java
git pull origin master --allow-unrelated-histories
// master远程仓库分支名
```

紧接着add,commit,push即可



```
# IntelliJ project files
.idea
*.iml
out
gen

# Compiled class file
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.gitignore
```









```c
git config --global http.https://github.com.proxy https://127.0.0.1:XXXX
git config --global https.https://github.com.proxy https://127.0.0.1:XXXX

git config --global --unset http.proxy
git config --global --unset https.proxy

    
git config --global --unset http.https://github.com.proxy
git config --global --unset https.https://github.com.proxy


git config --global -l


```





git设置代理

```python
git config --global http.proxy 'http://127.0.0.1:10809/' 
git config --global https.proxy 'http://127.0.0.1:10809/'
```

打开设置-》网络设置-》找代理-》端口号



```python
git config --global --unset http.proxy
```





```python
# github设置代理
git config --global http.https://github.com.proxy 'http://127.0.0.1:10809/'

# 取消代理
git config --global --unset http.https://github.com.proxy 'http://127.0.0.1:10809/'

```







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210320000112.png)





查阅了一下资料，发现可以在pull命令后紧接着使用`--allow-unrelated-history`选项来解决问题（该选项可以合并两个独立启动仓库的历史）。



```
git stash #封存修改


$git pull origin master --allow-unrelated-histories

git stash pop #把修改还原

```





```
$ git push <远程主机名> <本地分支名>:<远程分支名>
也就是
$git push origin master:master
提交成功。

```





git stash：备份当前工作区内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前工作区内容保存到Git栈中
git pull：拉取服务器上当前分支代码
git stash pop：从Git栈中读取最近一次保存的内容，恢复工作区相关内容。同时，用户可能进行多次stash操作，需要保证后stash的最先被取到，所以用栈（先进后出）来管理；pop取栈顶的内容并恢复
git stash list：显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
git stash clear：清空Git栈









```go
git clone -b dev 远程仓库地址
// 默认拉取master分支
git clone xxxx
```



```go
git fetch origin dev
git checkout -b dev origin/dev
git pull origin dev:dev
```



### 远程仓库创建-本地仓库创建



```go
// 本地初始化
git init

git add --all
git commit -m ""

// 关联仓库
git remote add origin 远程仓库地址
git remote -v

git push origin master:master
//提示出错，使用下面命令合并即可
// 合并俩个独立的仓库
git pull origin master --allow-unrelated-histories

```



```go
// 创建分支
git branch dev

//切换分支
git checkout dev

git branch -a

// 当前在master分支，合并devf
git merge dev
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812105559.png)











////

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210907232930.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20210907233059.png)







查看冲突文件

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210907233146.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20210907233410.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210907233632.png)













### 换行引发的发文

git config --global core.autocrlf false


https://cnbin.github.io/blog/2015/06/19/git-core-dot-autocrlf-pei-zhi-shuo-ming/
