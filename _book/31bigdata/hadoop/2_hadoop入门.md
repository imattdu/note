# 1概述

## 1.1hadoop是什么



分布式基础架构

主要解决，海量数据的存储和海量数据的分析计算问题。

3.1.3   

http://hadoop.apache.org/



## 1.2优势





1.高可靠性：Hadoop底层维护多个数据副本，所以即使Hadoop某个计算元
素或存储故障，也不会导致数据的丢失。

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811145502.png)





2.高扩展性：在集群间分配任务数据，可方便的扩展数以千计的节点。

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811145532.png)



3.高效性：在MapReduce的思想下，Hadoop是并行工作的，以加快任务处
理速度。

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811145627.png)



4.高容错性：能够自动将失败的任务重新分配。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811145652.png)





## 1.3hadoop组成



在 Hadoop1.x 时 代 ，Hadoop中的MapReduce同时处理业务逻辑运算和资源的调度，耦合性较大。在Hadoop2.x时代，增加了Yarn。Yarn只负责资源的调度，MapReduce 只负责运算 。Hadoop3.x在组成上没有变化。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811145850.png)







### 1hdfs



1.NameNode（nn）：存储文件的元数据，如文件名，文件目录结构，文件属性（生成时间、副本数、
文件权限），以及每个文件的块列表和块所在的DataNode等。





2.DataNode(dn)：在本地文件系统存储文件块数据，以及块数据的校验和。





3.Secondary NameNode(2nn)：每隔一段时间对NameNode元数据备份。







### 2.yarn

ResourceManager（RM）：整个集群资源（内存、CPU等）的老大

NodeManager（NM）：单个节点服务器资源老大

ApplicationMaster（AM）：单个任务运行的老大

Container：容器，相当一台独立的服务器，里面封装了任务运行所需要的资源，如内存、CPU、磁盘、网络等。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811150925.png)





### 3.mapreduce



MapReduce 将计算过程分为两个阶段：Map 和 Reduce
1）Map 阶段并行处理输入数据
2）Reduce 阶段对 Map 结果进行汇总





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210811150619.png)





### 1.3.4 三者关系

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812003241.png)





# 2.hadoop环境搭建



## 1.虚拟机下安装centos

参考 /note/Ⅰ_linux/Linux安装和基本配置.md

[centos 安装](https://github.com/imattdu/note/blob/main/%E2%85%A0_linux/Linux%E5%AE%89%E8%A3%85%E5%92%8C%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE.md#%E5%AE%89%E8%A3%85centos)



安装centos、设置网络、关闭防火墙设置主机名、安装常见软件包(安装epel-release,jdk,gcc,g++)、设置主机名ip映射、创建matt用户，创建软件目录，修改文件所有者，安装jdk,虚拟机克隆等。



matt05

matt06

matt07



## 2.单机安装hadoop





### 用 XShell 文件传输工具将 hadoop-3.1.3.tar.gz 导入到 opt 目录下面的 software文件夹下面



```go
tar -zxvf hadoop-3.1.3.tar.gz -C /opt/module/


hadoop version
```

### 编写配置path

```go
sudo vim /etc/profile.d/my_env.sh
```

### 配置path

```go
#HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop-3.1.3
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

### 配置生效

```go
source /etc/profile
```

### 验证

```go
hadoop version
```

### **目录结构**









- bin 目录:存放对 Hadoop 相关服务(hdfs，yarn，mapred)进行操作的脚本 
- etc 目录:Hadoop 的配置文件目录，存放 Hadoop 的配置文件
- lib 目录:存放 Hadoop 的本地库(对数据进行压缩解压缩功能)
- sbin 目录:存放启动或停止 Hadoop 相关服务的脚本
- share 目录:存放 Hadoop 的依赖 jar 包、文档、和官方案例





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803084344.png)





# 3.hadoop运行模式



## 3.1三种模式



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803221655.png)



Hadoop 运行模式包括:本地模式、伪分布式模式以及完全分布式模式。



- 本地模式:单机运行，只是用来演示一下官方案例。生产环境不用。
- 伪分布式模式:也是单机运行，但是具备Hadoop集群的所有功能，一台服务器模 拟一个分布式的环境。
- 完全分布式模式:多台服务器组成分布式环境。生产环境使用。



## 3.2本地模式使用





在hadoop安装目录下



```go
mkdir wcinput

cd wcinput

vim hello.txt
```



```go
a a
b c
a d
```

输出目录不可以存在

```go
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount wcinput wcoutput
```



查看结果

```go
cat wcoutput/part-r-00000
```



# 4.完全分布式

## 4.1操作步骤

1. 准备 3 台客户机(关闭防火墙、静态 IP、主机名称)
2. 安装 JDK
3. 配置环境变量
4. 安装 Hadoop
5. 配置环境变量
6. 配置集群
7. 单点启动
8. 配置ssh
9. 群起并测试集群



## 4.2文件传输



### scp

#### 概念

scp 可以实现服务器与服务器之间的数据拷贝。

#### 语法

scp -r $pdir/$fname $user@$host:$pdir/$fname

命令 递归 要拷贝的文件路径/名称 目的地用户@主机:目的地路径/名称



可以指定目标位置的文件名也可以不指定

```go
scp -r /opt/software/aa.txt matt@123.56.135.43:/matt/opt/software
```



```go
scp -r /opt/module/jdk1.8.0_212 matt@192.168.96.132:/opt/module
```

### **rsync** 远程同步工具

#### 优点

rsync 主要用于备份和镜像。具有速度快、避免复制相同内容和支持符号链接的优点。 rsync 和 scp 区别:用 rsync 做文件的复制要比 scp 的速度快，rsync 只对差异文件做更新。scp 是把所有文件都复制过去。



#### 语法

```go
rsync -av $pdir/$fname $user@$host:$pdir/$fname
命令 选项参数 要拷贝的文件路径/名称 目的地用户@主机:目的地路径/名称
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812191004.png)



