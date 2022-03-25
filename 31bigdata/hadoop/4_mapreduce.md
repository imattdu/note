





## 概述



### 定义



**MapReduce 是一个分布式运算程序的编程框架**，是用户开发“基于 Hadoop 的数据分析 应用”的核心框架。

MapReduce 核心功能是将用户编写的业务逻辑代码和自带默认组件整合成一个完整的 分布式运算程序，并发运行在一个 Hadoop 集群上。





### 优缺点

#### 优点

1.MapReduce 易于编程

​	它简单的实现一些接口，就可以完成一个分布式程序，



2.良好的扩展性

​	可以动态增加机器

3.高容错性

​	其中一台机器挂了，它可以把上面的计算任务转移到另外一个节点上运行， 不至于这个任务运行失败





#### 缺点

1.不擅长实时计算

​	MapReduce 无法像 MySQL 一样，在毫秒或者秒级内返回结果。

2.不擅长流式计算

​	流式计算的输入数据是动态的，而 MapReduce 的输入数据集是静态的，不能动态变化。 这是因为 MapReduce 自身的设计特点决定了数据源必须是静态的。



3.不擅长 DAG（有向无环图）计算



​	多个应用程序存在依赖关系，后一个应用程序的输入为前一个的输出。在这种情况下， MapReduce 并不是不能做，而是使用后，每个 MapReduce 作业的输出结果都会写入到磁盘， 会造成大量的磁盘 IO，导致性能非常的低下。





### 核心思想





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226095146.png)







（1）分布式的运算程序往往需要分成至少 2 个阶段。 

（2）第一个阶段的 MapTask 并发实例，完全并行运行，互不相干。 

（3）第二个阶段的 ReduceTask 并发实例互不相干，但是他们的数据依赖于上一个阶段 的所有 MapTask 并发实例的输出。 

（4）MapReduce 编程模型只能包含一个 Map 阶段和一个 Reduce 阶段，如果用户的业 务逻辑非常复杂，那就只能多个 MapReduce 程序，串行运行。









### MapReduce进程



（1）MrAppMaster：负责整个程序的过程调度及状态协调。

（2）MapTask：负责 Map 阶段的整个数据处理流程。 

（3）ReduceTask：负责 Reduce 阶段的整个数据处理流程。





### 序列化类型

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226095554.png)







### mapreduce编码规范



1．Mapper阶段 

（1）用户自定义的Mapper要继承自己的父类 

（2）Mapper的输入数据是KV对的形式（KV的类型可自定义） 

（3）Mapper中的业务逻辑写在map()方法中 

（4）Mapper的输出数据是KV对的形式（KV的类型可自定义） 

（5）map()方法（MapTask进程）对每一个调用一次





2．Reducer阶段 

（1）用户自定义的Reducer要继承自己的父类

（2）Reducer的输入数据类型对应Mapper的输出数据类型，也是KV 

（3）Reducer的业务逻辑写在reduce()方法中 

（4）ReduceTask进程对每一组相同k的组调用一次reduce()方法 



3．Driver阶段 

相当于YARN集群的客户端，用于提交我们整个程序到YARN集群，提交的是 封装了MapReduce程序相关运行参数的job对象





## 实操

### demo



#### 前提



需要首先配置好 HADOOP_HOME 变量以及 Windows 运行依赖（参考hdfs）





```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.matt.study</groupId>
    <artifactId>study-mapreduce</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>


    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>3.1.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.30</version>
        </dependency>
    </dependencies>


    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!--依赖的jar包也打进去-->
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```





log4j.properties

```properties
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

#### 具体实现

统计文本中每个单词出现的次数

map:单词的切割

reduce:单词的统计

driver:任务的配置 和提交



<img src="https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226100623.png" style="zoom:50%;" />







### hadoop集群运行

#### 打包依赖

```xml
<build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!--依赖的jar包也打进去-->
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

#### 运行



**不带依赖的jar包**

```sh
hadoop jar wc.jar com.matt.mapreduce.wordcount2.WordCountDriver /input /output
```





