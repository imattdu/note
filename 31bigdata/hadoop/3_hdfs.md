



## 1.概述

### 概述

hdfs:分布式 + 文件系统



### 优缺点

#### 优点:

高容错性：副本

适合处理大数据

#### 缺点：

不适合低延时数据访问

无法高效的对大量小文件进行存储：存储大量小文件的话，它会占用NameNode大量的内存来存储文件目录和 块信息



不支持并发写入、文件随机修改：一个文件只能有一个写，不允许多个线程同时写，仅支持数据的追加 不支持修改

### 架构



#### 1）NameNode（nn）：就是Master，它

是一个主管、管理者。
（1）管理HDFS的名称空间；
（2）配置副本策略；(有几个副本)
（3）管理数据块（Block）映射信息；
（4）处理客户端读写请求。

#### 2）DataNode：就是Slave。NameNode

下达命令，DataNode执行实际的操作。
（1）存储实际的数据块；
（2）执行数据块的读/写操作。





#### 3）Client：就是客户端。

（1）文件切分。文件上传HDFS的时候，Client将文件切分成一个一个的Block，然后进行上传；

// 200mb 的文件分为 128 72 2块 默认文件块大小128mb 可以修改

（2）与NameNode交互，获取文件的位置信息；
（3）与DataNode交互，读取或者写入数据；
（4）Client提供一些命令来管理HDFS，比如NameNode格式化；
（5）Client可以通过一些命令来访问HDFS，比如对HDFS增删查改操作；





#### 4）Secondary NameNode：并非NameNode的热备。当NameNode挂掉的时候，它并不

能马上替换NameNode并提供服务。
（1）辅助NameNode，分担其工作量，比如定期合并Fsimage（镜像）和Edits（编辑日志），并推送给NameNode ；
（2）在紧急情况下，可辅助恢复NameNode。





![](https://raw.githubusercontent.com/imattdu/img/main/img/202111160014439.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/202111160015676.png)









文件块大小是上限



如果小于128mb的文件 那么文件是100mb 那么占100mb







思考：为什么块的大小不能设置太小，也不能设置太大？ 

（1）HDFS的块设置太小，会增加寻址时间，程序一直在找块的开始位置； 

（2）如果块设置的太大，从磁盘传输数据的时间会明显大于定位这个块开 始位置所需的时间。导致程序在处理这块数据时，会非常慢。 总结：HDFS块的大小设置主要取决于磁盘传输速率。



## 2.shell



### **帮助命令**

```bash
hadoop fs -help rm
```







### 创建文件夹

```go
hadoop fs -mkdir /sanguo
```





### 剪切

```go
hadoop fs -moveFromLocal ./shuguo.txt /sanguo
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805233816.png)



**习惯**

```bash
hadoop fs -mv /sanguo/wuguo.txt /
```







### 拷贝

```go
hadoop fs -copyFromLocal ./weiguo.txt /sanguo
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210805234848.png)



**习惯**

```bash
hadoop fs -cp /sanguo/shuguo.txt /sanguo/jingguo
```





### 上传

```go
hadoop fs -put wuguo.txt /sanguo
```





### 下载

```go
hadoop fs -get /xiyou/test1 ./
```



### rm

```bash
hadoop fs -rm /sanguo/shuguo.txt
# 递归删除
hadoop fs -rm -r /sanguo/shuguo.txt
```













### ls

```go
[matt@matt05 stuhadoop]$ hadoop fs -ls /
Found 4 items
drwxr-xr-x   - matt supergroup          0 2021-08-05 23:22 /input
drwxr-xr-x   - matt supergroup          0 2021-08-05 00:22 /output
drwxr-xr-x   - matt supergroup          0 2021-08-05 23:50 /sanguo
drwx------   - matt supergroup          0 2021-08-05 00:21 /tmp
```

### cat



```go
[matt@matt05 stuhadoop]$ hadoop fs -cat /sanguo/weiguo.txt
2021-08-06 00:02:01,065 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
caocao
```







### tail

-tail：显示一个文件的末尾 1kb 的数据

```go
hadoop fs -tail /sanguo/wuguo.txt /
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806001930.png)





### 设置权限chmod

```go
[matt@matt05 stuhadoop]$ hadoop fs -chmod 777 /sanguo
[matt@matt05 stuhadoop]$ hadoop fs -ls /
Found 4 items
drwxr-xr-x   - matt supergroup          0 2021-08-05 23:22 /input
drwxr-xr-x   - matt supergroup          0 2021-08-05 00:22 /output
drwxrwxrwx   - matt supergroup          0 2021-08-05 23:50 /sanguo
drwx------   - matt supergroup          0 2021-08-05 00:21 /tmp
```



### chown

```go
hadoop fs -chown matt:matt /sanguo/shuguo.txt
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806000640.png)

