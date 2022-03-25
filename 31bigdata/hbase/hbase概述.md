# 1.hbase简介

## 1.1定义

HBase 是一种分布式、可扩展、支持海量数据存储的 NoSQL（非关系型） 数据库。



## 1.2数据模型

逻辑上，HBase 的数据模型同关系型数据库很类似，数据存储在一张表中，有行有列。
但从 HBase 的底层物理存储结构（K-V）来看，HBase 更像是一个 multi-dimensional map。



### 1.2.1逻辑结构



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210816004104.png)

store:真正存储的东西

### 1.2.2物理结构

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210829005616.png)





### 1.2.3数据模型

1.Name Space 
命名空间，类似于关系型数据库的 DatabBase 概念，每个命名空间下有多个表。HBase
有两个自带的命名空间，分别是 hbase 和 default，hbase 中存放的是 HBase 内置的表，
default 表是用户默认使用的命名空间。 
2.Region 
类似于关系型数据库的表概念。不同的是，HBase 定义表时只需要声明列族即可，不需
要声明具体的列。这意味着，往 HBase 写入数据时，字段可以动态、按需指定。因此，和关
系型数据库相比，HBase 能够轻松应对字段变更的场景。 
3.Row 
HBase 表中的每行数据都由一个 RowKey 和多个 Column（列）组成，数据是按照 RowKey
的字典顺序存储的，并且查询数据时只能根据 RowKey 进行检索，所以 RowKey 的设计十分重
要。 
4.Column 
HBase 中的每个列都由 Column Family(列族)和 Column Qualifier（列限定符）进行限
定，例如 info：name，info：age。建表时，只需指明列族，而列限定符无需预先定义。 
5.Time Stamp 
用于标识数据的不同版本（version），每条数据写入时，如果不指定时间戳，系统会
自动为其加上该字段，其值为写入 HBase 的时间。

6.Cell 
由{rowkey, column Family：column Qualifier, time Stamp} 唯一确定的单元。cell 中的数据是没有类型的，全部是字节码形式存贮。



## 1.3基础架构

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210829010537.png)







架构角色： 
1.Region Server 
Region Server 为 Region 的管理者，其实现类为 HRegionServer，主要作用如下: 
对于数据的操作：get, put, delete； 
对于 Region 的操作：splitRegion、compactRegion。 
2.Master 
Master 是所有 Region Server 的管理者，其实现类为 HMaster，主要作用如下： 
 对于表的操作：create, delete, alter 
对于 RegionServer 的操作：分配 regions 到每个 RegionServer，监控每个 RegionServer的状态，负载均衡和故障转移。 
3.Zookeeper
HBase 通过 Zookeeper 来做 Master 的高可用、RegionServer 的监控、元数据的入口以及集群配置的维护等工作。 
4.HDFS 
HDFS 为 HBase 提供最终的底层数据存储服务，同时为 HBase 提供高可用的支持。





# 2.安装



**提前安装zk,hadoop。二者都为集群**





## 2.1安装



### 1.解压

```go
tar -zxvf hbase-1.3.1-bin.tar.gz -C /opt/module/
```



## 2.2配置



### 1.进入配置目录

```go
/opt/module/hbase-1.3.1/conf
```

### 2.记录 JAVA_HOME 路径

```GO
echo $JAVA_HOME

/opt/module/jdk1.8.0_212
```

### 3.编写配置文件hbase-env.sh



```go
vim hbase-env.sh
```



```go
// The java implementation to use.  Java 1.7+ required.
export JAVA_HOME=/opt/module/jdk1.8.0_212

// hbase 有自带的zk,不要使用自带的
// Tell HBase whether it should manage it's own instance of Zookeeper or not.
export HBASE_MANAGES_ZK=false
```

### 4.hbase-site.xml

```go
vim hbase-site.xml
```

hbase-site.xml=core-site.xml

hbase.rootdir=fs.defaultFS

```go
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://matt05:8020</value>
</property>
```







