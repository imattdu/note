## dockerfile





### 是什么

Dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。





### 构建过程解析



#### Dockerfile内容基础知识







1. 每条保留字指令都必须为大写字母且后面要跟随至少一个参数
2. 指令按照从上到下，顺序执行
3. #表示注释
4. 每条指令都会创建一个新的镜像层，并对镜像进行提交







*  Dockerfile是软件的原材料
*  Docker镜像是软件的交付品
*  Docker容器则可以认为是软件的运行态。





#### 大致流程





（1）docker从基础镜像运行一个容器

（2）执行一条指令并对容器作出修改

（3）执行类似docker commit的操作提交一个新的镜像层

（4）docker再基于刚提交的镜像运行一个新容器

（5）执行dockerfile中的下一条指令直到所有指令都执行完成



### 关键字



|   关键字   |                             描述                             |
| :--------: | :----------------------------------------------------------: |
|    FROM    |             基础镜像，当前新镜像是基于哪个镜像的             |
| MAINTAINER |                  镜像维护者的姓名和邮箱地址                  |
|    RUN     |                   容器构建时需要运行的命令                   |
|   EXPOSE   |                   当前容器对外暴露出的端口                   |
|  WORKDIR   |   指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点   |
|    ENV     |               用来在构建镜像过程中设置环境变量               |
|    ADD     | 将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包 |
|    COPY    | 类似ADD，拷贝文件和目录到镜像中。<br/>将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置 |
|   VOLUME   |             容器数据卷，用于数据保存和持久化工作             |
|    CMD     | 指定一个容器启动时要运行的命令<br/>（Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，<br/>CMD 会被 docker run 之后的参数替换） |
| ENTRYPOINT | 指定一个容器启动时要运行的命令<br/>（ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数） |
|  ONBUILD   | 当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发 |





ENV MY_PATH /usr/mytest
这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；
也可以在其它指令中直接使用这些环境变量，

比如：WORKDIR $MY_PATH







ONBUILD  



![](https://raw.githubusercontent.com/imattdu/img/main/img/2022/03/05/20220305002458.png)







### 案例



#### dockerfile



```sh
FROM         centos:centos7
MAINTAINER    matt<matt@qq.com>
#把宿主机当前上下文的c.txt拷贝到容器/usr/local/路径下
COPY c.txt /usr/local/cincontainer.txt
#把java与tomcat添加到容器中
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.8.tar.gz /usr/local/
#安装vim编辑器
RUN yum -y install vim
#设置工作访问时候的WORKDIR路径，登录落脚点
ENV MYPATH /usr/local
WORKDIR $MYPATH
#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_171
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.8
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.8
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#容器运行时监听的端口
EXPOSE  8080
#启动时运行tomcat
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.8/bin/startup.sh" ]
# CMD ["/usr/local/apache-tomcat-9.0.8/bin/catalina.sh","run"]
CMD /usr/local/apache-tomcat-9.0.8/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.8/bin/logs/catalina.out
```



#### 构建



```sh
docker build -t mcentos:1

# 默认会使用该文件夹Dockerfile 也可以通过-f指定其他文件夹
docker build -f Dockerfile -t mcentos:1
```











## 常用软件安装





### mysql





```sh
docker pull mysql:5.6


docker run -p 12345:3306 --name mysql -v /zzyyuse/mysql/conf:/etc/mysql/conf.d -v /zzyyuse/mysql/logs:/logs -v /zzyyuse/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6




docker run -p 12345:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6


```



### redis





```sh
docker pull redis:3.2



# --appendonly yes 开启持久化
docker run -p 6379:6379 -v /home/matt/myredis/data:/data -v /home/matt/myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf  -d redis:3.2 redis-server /usr/local/etc/redis/redis.conf --appendonly yes


```





aliyun



https://dev.aliyun.com/search.html







![](https://raw.githubusercontent.com/imattdu/img/main/img/2022/03/05/20220305010528.png)