```
a --archive  ：归档模式，表示递归传输并保持文件属性。等同于"-rtopgDl"。
```



#### 例子

```go
rsync -av /home/matt/hello.txt matt@192.168.96.135:/home/matt
```



### xsync分发集群脚本

底层使用rxync



#### 进入当前家目录的bin目录下

```go
cd ~/bin
vim xsync
```

#### 编写脚本

**记得修改ip**

```go
#!/bin/bash

#1. 判断参数个数
if [ $# -lt 1 ]
then
	echo Not Enough Arguement!
	exit;
fi

#2. 遍历集群所有机器
for host in matt05 matt06 matt07
do
	echo ==================== $host ====================
	#3. 遍历所有目录，挨个发送
	for file in $@
	do
		#4. 判断文件是否存在
		if [ -e $file ]
			then
				#5. 获取父目录
				pdir=$(cd -P $(dirname $file); pwd)

                #6. 获取当前文件的名称
                fname=$(basename $file)
                ssh $host "mkdir -p $pdir"
                rsync -av $pdir/$fname $host:$pdir
			else
				echo $file does not exists!
		fi
	done
done
```

#### 赋予执行权限

```go
chmod u+x xsync
```

#### 判断当前用户家目录，是否在path下，如果没有则需要添加





```go
echo $PATH
```



```go
vim /etc/profile.d/my_env.sh
```



#### 脚本复制到bin目录下

使用sudo需要使用全路径。不要使用相对路径

```go
sudo cp /home/matt/bin/xsync /bin
```



#### 分发脚本环境变量生效

```sh
sudo xsync /home/matt/bin/xsync /bin/xsync /etc/profile.d/my_env.sh
```



```go
source /etc/profile
```



### ssh

ssh 用户名@ip

```go
ssh root@123.56.135.43
```

### 免密登录



```go
cd ~/.ssh

ls -a
```

#### 生成公钥私钥

```go
ssh-keygen -t rsa
```

然后敲(三个回车)，就会生成两个文件 id_rsa(私钥)、id_rsa.pub(公钥)



#### 将公钥拷贝到要免密登录的目标机器上

```go
ssh-copy-id -i id_rsa.pub matt@192.168.96.128
```

#### .ssh 目录下的文件解释

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812194239.png)



**a给b公钥 a就可以免密登录b**



分别配置matt,root用户免密登录其他服务器





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812195053.png)









## 4.3集群部署



### 集群规划

NameNode和SecondaryNameNode不要安装在同一台服务器

ResourceManager也很消耗内存，不要和NameNode、SecondaryNameNode配置在同一台机器上



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812195403.png)



### 配置文件说明

Hadoop 配置文件分两类:默认配置文件和自定义配置文件，只有用户想修改某一默认配置值时，才需要修改自定义配置文件，更改相应属性值。



