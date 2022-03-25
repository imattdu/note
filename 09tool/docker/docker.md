[![Typing SVG](https://readme-typing-svg.herokuapp.com?lines=this+is+docker)](https://git.io/typing-svg)

# 概述





### 是什么



一次封装， 到处运行

通过镜像构建一个容器，这个容器会提前安装一些软件



# 安装卸载





## 安装



**该教程适用centos7**



### 参考手册



https://docs.docker.com/install/linux/docker-ce/centos/





### 查看版本号

```go
cat /etc/redhat-release
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210812234746.png)



### 确保网络正常，安装gcc

```go
yum -y install gcc
```

### 安装gcc-c++

```go
yum -y install gcc-c++
```



### 卸载旧的版本

```sh
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728012901.png)



### 安装需要的软件包



```go
yum install -y yum-utils
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728012952.png)





### **设置stable镜像仓库**

推荐使用这个

```go
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 更新yum软件包索引

```go
yum makecache fast
```

### 安装DOCKER CE

```go
yum install docker-ce docker-ce-cli containerd.io
```

### 启动docker

```go
systemctl start docker
docker version
```

### 测试



```go
docker run hello-world
```

### 配置阿里云镜像



```go
mkdir -p /etc/docker
vim  /etc/docker/daemon.json
```

### 阿里云

```go
{
  "registry-mirrors": ["https://l0s7i35d.mirror.aliyuncs.com"]
}
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728232707.png)





```go
systemctl daemon-reload
systemctl restart docker
```



## 卸载



```go
systemctl stop docker

yum -y remove docker-ce docker-ce-cli containerd.io

rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```







## **错误**

可以参考linux 目录下Linux安装和基本配置.md中的磁盘扩容，更好的方式是安装的时候推荐50g空间

**空间不足**

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728013205.png)







## 基本组成



镜像：Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。

容器：Docker 利用容器（Container）独立运行的一个或一组应用。容器是用镜像创建的运行实例。

仓库：仓库（Repository）是集中存放镜像文件的场所。





## 原理



### 如何工作

Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上， 然后通过Socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。 容器，是一个运行时环境，就是我们前面说到的集装箱。





### 为什么比vm快





(1)docker有着比虚拟机更少的抽象层。由亍docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。

(2)docker利用的是宿主机的内核,而不需要Guest OS。因此,当新建一个容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引寻、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载Guest OS,返个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了返个过程,因此新建一个docker容器只需要几秒钟。

![](https://raw.githubusercontent.com/imattdu/img/main/img/2022/03/05/20220305102616.png)





# 常用命令

## 帮助命令



```go
docker version

docker info

docker --help
```



## 镜像命令

### docker images列出本地主机上的镜像

```go
docker images
```



REPOSITORY：表示镜像的仓库源
TAG：镜像的标签
IMAGE ID：镜像ID
CREATED：镜像创建时间
SIZE：镜像大小



```go
[root@iz2zeiw2bqogm8ir3ugpvqz ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   5 months ago   13.3kB
```



同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。
如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像





-a :列出本地所有的镜像（含中间映像层）

-q :只显示镜像ID。

--digests :显示镜像的摘要信息

--no-trunc :显示完整的镜像信息









```go
docker images --digests
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210813000017.png)

### docker search

docker search:从docker hub中搜，下载从阿里云镜像



docker search [OPTIONS] 镜像名字

https://hub.docker.com

```go
docker search tomcat
```



--no-trunc : 显示完整的镜像描述





### docker pull



docker pull 镜像名字[:TAG]



```go
docker pull tomcat
```



### docker rmi



docker rmi 某个XXX镜像名字ID



删除单个

```go
docker rmi  -f 镜像ID
```

删除多个

```go
docker rmi -f 镜像名1:TAG 镜像名2:TAG 
```



删除全部

```go
docker rmi -f $(docker images -qa)
```

## 容器命令

### 新建并启动容器



docker run [OPTIONS] IMAGE [COMMAND] [ARG...]



OPTIONS说明（常用）：有些是一个减号，有些是两个减号

--name="容器新名字": 为容器指定一个名称；
-d: 后台运行容器，并返回容器ID，也即启动守护式容器；
**-i：以交互模式运行容器，通常与 -t 同时使用；**
**-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；**
-P: 随机端口映射；
-p: 指定端口映射，有以下四种格式
      ip:hostPort:containerPort
      ip::containerPort
      hostPort:containerPort
      containerPort





```go
docker run -it centos /bin/bash
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210813001240.png)







### 退出容器

容器不停止

```go
ctrl+p+q
```

容器停止

```go
exit
```





### 列出当前运行的容器



docker ps [OPTIONS]



```go
docker ps
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210813001624.png)



OPTIONS说明（常用）：

**-a :列出当前所有正在运行的容器+历史上运行过的**
**-l :显示最近创建的容器。**
-n：显示最近n个创建的容器。
-q :静默模式，只显示容器编号。
--no-trunc :不截断输出。



常用

```sh
docker ps -qa
docker ps -a
```







### 启动容器



```go
//docker start 容器ID或者容器名

docker start centos:centos7
```

### 重启容器



```go
docker restart 容器ID或者容器名
```

### 停止容器

```go
docker stop 容器ID或者容器名
```

### 强制停止容器

```go
docker kill 容器ID或者容器名
```

### 删除已停止的容器



```go
docker rm 容器ID
```

**一次删除多个**

```go
docker rm -f $(docker ps -a -q)


docker ps -a -q | xargs docker rm
```







### 进入容器

```go
docker exec -it 容器id /bin/baah


docker attach 容器i
```





