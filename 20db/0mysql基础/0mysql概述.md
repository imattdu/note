## 概述

### 持久化

持久化(persistence): **把数据保存到可掉电式存储设备中以供之后使用**

掉电设备：数据库、文件、其他

### 数据库概念

#### 数据库 db

存储数据

#### DBMS**:数据库管理系统(**Database Management System**)**

对数据库进行统一管理和控制

#### SQL**:结构化查询语言(**Structured Query Language**)**

专门用来与数据库通信的语言

### 

### 数据库分类

#### 关系型数据库

##### 概述

这种类型的数据库是 最古老 的数据库类型，关系型数据库模型是把复杂的数据结构归结为简单的

二元关系 (即二维表格形式)。

##### 优缺点

**复杂查询** 可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。

**事务支持** 使得对于安全性能很高的数据访问要求得以实现。

#### 非关系型数据库

**非关系型数据库**，可看成传统关系型数据库的功能 阉割版本 ，基于键值对存储数据，不需要经过SQL层

的解析， 性能非常高 。同时，通过减少不常用的功能，进一步提高性能。

键值数据库

文档数据库

搜索引擎数据库

列式数据库

## 安装

### mac

#### 安装

官网：https://dev.mysql.com/downloads/mysql/

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120638517.png)

**一步一步安装即可**

**这里选择5.x加密**

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120151271.png)

设置root密码

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120152377.png)

#### 启动

在系统偏好设置里启动或者关闭服务

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120154920.png)

#### 配置

```sh
vim .zshrc
```

```sh
# mysql

PATH=$PATH:/usr/local/mysql/bin
export PATH
```