#### 默认配置文件

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812195628.png)



#### 自定义配置文件

**core-site.xml**、**hdfs-site.xml**、**yarn-site.xml**、**mapred-site.xml** 四个配置文件存放在 $HADOOP_HOME/etc/hadoop 这个路径上，用户可以根据项目需求重新进行修改配置。



### 配置集群

#### 1.核心配置文件-matt05

配置 core-site.xml



```cpp
vim core-site.xml
```





```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- 指定 NameNode 的地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://matt05:8020</value>
    </property>
    <!-- 指定 hadoop 数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop-3.1.3/data</value>
    </property>
    <!-- 配置 HDFS 网页登录使用的静态用户为 matt -->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>matt</value>
    </property>
</configuration>
```

#### 2.HDFS 配置文件-matt05

配置 hdfs-site.xml

```go
vim hdfs-site.xml
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- nn web 端访问地址-->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>matt05:9870</value>
    </property>
    <!-- 2nn web 端访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>matt07:9868</value>
    </property>
</configuration>
```

#### 3.YARN 配置文件-matt05



```go
vim yarn-site.xml
```



```xml
<configuration>
    <!-- 指定 MR 走 shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <!-- 指定 ResourceManager 的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>matt06</value>
    </property>
    <!-- 环境变量的继承 -->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>         <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

#### 4.MapReduce 配置文件-matt05

```go
vim mapred-site.xml
```





```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- 指定 MapReduce 程序运行在 Yarn 上 -->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

#### 分发配置文件



```go
xsync /opt/module/hadoop-3.1.3/etc/hadoop/
```





### 群起

#### 配置workers

```go
vim /opt/module/hadoop-3.1.3/etc/hadoop/workers
```

添加如下内容

```go
matt05
matt06
matt07
```

分发配置文件



```go
xsync /opt/module/hadoop-3.1.3/etc
```

#### 启动集群

##### 1.如果集群是第一次启动，需要在 matt05 节点格式化 NameNode(**注意:格式化 NameNode，会产生新的集群 id，导致 NameNode 和 DataNode 的集群 id 不一致，集群找 不到已往数据。如果集群在运行过程中报错，需要重新格式化 NameNode 的话，一定要先停 止 namenode 和 datanode 程，并且要删除所有机器的 data 和 logs 目录，然后再进行格式化。**)



```go
hdfs namenode -format
```



##### 2.启动hdfs



```go
sbin/start-dfs.sh
```

##### 3.在配置了 **ResourceManager** 的节点(**matt06**)启动 YARN

**yarn:必须在matt06开启关闭**



```go
sbin/start-yarn.sh
```

##### 4.Web 端查看 HDFS 的 NameNode

(a)浏览器中输入:http://matt05:9870

(b)查看 HDFS 上存储的数据信息 

##### 5.Web 端查看 YARN 的 ResourceManager

(a)浏览器中输入:http://matt06:8088

(b)查看 YARN 上运行的 Job 信息



### 测试配置是否成功

创建文件夹并上传

```go
hadoop fs -mkdir /input

hadoop fs -put $HADOOP_HOME/wcinput/word.txt /input
```

hadoop文件存储路径

```go
/opt/module/hadoop-3.1.3/data/dfs/data/current/BP-1436128598- 192.168.10.102-1610603650062/current/finalized/subdir0/subdir0
```

下载文件

```go
hadoop fs -get /jdk-8u212-linux- x64.tar.gz ./
```



```go
[matt@matt05 subdir0]$ cat blk_1073741825
hello word
hello matt
tencent 
[matt@matt05 subdir0]$ pwd
/opt/module/hadoop-3.1.3/data/dfs/data/current/BP-1324950131-192.168.96.135-1628037446040/current/finalized/subdir0/subdir0
[matt@matt05 subdir0]$
```



```go
[atguigu@hadoop102 subdir0]$ cat blk_1073741836>>tmp.tar.gz
[atguigu@hadoop102 subdir0]$ cat blk_1073741837>>tmp.tar.gz
[atguigu@hadoop102 subdir0]$ tar -zxvf tmp.tar.gz
```

执行jar包

```go
hadoop jar /opt/module/hadoop-3.1.3/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount /input /output
```







### 配置历史服务器

可以查看历史运行情况