### chgrp

```go
hadoop fs -chgrp matt /sanguo/weiguo.txt
```











### 查看文件大小

```go
hadoop fs -du -h -s /
```

三个副本



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806004950.png)





### 更加详细内容

```go
hadoop fs -du -h /
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806005124.png)



### 设置副本

```go
hadoop fs -setrep 10 /sanguo
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806005517.png)





## 3.api

### 安装

#### 解压

如果win10要使用还需要安装依赖库

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212153216.png)





#### 配置

```go
HADOOP_HOME


D:\develop\env\hadoop-3.1.0 
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806010731.png)



path:

```go
%HADOOP_HOME%\bin
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806010954.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806011151.png)













![](https://raw.githubusercontent.com/imattdu/img/main/img/20210806081940.png)





复制不要带空格，不然它就会换行





### api

可以参考 https://github.com/imattdu/studyhdfs







## 4.hdfs读写流程



### 4.1写数据流程



#### 具体流程



![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212141219.png)





（1）客户端通过 Distributed FileSystem 模块向 NameNode 请求上传文件，NameNode 检查目标文件是否已存在，父目录是否存在。 （2）NameNode 返回是否可以上传。 

（3）客户端请求第一个 Block 上传到哪几个 DataNode 服务器上。 （4）NameNode 返回 3 个 DataNode 节点，分别为 dn1、dn2、dn3。 

（5）客户端通过 FSDataOutputStream 模块请求 dn1 上传数据，dn1 收到请求会继续调用 dn2，然后 dn2 调用 dn3，将这个通信管道建立完成。 

（6）dn1、dn2、dn3 逐级应答客户端。 

（7）客户端开始往 dn1 上传第一个 Block（先从磁盘读取数据放到一个本地内存缓存）， 以 Packet 为单位，dn1 收到一个 Packet 就会传给 dn2，dn2 传给 dn3；dn1 每传一个 packet 会放入一个应答队列等待应答。 

（8）当一个 Block 传输完成之后，客户端再次请求 NameNode 上传第二个 Block 的服务 器。（重复执行 3-7 步）。





####  网络拓扑-节点距离计算

在 HDFS 写数据的过程中，NameNode 会选择距离待上传数据最近距离的 DataNode 接 收数据。那么这个最近距离怎么计算呢？



俩个节点到最近公共祖先节点的距离和





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212142248.png)





副本节点的选择



![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212142452.png)



### 4.2读数据



#### 4.2.1 具体流程

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212142533.png)



（1）客户端通过 DistributedFileSystem 向 NameNode 请求下载文件，NameNode 通过查询元数据，找到文件块所在的 DataNode 地址。 

（2）挑选一台 DataNode（就近原则，然后随机）服务器，请求读取数据。 

（3）DataNode 开始传输数据给客户端（从磁盘里面读取数据输入流，以 Packet 为单位 来做校验）。 

（4）客户端以 Packet 为单位接收，先在本地缓存，然后写入目标文件。







## 5.NameNode 和 SecondaryNameNode



### NN 和 2NN 工作机制





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212143103.png)



1）第一阶段：NameNode 启动 

（1）第一次启动 NameNode 格式化后，创建 Fsimage 和 Edits 文件。如果不是第一次启动，直接加载编辑日志和镜像文件到内存。 

Fsimage ：镜像文件

Edits ：日志

（2）客户端对元数据进行增删改的请求。 

（3）NameNode 记录操作日志，更新滚动日志。 

（4）NameNode 在内存中对元数据进行增删改。 



2）第二阶段：Secondary NameNode 工作 

（1）Secondary NameNode 询问 NameNode 是否需要 CheckPoint。直接带回 NameNode 是否检查结果。 

（2）Secondary NameNode 请求执行 CheckPoint。 

（3）NameNode 滚动正在写的 Edits 日志。 

（4）将滚动前的编辑日志和镜像文件拷贝到 Secondary NameNode。 （5）Secondary NameNode 加载编辑日志和镜像文件到内存，并合并。 （6）生成新的镜像文件 fsimage.chkpoint。 

（7）拷贝 fsimage.chkpoint 到 NameNode。 

（8）NameNode 将 fsimage.chkpoint 重新命名成 fsimage。





### Fsimage 和 Edits 解析





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212103758.png)