### 重要命令



#### 以后台方式启动

docker run -d 容器名

```sh
docker run -d tomcat
```

#### 查看容器日志



```sh
docker logs -f -t --tail 容器ID


```

- -t 是加入时间戳
- -f 跟随最新的日志打印
- --tail 数字 显示最后多少条





#### 查看容器内运行的进程



```sh
docker top 容器ID
```



#### 查看容器内部细节

docker inspect 容器ID









#### 进入正在运行的容器并以命令行交互

docker exec -it 容器ID bashShell



```sh
docker exec -it xxccc /bin/bash
```





docker attach 容器ID





区别

attach 直接进入容器启动命令的终端，不会启动新的进程

exec 是在容器中打开新的终端，并且可以启动新的进程



#### 从容器内拷贝文件到主机上



docker cp  容器ID:容器内路径 目的主机路径





```sh
attach    Attach to a running container                 # 当前 shell 下 attach 连接指定运行镜像
build     Build an image from a Dockerfile              # 通过 Dockerfile 定制镜像
commit    Create a new image from a container changes   # 提交当前容器为新的镜像
cp        Copy files/folders from the containers filesystem to the host path   #从容器中拷贝指定文件或者目录到宿主机中
create    Create a new container                        # 创建一个新的容器，同 run，但不启动容器
diff      Inspect changes on a container's filesystem   # 查看 docker 容器变化
events    Get real time events from the server          # 从 docker 服务获取容器实时事件
exec      Run a command in an existing container        # 在已存在的容器上运行命令
export    Stream the contents of a container as a tar archive   # 导出容器的内容流作为一个 tar 归档文件[对应 import ]
history   Show the history of an image                  # 展示一个镜像形成历史
images    List images                                   # 列出系统当前镜像
import    Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export]
info      Display system-wide information               # 显示系统相关信息
inspect   Return low-level information on a container   # 查看容器详细信息
kill      Kill a running container                      # kill 指定 docker 容器
load      Load an image from a tar archive              # 从一个 tar 包中加载一个镜像[对应 save]
login     Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器
logout    Log out from a Docker registry server          # 从当前 Docker registry 退出
logs      Fetch the logs of a container                 # 输出当前容器日志信息
port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口
pause     Pause all processes within a container        # 暂停容器
ps        List containers                               # 列出容器列表
pull      Pull an image or a repository from the docker registry server   # 从docker镜像源服务器拉取指定镜像或者库镜像
push      Push an image or a repository to the docker registry server    # 推送指定镜像或者库镜像至docker源服务器
restart   Restart a running container                   # 重启运行的容器
rm        Remove one or more containers                 # 移除一个或者多个容器
rmi       Remove one or more images             # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]
run       Run a command in a new container              # 创建一个新的容器并运行一个命令
save      Save an image to a tar archive                # 保存一个镜像为一个 tar 包[对应 load]
search    Search for an image on the Docker Hub         # 在 docker hub 中搜索镜像
start     Start a stopped containers                    # 启动容器
stop      Stop a running containers                     # 停止容器
tag       Tag an image into a repository                # 给源中镜像打标签
top       Lookup the running processes of a container   # 查看容器中运行的进程信息
unpause   Unpause a paused container                    # 取消暂停容器
version   Show the docker version information           # 查看 docker 版本号
wait      Block until a container stops, then print its exit code   # 截取容器停止时的退出状态值
```





# 镜像



## 是什么





一种可以创建运行环境的软件





## 特点

Docker镜像都是只读的
当容器启动时，一个新的可写层被加载到镜像的顶部。
这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。







## docker commit





docker commit提交容器副本使之成为一个新的镜像



docker commit -m=“提交的描述信息” -a=“作者” 容器ID 要创建的目标镜像名:[标签名]







```sh
docker run -it -p 8080:8080 tomcat


cd /usr/local/tomcat/webapps

touch a.txt

docker commit -m="my centos" -a="matt" xxcc mcentos:1.0



```





# 容器数据卷





## 可以干什么



容器的持久化

容器建继承+共享数据





## 数据卷



### 命令





```sh
docker run -it -v /宿主机绝对路径目录:/容器内目录      镜像名
 
 
# 查看容器是否挂载成功 
docker inspect 容器ID


# 只读
docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名

```

容器停止，对宿主机的修改仍可以对容器发生改变





![](https://raw.githubusercontent.com/imattdu/img/main/img/2022/03/04/20220304003222.png)



```sh
docker run -it -v /home/matt/dataVolumeContainer:/dataVolumeContainer centos /bin/bash
```









### Dockerfile



```sh
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,--------success1"
CMD /bin/bash
```





```sh
 docker build -f /home/matt/docker/Dockerfile -t matt/centos .
```











镜像是千层饼，一个镜像可能多层组成





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210731181948.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/20210731235833.png)



!> 一段重要的内容，随机端口号





```go
docker run -dit -P 8080 tomcat
```

宿主机端口随机分配









## 问题

*没遇到*

Docker挂载主机目录Docker访问出现cannot open directory .: Permission denied
解决办法：在挂载目录后多加一个--privileged=true参数即可







## 数据卷容器





### 是什么



命名的容器挂载数据卷，其它容器通过挂载这个(父容器)实现数据共享，挂载数据卷的容器，称之为数据卷容器







c1

```sh
docker run -it --name c1 matt/centos
```

c2继承c1

```sh
docker run -it --name c2 --volumes-from c1 matt/centos

docker run -it --name c3 --volumes-from c1 matt/centos
```

c2，c3数据就可以共享，即使删除c1，仍然可以共享



**容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止**