##### 编写配置文件

```go
vim mapred-site.xml
```



```xml
    <!-- 历史服务器端地址 -->
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>matt05:10020</value>
    </property>
    <!-- 历史服务器 web 端地址 -->
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>matt05:19888</value>
    </property>
```



#### 分发配置

```go
xsync $HADOOP_HOME/etc/hadoop/mapred-site.xml
```





#### matt05启动历史服务器

$HADOOP_HOME/bin

```go
[matt@matt05 bin]$ mapred --daemon start historyserver
```

#### 测试

```go
jps
```



### 日志聚集

日志聚集概念：应用运行完成以后，将程序运行日志信息上传到 HDFS 系统上。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210814094318.png)











#### 编写配置文件matt05

```go
vim yarn-site.xml
```





```xml
    <!-- 开启日志聚集功能 -->
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <!-- 设置日志聚集服务器地址 -->
    <property>
        <name>yarn.log.server.url</name>
        <value>http://matt05:19888/jobhistory/logs</value>
    </property>
    <!-- 设置日志保留时间为 7 天 -->
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>604800</value>
    </property>
```



#### 分发配置文件

```go
xsync $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

#### 关闭 NodeManager 、ResourceManager 和 HistoryServer

```go
# matt06服务器
sbin/stop-yarn.sh
# matt05服务器
mapred --daemon stop historyserver
```

#### 启动 NodeManager 、ResourceManage 和 HistoryServer



```go
./start-yarn.sh
mapred --daemon start historyserver
```

#### 删除 HDFS 上已经存在的输出文件



```go
hadoop fs -rm -r /output
```

#### 执行wordcount程序

```go
hadoop jar /opt/module/hadoop-3.1.3/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount /input /output
```

## 启动

### 整体

#### 整体启动/停止 HDFS

```go
start-dfs.sh/stop-dfs.sh
```

#### 整体启动/停止 YARN

```go
start-yarn.sh/stop-yarn.sh
```

### 各个服务组件逐一启动/停止

#### 分别启动/停止 HDFS 组件

```go
hdfs --daemon start/stop namenode/datanode/secondarynamenode
```



#### 启动/停止 YARN

```go
yarn --daemon start/stop resourcemanager/nodemanager
```



# **matt用户下启动**



## 脚本

### myhadoop.sh



```go
cd ~/bin


vim myhadoop.sh
```





```go
#!/bin/bash
if [ $# -lt 1 ]
then
	echo "No Args Input..."
	exit ;
fi

case $1 in
"start")
	echo " =================== 启动 hadoop 集群 ==================="
	
	echo " --------------- 启动 hdfs ---------------"
	ssh matt05 "/opt/module/hadoop-3.1.3/sbin/start-dfs.sh"
	echo " --------------- 启动 yarn ---------------"
	ssh matt06 "/opt/module/hadoop-3.1.3/sbin/start-yarn.sh"
	echo " --------------- 启动 historyserver ---------------"
	ssh matt05 "/opt/module/hadoop-3.1.3/bin/mapred --daemon start historyserver"
;;
"stop")
	echo " =================== 关闭 hadoop 集群 ==================="

	echo " --------------- 关闭 historyserver ---------------"
	ssh matt05 "/opt/module/hadoop-3.1.3/bin/mapred --daemon stop historyserver"
	echo " --------------- 关闭 yarn ---------------"
	ssh matt06 "/opt/module/hadoop-3.1.3/sbin/stop-yarn.sh"
	echo " --------------- 关闭 hdfs ---------------"
	ssh matt05 "/opt/module/hadoop-3.1.3/sbin/stop-dfs.sh"
;;
*)
	echo "Input Args Error..."
;;
esac
```





```go
chmod 777 myhadoop.sh
```





#### 赋予执行权限

```go
chmod 777 xsync
```

#### 判断当前用户家目录，是否在path下，如果没有则需要添加



```go
echo $PATH
```



```go
vim /etc/profile.d/my_env.sh
```



#### 脚本复制到bin目录下

使用sudo需要使用全路径。不要使用相对路径

```go
sudo cp /home/matt/bin/xsync /bin
```



#### 分发脚本环境变量生效

```sh
sudo xsync /home/matt/bin/xsync /bin/xsync /etc/profile.d/my_env.sh
```



```go
source /etc/profile
```





### JPSALL

```go
vim jpsall
```



```bash
#!/bin/bash

