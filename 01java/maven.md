



## maven安装与配置





### mac

#### 安装

下载地址：https://maven.apache.org/download.cgi



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202120020582.png)



解压重命名为 ApacheMaven，移动到/usr/local下

重命名

```sh
mv apache-maven-3.8.4 ApacheMaven
```



移动

```sh
mv ./ApacheMaven /usr/local/
```



#### 配置



```sh
vi ~/.bash_profile 

```





```sh
export M="/usr/local/ApacheMaven"
export PATH="$M/bin:$PATH"
```







```sh
source ~/.bash_profile

```





```sh
mvn -v
```



### Windows



#### 1.检查JAVA_HOME环境变量。

Maven是使用Java开发的，所以必须知道当前系统环境中JDK的安装目录。



```bash
C:\Users\matt>echo %JAVA_HOME%
D:\develop\env\jdk-8u201-windows-x64
```



#### 2.解压Maven的核心程序。

将apache-maven-3.5.0-bin.zip解压到一个**非中文无空格**的目录下。例如



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210508000144.png)



#### 3.配置环境变量

*都可以写在系统环境变量中*

**M2_HOME**

```bash
D:\develop\env\apache-maven-3.5.0
```

path

```bash
%M2_HOME%\bin
```



### 验证

#### 查看Maven版本信息验证安装是否正确



![](https://raw.githubusercontent.com/imattdu/img/main/img/20210508000527.png)



### 配置依赖下载位置、aliyun仓库、 jdk



[1]Maven默认的本地仓库：~\.m2\repository目录。

Tips：~表示当前用户的家目录。

[2]Maven的核心程序并不包含具体功能，仅负责宏观调度。具体功能由插件来完成。Maven核心程序会到本地仓库中查找插件。如果本地仓库中没有就会从远程中央仓库下载。此时如果不能上网则无法执行Maven的具体功能。为了解决这个问题，我们可以将Maven的本地仓库指向一个在联网情况下下载好的目录。

[3]Maven的核心配置文件位置：

```bash
解压目录\ D:\Server\ apache-maven-3.5.0\conf\settings.xml
```

[4]设置方式

```bash
<localRepository>D:/develop/env/mavenRepository</localRepository>
```

![](https://raw.githubusercontent.com/imattdu/img/main/img/20210508001031.png)







指定aliyun仓库

```xml
 <mirrors>
	<mirror>  
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>central</mirrorOf> 
	</mirror>
</mirrors>
```



指定jdk版本

```xml
<profiles>
	<profile>
		<id>jdk-1.8</id>
		<activation>
			<activeByDefault>true</activeByDefault>
			<jdk>1.8</jdk>
		</activation>

		<properties>
			<maven.compiler.source>1.8</maven.compiler.source>
			<maven.compiler.target>1.8</maven.compiler.target>
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
		</properties>
	</profile> 
</profiles>
```