#### 默认案例

```go
/opt/module/hadoop-3.1.3/share/hadoop/mapreduce
```



```go
sz hadoop-mapreduce-examples-3.1.3.jar
```

 







## hadoop序列化

### 概述

#### 定义

序列化就是把内存中的对象，转换成字节序列（或其他数据传输协议）以便于存储到磁 盘（持久化）和网络传输。

反序列化就是将收到字节序列（或其他数据传输协议）或者是磁盘的持久化数据，转换 成内存中的对象。



#### 原因

一般来说，“活的”对象只生存在内存里，关机断电就没有了。而且“活的”对象只能 由本地的进程使用，不能被发送到网络上的另外一台计算机。 然而序列化可以存储“活的” 对象，可以将“活的”对象发送到远程计算机。





#### 为什么不用java的序列化

Java 的序列化是一个重量级序列化框架（Serializable），一个对象被序列化后，会附带 很多额外的信息（各种校验信息，Header，继承体系等），不便于在网络中高效传输。所以， Hadoop 自己开发了一套序列化机制（Writable）。



#### hadoop序列化特点

（1）紧凑 ：高效使用存储空间。 

（2）快速：读写数据的额外开销小。 

（3）互操作：支持多语言的交互





### 自定义 bean 对象实现序列化接口（Writable）



（1）必须实现 Writable 接口 

（2）反序列化时，需要反射调用空参构造函数，所以必须有空参构造

```java
public FlowBean() {
	super();
}
```



（3）重写序列化方法



```java
@Override
public void write(DataOutput out) throws IOException {
    out.writeLong(upFlow);
    out.writeLong(downFlow);
    out.writeLong(sumFlow);
}

```

（4）重写反序列化方法



```java
@Override
public void readFields(DataInput in) throws IOException {
    upFlow = in.readLong();
    downFlow = in.readLong();
    sumFlow = in.readLong();
}
```

（5）注意反序列化的顺序和序列化的顺序完全一致 

（6）要想把结果显示在文件中，需要重写 toString()，可用"\t"分开，方便后续用。 

（7）如果需要将自定义的 bean 放在 key 中传输，则还需要实现 Comparable 接口，因为 MapReduce 框中的 Shuffle 过程要求对 key 必须能排序。



### 实操



#### 需求

文本中有手机号 和它对象的上行流量 下行流量 总流量

求手机号上行流量 下行流量 总流量 各个和

手机号会出现多次



#### 参考**writable**





FlowBean：序列化 反序列化 toString

map:根据输入的记录构造进入到reduce的数据

reduce:求和



















![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226114616.png)





#### 建立连接



![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226114715.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226114831.png)



#### 提交job

835建立连接



839提交任务 **->**



![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226114918.png)



81创建给集群提交数据的 Stag 路径



90获取 jobid ，并创建 Job 路径

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226115020.png)



121拷贝 jar 包到集群

124计算切片，生成切片规划文件

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226115044.png)



153向 Stag 路径写 XML 配置文件







155提交 Job,返回提交状态

![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/26/20211226115108.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210809232800.png)





















![](https://raw.githubusercontent.com/imattdu/img/main/img/20210810002436.png)

输出路径不能存在







output不能存在

input只能有一个文件





map: key 偏移量

v:一行数据







inpputformat指定读取的格式 比如一行一行读入







![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/16/20211216225111.png)



index是逻辑 并没有真正切割







// 5）向 Stag 路径写 XML 配置文件

job配置









![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/17/20211217003558.png)











快排 meta数据

8按照字典序排序



80% 溢写到文件中  





11 合并 a 1 a 1 -> a 2







![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/17/20211217013920.png)







  



![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/17/20211217014654.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/19/20211219143735.png)









map 阶段 溢出 后归并排序

a1 ,a1 -> a2









![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/19/20211219164028.png)





\

### 

预聚合



combiner : a 1 a 1    a2 









**104**







![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/21/20211221225311.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/2021/12/22/20211222004409.png)







执行完成也会溢出