for host in matt05 matt06 matt07
do
	echo =============== $host ===============
	ssh $host jps
done
```





```go
chmod 777 jpsall
```







## 常用端口号 







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805080820.png)





常用配置文件

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805081001.png)









## 时间同步

### 需求



如果服务器在公网环境（能连接外网），可以不采用集群时间同步，因为服务器会定期
和公网时间进行校准；
如果服务器在内网环境，必须要配置集群时间同步，否则时间久了，会产生时间偏差，
导致集群执行任务时间不同步



找一个机器，作为时间服务器，所有的机器与这台集群时间进行定时的同步，生产环境
根据任务对时间的准确程度要求周期同步。测试环境为了尽快看到效果，采用 1 分钟同步一
次。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210814094540.png)



### 具体步骤

#### 查看所有节点 ntpd 服务状态和开机自启动状态

```go
sudo su root


systemctl status ntpd

systemctl start ntpd


systemctl is-enabled ntpd
```



#### 修改 hadoop102 的 ntp.conf 配置文件

```go
vim /etc/ntp.conf
```

#### 修改 1（授权 192.168.96.0-192.168.10.255 网段上的所有机器可以从这台机器上查

询和同步时间）

```go
estrict 192.168.96.0 mask 255.255.255.0 nomodify notrap
```

#### 修改 2（集群在局域网中，不使用其他互联网上的时间）

```go
#server 0.centos.pool.ntp.org iburst
#server 1.centos.pool.ntp.org iburst
#server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst
```



#### 添加 3（当该节点丢失网络连接，依然可以采用本地时间作为时间服务器为集群中

的其他节点提供时间同步）



```go
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```



#### 修改 matt05 的/etc/sysconfig/ntpd 文件





```go
vim /etc/sysconfig/ntpd
```

#### 增加内容如下（让硬件时间与系统时间一起同步）

```go
SYNC_HWCLOCK=yes
```





##### 重新启动 ntpd 服务



```go
systemctl start ntpd

systemctl enable ntpd
```

#### **其他机器配置**root用户下

##### 关闭所有节点上 ntp 服务和自启动



```go
systemctl stop ntpd


systemctl disable ntpd
```



##### 在其他机器配置 1 分钟与时间服务器同步一次

```go
crontab -e
```



##### 编写定时任务如下：

```go
*/1 * * * * /usr/sbin/ntpdate matt05
```





##### 测试

```go
date -s "2021-9-11 11:11:11"


date
```









多台服务器



文件块大小：128m

上限是128m 如果是1kb的文件那么剩余的空间还可以为其他文件存储







 ![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805231056.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805231852.png)







因为每个datanode节点都有版本号，如果不一致就会启动报错



```go
sbin/stop-dfs.sh
```

删除每个节点的data logs 即可

```go
rm -rvf data
rm -rvf logs
```

格式化

```go
hdfs namenode -format
```

启动

```go
sbin/start-dfs.sh
```







http://matt05:9870/







## 5常见错误



1）防火墙没关闭、或者没有启动 YARN 

INFO client.RMProxy: Connecting to ResourceManager at hadoop108/192.168.10.108:8032  

2）root 用户和 matt05 两个用户启动集群不统一  

3）不识别主机名称

```java
java.net.UnknownHostException: hadoop102: hadoop102 at java.net.InetAddress.getLocalHost(InetAddress.java:1475) at org.apache.hadoop.mapreduce.JobSubmitter.submitJobInternal(Job Submitter.java:146) at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1290) at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1287) at java.security.AccessController.doPrivileged(Native Method) at javax.security.auth.Subject.doAs(Subject.java:415)
```





 解决办法： 

​	（1）在/etc/hosts 文件中添加 192.168.10.102 hadoop102 

​	（2）主机名称不要起 hadoop hadoop000 等特殊名称



4）jps 发现进程已经没有，但是重新启动集群，提示进程已经开启。 原因是在 Linux 的根目录下/tmp 目录中存在启动的进程临时文件，将集群相关进程删 除掉，再重新启动集群。 

5）jps 不生效 

原因：全局变量 hadoop java 没有生效。

解决办法：需要 source /etc/profile 文件。 



6）8088 端口连接不上

 ```bash
 cat /etc/hosts 
 ```



注释掉如下代码

```bash
#127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
#::1 matt05
```



