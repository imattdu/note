













[centos 7 镜像](http://mirrors.aliyun.com/centos/7/isos/x86_64/)





![](https://raw.githubusercontent.com/imattdu/img/main/img/202111141403835.png)

镜像介绍 各种格式



虚拟机默认安装即可



虚拟化



![](https://raw.githubusercontent.com/imattdu/img/main/img/202111141117716.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803013950.png)

# 安装centos





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165457.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165531.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165627.png)



**2个处理器、每个处理器一个核心数量**

**内存2g**



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210802235013.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210802235245.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165858.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165922.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223165944.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170017.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210802235149.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210802235340.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170129.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170218.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170315.png)



开机回车



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170546.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170716.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223170902.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223171000.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223171104.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223171209.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223171317.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803000832.png)



1g    45g    4g

标准分区，/boot，1g,文件系统， ext4

根分区新建，设备类型“标准分区”，挂载点为“/”，文件系统为“xfs”   

swap分区设置，文件系统为“swap”,Swap:内存不足有硬盘替换

**文件系统要选择对**



不启用Kdump

不启用dump,kdump用于系统崩溃保存数据



修改主机名



开始安装，设置Root密码即可等待安装完成





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223172257.png)



重启同意许可证完成配置即可





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223172950.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223173023.png)





完成配置在线账号跳过、设置账号注销使用root登录即可











# 网络

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210802195541.png)

## 设置NAT



如果win10没出现v8 那么在虚拟机中更改怀远配置

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803003602.png)

### 1.设置主机虚拟网络v8

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174231.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174307.png)



设置静态ip

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174355.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/20210803003327.png)

### 2.设置虚拟机网络



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174612.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174702.png)





![](https://raw.githubusercontent.com/imattdu/img/main/img/20210131174736.png)



子网ip，主机和虚拟机 需要在同一网段（因为在一个内网下）

**关闭windows防火墙**



### 3.设置centos网络



```java
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```



```java
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="6b84fa31-a207-4928-9e53-61ad2ec7b485"
DEVICE="ens33"
ONBOOT="yes"
```

```java
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static" // 更改该值
DEFROUTE="yes" 
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="6b84fa31-a207-4928-9e53-61ad2ec7b485"
DEVICE="ens33"
ONBOOT="yes"   // 改
IPADDR=192.168.96.129   // ip 地址,前三位和子网ip一致
GATEWAY=192.168.96.2    // ip.2
DNS1=192.168.96.2    // 默认
```



```go
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=dba7c9df-22d1-43b7-8291-e28dc76ed1dd
DEVICE=ens33
ONBOOT=no
```



### 4.重启网络或者重启

推荐重启即可

```java
service network restart
```

```go
systemctl restart network
```

如果无法上网点击有线连接即可

![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223174649.png)









# 防火墙

查看防火墙状态

```java
systemctl status firewalld.service
```

关闭防火墙

```java
systemctl stop firewalld.service
```

禁止开机启动

```java
systemctl disable firewalld.service
```





# 设置主机名称

```go
vim /etc/hostname
```

# 主机ip映射

主机名和ip映射

```go
vim /etc/hosts
```



```go
192.168.96.128 matt04
```



# 创建非root用户

## 创建matt用户



```go
useradd matt

passwd matt
```

## 为matt用户添加sudo权限

修改/etc/sudoers 文件，在%wheel 这行下面添加一行，如下所示：

```go
vim /etc/sudoers
```



```go
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL
matt    ALL=(ALL)       NOPASSWD:ALL
```

## 创建软件安装目录



```go
mkdir /opt/software

mkdir /opt/module
```

## 修改创建的文件的用户和用户组为matt



```go
chown -R matt:matt /opt/software

chown -R matt:matt /opt/module
```



验证

```go
ll
```



# 虚拟机克隆

在网络配置好在克隆记得一定要关机下



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223174740.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223174902.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/20201223175010.png)







**记得修改ip,服务器名，主机IP映射**



# 安装ssh

检查是否安装ssh



```java
yum list installed | grep openssh-server
```

安装ssh

```java
yum install -y openssl openssh-server
```

配置