```xml
<configuration>
	<property>
    	<name>hbase.rootdir</name>
    	<value>hdfs://matt05:8020/HBase</value>
    </property>

    <property>
    	<name>hbase.cluster.distributed</name>
    	<value>true</value>
    </property>

    <!-- 0.98 后的新变动，之前版本没有.port,默认端口为 60000 -->
    <property>
    	<name>hbase.master.port</name>
    	<value>16000</value>
    </property>
    <property>
    	<name>hbase.zookeeper.property.dataDir</name>
    	<value>/opt/module/zookeeper-3.5.7/zkData</value>
    </property>
    <property>
    	<name>hbase.zookeeper.quorum</name>
    	<value>matt05,matt06,matt07</value>
    </property>

</configuration>

```



### 5.集群文件regionservers

```go
vim regionservers 
```





```go
matt05
matt06
matt07
```



### 6建立hadoop快捷方式



```go
ln -s /opt/module/hadoop-3.1.3/etc/hadoop/core-site.xml /opt/module/hbase-1.3.1/conf/core-site.xml
```





```go
ln -s /opt/module/hadoop-3.1.3/etc/hadoop/hdfs-site.xml /opt/module/hbase-1.3.1/conf/hdfs-site.xml
```





### 7.分发安装

```go
xsync hbase-1.3.1/
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210822150446.png)





## 2.3启动



记得开启同步时间

### 方式一

```go
[matt@matt05 bin]$ ./hbase-daemon.sh start master
```



```go
./hbase-daemon.sh start regionserver
```



### 方式二



```go
bin/start-hbase.sh

bin/stop-hbase.sh
```



## 2.4访问



### web页面

ip:port



192.168.96.135:16010







# 3.使用



## 3.1基础

### 进入hbase shell



```go
./hbase shell


quit
exit
```

退出；

### 帮助

```go
help
```



### 查看有哪些表

```go
list
```





## 命名相关





```go
// 创建命名空间
create_namespace 'test1'



create 'test1:student','info1'


// 需要先删除该命名空间下的表
drop_namespace 'test1'


list_namespace

```



## 表相关ddl



```go
hbase(main):023:0> create

ERROR: wrong number of arguments (0 for 1)

Here is some help for this command:
Creates a table. Pass a table name, and a set of column family
specifications (at least one), and, optionally, table configuration.
Column specification can be a simple string (name), or a dictionary
(dictionaries are described below in main help output), necessarily 
including NAME attribute. 
Examples:

Create a table with namespace=ns1 and table qualifier=t1
  hbase> create 'ns1:t1', {NAME => 'f1', VERSIONS => 5}

```





### 创建表

```go
create 't2','info'
```



### 删除表

```go
disable 'test'
drop 'test'
```



### 修改列组版本

```go
alter 't1', {NAME => 'info', VERSIONS => 1}
```



### 查看表的信息

```go
describe 't1'
```



### 查看所有表

```go
list
```



## dml crud





```go
put 'student','1001','info1:name','matt'



scan 'student'

// 表名 rowKey
get 'student','1001'


scan 'student',{COLUMNS => ['info1:name', 'info2:sex'], LIMIT => 10, STARTROW => '1001',STOPROW => '1003'}



scan 'student', {RAW => true, VERSIONS => 3}
```

```go
hbase(main):010:0> scan 't2', {RAW => true, VERSIONS => 3}
ROW                    COLUMN+CELL                                                    
 1001                  column=info:, timestamp=1630203659417, type=DeleteColumn       
 1001                  column=info:name, timestamp=1630203905379, type=DeleteColumn   
 1001                  column=info:name, timestamp=1630203448221, value=b             
 1001                  column=info:name, timestamp=1630203437357, value=a             
1 row(s) in 0.0240 seconds
```





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210823011717.png)





```go
// update
put 'student','1001','info1:name','matt1'


// 指定到列组则不会删除
delete 'student','1001','info1:sex'



deleteall 'student','1002'



truncate 'student'
```







