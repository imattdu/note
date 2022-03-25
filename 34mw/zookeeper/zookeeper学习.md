

# 1.概述

## 1.1概述



Zookeeper 是一个开源的分布式的，为分布式框架提供协调服务的 Apache 项目。



Zookeeper从设计模式角度来理解:是一个基于观察者模式设计的分布式服务管理框架，它负责 存储和管理大家都关心的数据，然后接受观察者的 注册，一旦这些数据的状态发生变化，Zookeeper 就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应。



存储少量数据，一般是配置信息。



## 1.2特点



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210810195011.png)



1.Zookeeper:一个领导者(Leader)，多个跟随者(Follower)组成的集群。 

2.集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。所以Zookeeper适合安装奇数台服务器。

3.全局数据一致:每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。 

4.更新请求顺序执行，来自同一个Client的更新请求按其发送顺序依次执行。 

5.数据更新原子性，一次数据更新要么成功，要么失败。

6.实时性，在一定时间范围内，Client能读到最新数据。



## 1.3数据结构





ZooKeeper 数据模型的结构与 Unix 文件系统很类似，整体上可以看作是一棵树，每个 节点称做一个 ZNode。每一个 ZNode 默认能够存储 1MB 的数据，每个 ZNode 都可以通过 其路径唯一标识。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210810195130.png)











## 1.4应用场景



提供的服务包括:统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下

线、软负载均衡等。





### 统一命名服务

在分布式环境下，经常需要对应用/服 务进行统一命名，便于识别。

例如:IP不容易记住，而域名容易记住。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210810195605.png)



### 统一配置管理

1.分布式环境下，配置文件同步非常常见。

1.1一般要求一个集群中，所有节点的配置信息是一致的，比如 Kafka 集群。 

1.2对配置文件修改后，希望能够快速同步到各个节点上。



2配置管理可交由ZooKeeper实现。

2.1可将配置信息写入ZooKeeper上的一个Znode。

2.2各个客户端服务器监听这个Znode。

2.3一旦Znode中的数据被修改，ZooKeeper将通知 各个客户端服务器。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812100213.png)





### 统一集群管理

1分布式环境中，实时掌握每个节点的状态是必要的。

1.1可根据节点实时状态做出一些调整。



2.ZooKeeper可以实现实时监控节点状态变化

2.1可将节点信息写入ZooKeeper上的一个ZNode。

2.2监听这个ZNode可获取它的实时状态变化。



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812100422.png)





### 服务器动态上下线



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812100642.png)





### 软负载均衡



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812100727.png)



## 1.5下载地址

3.5.7



https://archive.apache.org/dist/zookeeper/zookeeper-3.5.7/









# 2.安装

## 2.1前置条件

需要安装jdk,参考linux 目录下的常用软件安装即可看到jdk的安装。





## 2.2本地安装



### 安装

#### 1.将tar包发送到服务器

```go
scp -r apache-zookeeper-3.5.7-bin.tar.gz root@123.56.135.43:/opt/software
```

#### 2.解压

```go
tar -zxvf apache-zookeeper-3.5.7-bin.tar.gz -C /opt/module/
```

#### 3.改名

```go
mv apache-zookeeper-3.5.7-bin/ zookeeper-3.5.7/
```

### 配置修改



**/tmp存储临时数据，一定周期就会删除**

#### 1.在 zookeeper 安装目录下创建数据目录

```go
mkdir zkData

pwd
```

#### 2.配置文件目录下的zoo_sample.cfg改为zoo.cfg

```go
cd conf
mv zoo_sample.cfg zoo.cfg
```

#### 3.修改配置文件

```go
vim zoo.cfg
```

```go
dataDir=/opt/module/zookeeper-3.5.7/zkData
```

## 2.3集群安装

### 安装



#### 1.将tar包发送到服务器

```go
scp -r apache-zookeeper-3.5.7-bin.tar.gz root@123.56.135.43:/opt/software
```

#### 2.解压

```go
tar -zxvf apache-zookeeper-3.5.7-bin.tar.gz -C /opt/module/
```

#### 3.改名

```go
mv apache-zookeeper-3.5.7-bin/ zookeeper-3.5.7/
```

### 配置

#### 1.在zookeeper安装目录下创建zkData目录

```go
mkdir zkData
```



####  2.在zkData目录下创建一个 myid 的文件

```go
touch myid
```

#### 3.编写myid文件，文件中写该服务器唯一标识符



```go
1
```

#### 4.将该zookeeper安装目录分发到其他服务器



```go
xsync zookeeper-3.5.7
```

#### 5.更改其他服务中zkData中的myid文件中的唯一标识






#### 6.配置文件目录下的zoo_sample.cfg改为zoo.cfg

```go
cd conf
mv zoo_sample.cfg zoo.cfg
```

#### 7.修改配置文件

```go
vim zoo.cfg
```

