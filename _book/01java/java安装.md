



## jdk



### 安装

下载地址：https://www.oracle.com/java/technologies/downloads/#java8-mac

一步一步安装即可（都是默认无需更改）



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120006306.png)





### 配置

```sh
# if shell
vim .bash_profile
# if zsh
vim .zshrc
```





```sh
JAVA_HOME=/Library/java/JavaVirtualMachines/jdk1.8.0_321.jdk/Contents/Home
PATH=$JAVA_HOME/bin:$PATH:.
CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export JAVA_HOME
export PATH
export CLASSPATH
```





```sh
source .bash_profile
```



### 验证

```sh
java -version


java


javac
```