```java
vim /etc/ssh/sshd_config
```

以下打开

**port,ListenAddress**

![](https://raw.githubusercontent.com/imattdu/img/main/img/20201218140734.png)



**PermitRootLogin,PubkeyAuthentication,Password**



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201218140817.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/20201218140845.png)



#### 退出

```python
logout 123.56.135.43 //退出ssh连接
exit
```



免密登录

```go
ssh-keygen

cd /home/matt/.ssh
    
ssh-copy-id -i id_rsa.pub root@192.168.96.128
```





# 虚拟机扩容

### 查看本机磁盘环境

```go
df -h
```



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728022647.png)



```go
lsblk
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728021802.png)



### 添加磁盘分区

```go
fdisk /dev/sda
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728031925.png)



**输入p不是默认e,第二个默认**

记得重启

```go
lsblk
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210728022101.png)



#### 开始扩容



创建物理卷

```go
[root@localhost ~]# lvm
lvm> pvcreate /dev/sda4
WARNING: dos signature detected on /dev/sda4 at offset 510. Wipe it? [y/n]: y
  Wiping dos signature on /dev/sda4.
  Physical volume "/dev/sda4" successfully created.
```

查看物理卷和卷组：



```go
lvm> pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               11.23 GiB / not usable 4.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              2875
  Free PE               0
  Allocated PE          2875
  PV UUID               lTqDdT-6UxW-feyh-V97X-BJEg-zN18-G5kc4Z
   
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               centos
  PV Size               5.00 GiB / not usable 4.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              1279
  Free PE               0
  Allocated PE          1279
  PV UUID               kSnxe0-jrzC-UPoT-UZQk-2oda-eZrK-ggQXKp
   
  "/dev/sda4" is a new physical volume of "2.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sda4
  VG Name               
  PV Size               2.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               VVW6TL-NF39-NfgU-2oBR-zqkF-RCiN-0I73Zq
   
```



```go
lvm> vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <16.23 GiB
  PE Size               4.00 MiB
  Total PE              4154
  Alloc PE / Size       4154 / <16.23 GiB
  Free  PE / Size       0 / 0   
  VG UUID               1V8J83-cmtx-C8uY-KAQZ-teqj-uGD2-Tmad21
```



将物理卷加入到卷组：



```go
lvm> vgextend centos /dev/sda4
  Volume group "centos" successfully extended
lvm> vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                3
  Act PV                3
  VG Size               18.22 GiB
  PE Size               4.00 MiB
  Total PE              4665
  Alloc PE / Size       4154 / <16.23 GiB
  Free  PE / Size       511 / <2.00 GiB
  VG UUID               1V8J83-cmtx-C8uY-KAQZ-teqj-uGD2-Tmad21
```





将卷组剩余空间(刚添加的2G)添加到逻辑卷/dev/centos/root :

```go
lvm> lvextend -l +100%FREE /dev/centos/root
  Size of logical volume centos/root changed from <14.32 GiB (3665 extents) to 16.31 GiB (4176 extents).
  Logical volume centos/root successfully resized.
lvm> exit
  Exiting.
```



#### 同步到文件系统

之前只是对逻辑卷扩容，还要同步到文件系统，实现对根目录的扩容。

```go
[root@localhost ~]# xfs_growfs /dev/centos/root
meta-data=/dev/mapper/centos-root isize=512    agcount=7, agsize=610304 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=3752960, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 3752960 to 4276224

```





```go
[root@localhost ~]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
devtmpfs                 894M     0  894M    0% /dev
tmpfs                    910M     0  910M    0% /dev/shm
tmpfs                    910M   11M  900M    2% /run
tmpfs                    910M     0  910M    0% /sys/fs/cgroup
/dev/mapper/centos-root   17G  9.3G  7.1G   57% /
/dev/sda1                187M  163M   25M   87% /boot
tmpfs                    182M     0  182M    0% /run/user/0
tmpfs                    182M  8.0K  182M    1% /run/user/42
[root@localhost ~]# 
```







### 删除分区

适用于还没有挂载，如果挂载则需要卸载

```go
fdisk /dev/sdc

# 输入d,然后按照提示输入分区号
```





