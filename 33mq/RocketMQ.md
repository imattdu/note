## **错误**

在编写配置文件，错误将brokerId写成brokerld,导致从节点不能启动，从节点需要满足brokerId不是0。



## 概述



### RocketMQ优点



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224202047.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201221215543.png)



### 概念模型



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220202232.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220202423.png)



**message:消息**



NameServer的主要功能是为整个MQ集群提供服务协调与治理，具体就是记录维护Topic、Broker的信息，及监控Broker的运行状态。为client提供路由能力（具体实现和zk有区别，NameServer是没有leader和follower区别的，不进行数据同步，通过Broker轮训修改信息）。







### 源码包说明



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220203132.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220203359.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220203700.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201220203743.png)



## 安装

```
vim /etc/hosts
```

```
192.168.96.128 rocketmq-nameserver1
192.168.96.128 rocketmq-master1
```





```java
mkdir /usr/local/apache-rocketmq

tar -zxvf apache-rocketmq.tar.gz -C /usr/local/apache-rocketmq/
```





```java
cd /usr/local/apache-rocketmq/
    
ln -s apache-rocketmq/ rocketmq
```
创建数据存储文件夹

```
mkdir /usr/local/rocketmq/store 
mkdir /usr/local/rocketmq/store/commitlog 
mkdir /usr/local/rocketmq/store/consumequeue 
mkdir /usr/local/rocketmq/store/index
```
编写配置文件
```
vim /usr/local/rocketmq/conf/2m-2s-async/broker-a.properties
```



/2m-2s-async



broker-a.properties



```
#把下面得配置覆盖替换到broker-a.properties文件中
#所属的集群名字
brokerClusterName=rocketmq-cluster
#broker名字，注意：此处不同的配置文件填写的不一样
brokerName=broker-a
# 0 表示 master主节点 ，不是0则代表从节点slave
brokerId=0
#nameServer地址，分号分隔，
#注意：nameSeve的地址就是部署mq服务器的地址，可以在/etc/hosts中修改服务器ip指定一个名字
#我的服务器ip对应的名字是阿里云默认的名字aliyun
#也可以自己修改成名字叫rocketmq-nameserver1,修改方法为
#vim /etc/hosts ,然后保存退出，再重启一下网卡命令： service network restart
namesrvAddr=rocketmq-nameserver1:9876
#在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
defaultTopicQueueNums=4
#是否允许broker自动创建topic,建议线下开启，线上关闭
autoCreateSubscriptionGroup=true
#broker 对外服务的监听端口
listenPort=10911
#删除文件时间点，默认凌晨4点
deleteWhen=04
文件保留时间，默认48小时
fileReservedTime=48
#commitlog每个文件的大小默认1G
mapedFileSizeCommitLog=1073741824
#ConsumeQueue每个文件默认存30w条，根据业务情况调整
mapedFileSizeConsumeQueue=300000
#检测物理文件磁盘空间
diskMaxUsedSpaceRatio=88
 
#存储路径
storePathRootDir=/usr/local/rocketmq/store
#commitlog 存储路径
storePathCommitLog=/usr/local/rocketmq/store/commitlog
#消费队列存储路径
storrPathConsumeQueue=/usr/local/rocketmq/store/consumequeue
#消息索引存储路径
storePathIndex=/usr/local/store/index
#checkpoint文件存储路径
storeCheckpoint=/usr/local/rocketmq/store/checkpoint
#abort文件存储路径
abortFile=/usr/local/rocketmq/store/abort
#限制消息得大小
maxMessageBiz=65536
#broker的角色
#ASYNC_MASTER 异步复制模式master
#SYNC_MASTER 同步双写模式master
#SLAVE
brokerRole=ASYNC_MASTER
#刷盘方式
#ASYNC_MASTER 异步刷盘
#SYNC_MASTER 同步刷盘
flushDiskType=ASYNC_FLUSH
```

替换日志文件

```bash
mkdir -p /usr/local/rocketmq/logs 
cd /usr/local/rocketmq/conf && sed -i 's#${user.home}#/usr/local/rocketmq#g' *.xml
```


修改jvm参数

vim /usr/local/rocketmq/bin/runbroker.sh

```bash
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn1g
```

vim /usr/local/rocketmq/bin/runserver.sh



```bash
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn1g
```

cd /usr/local/rocketmq/bin/



### **启动**



```bash
cd /usr/local/rocketmq/bin/
nohup sh mqnamesrv &
```



```bash
nohup sh mqbroker -c ../conf/2m-2s-async/broker-a.properties >/dev/null 2 >&1 &
```

### 关闭

```
// 关闭broker
sh mqshutdown broker

// 关闭namesrv
sh mqshutdown namesrv
```







## 使用



[控制台地址](https://github.com/apache/rocketmq-externals)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210310200407.png)







Consumer



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210310212835.png)





## 原理









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201222204249.png)

主从模式：当主机写入数据时，从节点会拉取主节点的值







同步刷盘：



异步刷盘：直接返回成功，之后才判断数据是否存在





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201222204917.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224163059.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224163334.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224164533.png)





真实消息实时同步socket，元数据netty定时任务



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224170636.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224170707.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224174949.png)











zaimessage设置

![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224175540.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224180140.png)





### will

同步异步



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224201230.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224201404.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224201743.png)





消费端的监听器：并发和顺序俩种





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224203010.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224204541.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224204846.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224205041.png)



pull:由自己去维护offset值





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224210502.png)



push:broker主动推送







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224210942.png)



offset：1.存储在map中



2.定时任务去拉去消息



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224212833.png)





消费者零拷贝

顺序写随机读





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224214126.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224214305.png)





同步复制异步刷盘





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224215015.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224215518.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201224220452.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225214641.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225215124.png)

设计在同一个数据库中，或者同一张表中

可能一次调用不成功，所有可能调用多次，我们做到接口幂等性













划分任务维度





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225215820.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225220416.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225220808.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225220947.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225221535.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225221647.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225221743.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225223628.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201225230208.png)









```java
 mvn -Prelease-all -DskipTests clean install -U
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210309215050.png)