#### 8.修改数据存储路径配置

```go
dataDir=/opt/module/zookeeper-3.5.7/zkData
```

#### 9.添加集群信息



```go
#######################cluster########################## 
server.2=matt05:2888:3888
server.3=matt06:2888:3888
server.4=matt07:2888:3888
```

#### **配置文件解读**

```go
server.A=B:C:D
```



**A** 是一个数字，表示这个是第几号服务器;集群模式下配置一个文件 myid，这个文件在 dataDir 目录下，这个文件里面有一个数据 就是 A 的值，Zookeeper 启动时读取此文件，拿到里面的数据zoo.cfg 里面的配置信息比 较从而判断到底是哪个 server。

**B** 是这个服务器的地址;
**C** 是这个服务器 Follower 与集群中的 Leader 服务器交换信息的端口;
**D** 是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的Leader，而个端口就是用来执行选举时服务器相互通信的端口。



#### 10.同步配置文件，发送到其他服务器



```go
xsync zoo.cfg
```

### 启动集群脚本



```go
cd /home/matt/bin


vim zk.sh
```






```go
#!/bin/bash

case $1 in
"start"){
	for i in matt05 matt06 matt07
  do
    echo ---------- zookeeper $i 启动 ------------
    ssh $i "/opt/module/zookeeper-3.5.7/bin/zkServer.sh start"
	done 
};;
"stop"){
	for i in matt05 matt06 matt07
  do
    echo ---------- zookeeper $i 停止 ------------
    ssh $i "/opt/module/zookeeper-3.5.7/bin/zkServer.sh stop"
	done
};;
"status"){
	for i in matt05 matt06 matt07
  do
    echo ---------- zookeeper $i 状态 ------------
    ssh $i "/opt/module/zookeeper-3.5.7/bin/zkServer.sh status"
	done
};;
esac
```





```go
chmod u+x zk.sh
```



```go
zk.sh start

zk.sh stop
```









写数据：半数以上同意就可以给客户端发送ack。





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210810164957.png)









# 3.客户端操作



## 启动

### 服务端启动

```go
./zkServer.sh start
```

### 查看服务端状态

```go
./zkServer.sh status
```

### 服务端关闭



```go
./zkServer.sh stop
```



### 客户端启动

```go
// 不指定服务端
./zkCli.sh

// 指定服务端
./zkCli.sh -server 192.168.96.132:2181
```

### 全格式查看jps

```go
jps -l
```



## 查看所有命令

```go
help
```



## 创建节点



```go
create /matt "matt"
```



-e:短暂，客户端关闭再次开启该节点就会删除

-s:带序号



```go
create -e -s /matt "1"

create -s /matt "2"
```



## 删除节点



```go
delete /matt

// 递归删除节点
deleteall /matt
```



## 修改节点



```go
set /matt "matt"
```

## 查看子节点

```go
// 查看当前节点包含哪些子节点
ls /matt

// 更加详细 查看当前节点包含哪些子节点
ls -s /matt
```

## 查看当前节点的值

```go
get /matt

// 详细
get -s /matt

// 查看节点的状态，就是不显示当前的值
stat /matt
```



**1.czxid:创建节点的事务 zxid**

每次修改ZooKeeper状态都会产生一个ZooKeeper事务ID。事务ID是ZooKeeper中所 有修改总的次序。每次修改都有唯一的 zxid，如果 zxid1 小于 zxid2，那么 zxid1 在 zxid2 之 前发生。

2.ctime:znode 被创建的毫秒数(从 1970 年开始)

**3.mzxid:znode 最后更新的事务 zxid**

4.mtime:znode 最后修改的毫秒数(从 1970 年开始) 

**5.pZxid:znode 最后更新的子节点 zxid**

6.cversion:znode 子节点变化号，znode 子节点修改次数

**7.dataversion:znode 数据变化号**

8.aclVersion:znode 访问控制列表的变化号

9.ephemeralOwner:如果是临时节点，这个是 znode 拥有者的 session id。如果不是 临时节点则是 0。

**10.dataLength:znode 的数据长度** 

**11.numChildren:znode 子节点数量**



## 监听

监听器：使用一次就会失效

监听节点的值发生变化

```go
get -w /matt
```



监听节点的子节点发生变化

```go
ls -w /matt
```











# **错误**





org.apache.zookeeper.server.quorum.QuorumPeerConfig$ConfigException: Address





```go
2021-08-14 11:44:23,004 [myid:] - ERROR [main:QuorumPeerMain@89] - Invalid config, exiting abnormally
org.apache.zookeeper.server.quorum.QuorumPeerConfig$ConfigException: Address unresolved: 192.168.96.135:3887 
```



在集群配置有空格，删除即可



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210814115022.png)







# 待做



软件包管理

2.2本地安装配置文件解读

3.1.2选举机制



监听器原理