（1）Fsimage文件：HDFS文件系统元数据的一个永久性的检查点，其中包含HDFS文件系统的所有目 录和文件inode的序列化信息。 fsimage_0000000000000000000 fsimage_0000000000000000000.md5 seen_txid VERSION 

（2）Edits文件：存放HDFS文件系统的所有更新操作的路径，文件系统客户端执行的所有写操作首先 会被记录到Edits文件中。 

（3）seen_txid文件保存的是一个数字，就是最后一个edits_的数字 

（4）每 次NameNode启动的时候都会将Fsimage文件读入内存，加 载Edits里面的更新操作，保证内存 中的元数据信息是最新的、同步的，可以看成NameNode启动的时候就将Fsimage和Edits文件进行了合并。





#### 5.2.1 oiv 查看 Fsimage 文件

hdfs oiv -p 文件类型 -i 镜像文件 -o 转换后文件输出路径



```sh
cd /opt/module/hadoop-3.1.3/data/dfs/name/current
```





```sh
hdfs oiv -p XML -i fsimage_0000000000000008685 -o /opt/module/hadoop-3.1.3/fsimage.xml
```



#### 5.2.2 oev 查看 Edits 文件



hdfs oev -p 文件类型 -i 镜像文件 -o 转换后文件输出路径

```sh
hdfs oev -p XML -i edits_inprogress_0000000000000008686 -o /opt/module/hadoop-3.1.3/edits.xml
```





**下载文件到本地**

```sh
sz fsimage.xml
```



### 5.3CheckPoint 时间设置

1）通常情况下，SecondaryNameNode 每隔一小时执行一次。 



```xml
<property>
     <name>dfs.namenode.checkpoint.period</name>
     <value>3600s</value>
</property>

```









2）一分钟检查一次操作次数，当操作次数达到 1 百万时，SecondaryNameNode 执行一次。





```xml
<property>
    <name>dfs.namenode.checkpoint.txns</name>
     <value>1000000</value>
    <description>操作动作次数</description>
</property>
<property>
     <name>dfs.namenode.checkpoint.check.period</name>
     <value>60s</value>
    <description> 1 分钟检查一次操作次数</description>
</property>
```





## 6.datanode



### 6.1 datanode 工作机制







![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212145927.png)





（1）一个数据块在 DataNode 上以文件形式存储在磁盘上，包括两个文件，一个是数据 本身，一个是元数据包括数据块的长度，块数据的校验和，以及时间戳。 

（2）DataNode 启动后向 NameNode 注册，通过后，周期性（6 小时）的向 NameNode 上 报所有的块信息。



```properties
<property>
    <name>dfs.blockreport.intervalMsec</name>
    <value>21600000</value>
    <description>Determines block reporting interval in
    milliseconds.</description>
</property>

```

DN 扫描自己节点块信息列表的时间，默认 6 小时



```properties
<property>
    <name>dfs.datanode.directoryscan.interval</name>
    <value>21600s</value>
    <description>Interval in seconds for Datanode to scan datadirectories and reconcile the difference between blocks in memory and on
    the disk.
    Support multiple time unit suffix(case insensitive), as described
    in dfs.heartbeat.interval.
    </description>
</property>
```

（3）心跳是每 3 秒一次，心跳返回结果带有 NameNode 给该 DataNode 的命令如复制块 数据到另一台机器，或删除某个数据块。如果超过 10 分钟没有收到某个 DataNode 的心跳， 则认为该节点不可用。 

（4）集群运行中可以安全加入和退出一些机器。



### 6.2数据校验

**保证数据完整性**



如下是 DataNode 节点保证数据完整性的方法。 

（1）当 DataNode 读取 Block 的时候，它会计算 CheckSum。 

（2）如果计算后的 CheckSum，与 Block 创建时值不一样，说明 Block 已经损坏。 

（3）Client 读取其他 DataNode 上的 Block。 

（4）常见的校验算法 crc（32），md5（128），sha1（160） 

（5）DataNode 在其文件创建后周期验证 CheckSum。



奇偶校验

1的个数 偶数是0 否则是1

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212114050.png)





### 6.3 掉线时限参数设置





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/12/20211212150510.png)



4、如果定义超时时间为TimeOut，则超时时长的计算公式为：

TimeOut = 2 * dfs.namenode.heartbeat.recheck-interval + 10 * dfs.heartbeat.interval。 而默认的dfs.namenode.heartbeat.recheck-interval 大小为5分钟，dfs.heartbeat.interval默认为3秒。



```properties
<property>
     <name>dfs.namenode.heartbeat.recheck-interval</name>
     <value>300000</value>
    </property>
    <property>
     <name>dfs.heartbeat.interval</name>
     <value>3</value>
</property>
```

