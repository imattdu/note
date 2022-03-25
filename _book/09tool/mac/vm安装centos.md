







## 安装



### 基础安装



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191304491.png)













#### 选择镜像



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191306870.png)













![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191309098.png)





















![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191319105.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191320034.png)











**处理器可以选择2个**

**内存4096**





![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191324100.png)





**磁盘 50g**



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191324443.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191325401.png)











![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191326882.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191327100.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191327137.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191328827.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191329219.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191329727.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191330347.png)





设置root 密码



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191331477.png)







![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191331892.png)







## 配置

### 配置网络

#### vm开启nat

偏好设置，网络 nat



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191521087.png)









```sh
cd /Library/Preferences/VMware\ Fusion/vmnet8

```

#### 查看nat.conf 

网关地址 子网掩码



```sh
cat nat.conf

```







![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191524417.png)



#### 查看dhcpd.conf



```sh
cat dhcpd.conf
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191527630.png)





#### 查看dns



系统偏好设置 -> 网络

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202191529306.png)







#### 配置centos



```sh
cd /etc/sysconfig/network-scripts



vim ifcfg-ens160
```



```sh
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
# 改
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=ens160
UUID=2c174a85-8cdc-4543-9e95-84bcf36e9d8d
DEVICE=ens160
# 下面都改
ONBOOT=yes
# 自定义静态ip 需要在range范围
IPADDR=172.16.110.129
# 网关地址
GATEWAY=172.16.110.2
# 子网掩码
NETMASK=255.255.255.0
# dns地址
DNS1=192.168.199.1
```







```sh
systemctl restart network
```





**如果你换了一个地方上网的话，可能会发现你的虚拟机有不通了，那是因为DNS地址发生了变化，此时只需要再次编辑ifcfg-enxxx文件，然后加上你现在网络的DNS地址即可**



参考

https://www.cnblogs.com/itbsl/p/10998696.html#%E9%85%8D%E7%BD%AE%E9%9D%99%E6%80%81ip







