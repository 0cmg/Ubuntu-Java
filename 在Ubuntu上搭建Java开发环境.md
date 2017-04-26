# 在Ubuntu上搭建Java开发环境

![enter image description here](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/linux%E7%9B%AE%E5%BD%95.png?raw=true)

&emsp;&emsp;本文将介绍在Linux的发行版本Ubuntu上搭建java环境 及部署java  web项目。作为个人的学习总结，方便今后复习查看。  
- **SSH服务端和客户端安装**
- **Java环境搭建**
- **部署Java Web项目**
## 安装SSH服务端及客户端
>**SSH简介:** secure shell,通过ssh实现安全远程访问linux系统 。SSH是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用SSH协议可以有效防止远程管理过程中的信息泄露问题。通过SSH可以对所有传输的数据进行加密，也能够防止DNS欺骗和IP欺骗。——[维基百科](https://zh.wikipedia.org/wiki/Secure_Shell)

```python
1. 查看是否安装了ssh 服务端与客户端
     sudo apt-cache policy openssh-client openssh-server
2. 安装ssh 服务端与客户端软件(*Ubuntu上默认安装了SSH的客户端*)
      sudo dpkg -i ./ssh/*
```
*ps:* sudo dpkg -i ./ssh/*  中的ssh是SSH安装包的文件夹
![Aaron Swartz](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/ls_ssh.png?raw=true)
&emsp;&emsp;在Ubuntu上安装好了SSH的服务端（默认端口22）后就需要在个人电脑上(一般是Windows系统)安装SSH的客户端,在Windows下的SSH客户端软件有很多，比如：
- **putty：** 在Windows下进行远程登录到Ubuntu系统；
- **winscp433setup.exe ：** 文件传输工具；
- **[Xmanager Enterprise ](https://www.netsarang.com/)（个人安装的客户端软件）：** 可以进行远程登录(***通过打开Xshell***)和文件传输(***通过打开Xftp***)![enter image description here](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/xmanager.png?raw=true)

&emsp;&emsp;**xshell远程登录连接：**

<img src="https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/Xshell.png?raw=true" width = "70%" height = "50%"/> 

&emsp;&emsp;**xftp远程文件传输：**（*协议选SFTP*）

<img src="https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/xftp.png?raw=true" width = "60%" /> 

## Java环境搭建
- **安装JDK**
- **安装Tomcat**
- **安装MySQL**

**安装JDK**
```java
进入 root用户
   实现步骤：
		1.把jdk压缩文件剪切到/opt下面：
			mv jdk-7u80-linux-i586.tar.gz /opt 
		2. 绿色软件，解压（解压之后将jdk压缩文件删除）
			tar -xzvf jdk-7u80-linux-i586.tar.gz
		3. 执行命令
			进入到jdk的bin目录下执行：./命令
			比如：./java
		4.设置环境变量
		   vim /etc/profile 
		   export JAVA_HOME="/opt/jdk1.7.0_80"
	       export PATH="$JAVA_HOME/bin:$PATH"
	       // 可以使用echo $环境变量 查看环境变量的值
	       //如：
	       // xjc@ubuntu:~/Desktop$ echo $JAVA_HOME
		   // /opt/jdk1.7.0_80

		5. 刷新配置 ，让配置生效
		   source /etc/profile
		6. 编写Demo.java,测试 
		   -javac Demo.java
	       -java Demo
```

---------
**安装tomcat**
```java
	1.将apache-tomcat-7.0.77.tar.gz剪切到/opt下面
		sudo mv apache-tomcat-7.0.77.tar.gz /opt 
	2.解压（解压后删除压缩文件）
		tar -xzvf  apache-tomcat.tar.gz	
	3.将解压文件重命名
		sudo mv apache-tomcat-7.0.77 tomcat7 	
	4. 运行
	   $ ./startup.sh
	   $ ./shutdown.sh		
	5.cd到/opt/tomcat7/bin 下，编辑catalina.sh文件，向中插入以下代码（插到'#'注释结束，正文开始的地方）：   
		#此处依你的jdk安装目录而定  
		JAVA_HOME=/opt/jdk1.7.0_80
	6.保存，关闭。 //使用vim编辑 i：insert插入模式  ：命令模式(wq:保存退出、q！：强制退出，不保存)  esc：回到只读模式   
```

**配置Tomcat服务**
```java
	1.cd到/etc/init.d目录，增加tomcat7文件
		cd /etc/init.d
		sudo touch tomcat7
	2.编辑tomcat7文件
		$ sudo vi tomcat7 
		增加下面内容，保存。
			#!/bin/sh  
			#tomcat auto-start  
			  
			case $1 in  
			start)  
			 sh /opt/tomcat7/bin/startup.sh  
			 ;;  
			stop)  
			 sh /opt/tomcat7/bin/shutdown.sh  
			 ;;  
			restart)  
			 sh /opt/tomcat7/bin/shutdown.sh  
			 sh /opt/tomcat7/bin/startup.sh  
			 ;;  
			*)  
			 echo 'Usage:tomcat7 start|stop|restart'  
			 ;;  
			esac  
			exit 0		
	3.tomcat7设置成可执行		
		$ sudo chmod +x /etc/init.d/tomcat7 
	4.将tomcat7加入服务（开机自启动）
		$ sudo update-rc.d tomcat7 defaults
	5.启动、重启、停止命令
		$ sudo service tomcat7 start    //启动  
		$ sudo service tomcat7 restart  //重启  
		$ sudo service tomcat7 stop     //停止  		
```

&emsp;&emsp;ok，完成了，重启后（不登录），找另一台机器在浏览器上访问该Tomcat，只要能通就说明服务起来了。

**卸载**

	- 1.删除tomcat7目录即可
	- 2.删除tomcat7服务文件

***ps：***要修改Tomcat的server.xml文件中的<Connector>标签，将URI的编码设置为UTF-8，否则get请求参数传至后台会出现乱码。

```java
<Connector URIEncoding="UTF-8" acceptCount="8000" connectionTimeout="60000" maxProcessors="8500" maxThreads="7500" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
```

---------

**安装MySQL**
>查看是否安装mysql 需要管理员权限
	netstat -tap|grep mysql
apt安装mysql (apt-get是在线安装命令)
	sudo apt-get install mysql-server mysql-client
	如果安装失败,尝试使用 apt-get update
	安装过程中弹出一个界面要求输入mysql的root 密码,一定要输入.
	然后要求确认再输入一边.等到mysql 完成.
	默认安装目录:/usr/local/mysql
查看是否安装成功
	netstat -tap|grep mysql来查看系统是否已经有了mysql服务

**修改MySQL服务器的配置**
&emsp;&emsp; 
1.将字符编码设置为UTF-8
```java
	默认情况下，MySQL的字符集是latin1，因此在存储中文的时候，会出现乱码的情况，所以我们需要把字符集统一改成UTF-8。
用vi打开MySQL服务器的配置文件my.cnf
~ sudo vi /etc/mysql/my.cnf
#在[client]标签下，增加客户端的字符编码
[client]
default-character-set=utf8
#在[mysqld]标签下，增加服务器端的字符编码
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
```
&emsp;&emsp;2.让MySQL服务器被远程访问
```java
默认情况下，MySQL服务器不允许远程访问，只允许本机访问，所以我们需要设置打开远程访问的功能。
用vi打开MySQL服务器的配置文件my.cnf
~ sudo vi /etc/mysql/my.cnf
#注释bind-address
#bind-address            = 127.0.0.1
```

修改后，重启MySQL服务器。
```java
	$ sudo service mysql restart  //重启
	$ sudo service mysql start    //启动  
	$ sudo service mysql stop     //停止  	
	或者：
		启动
		/etc/init.d/mysql start
		停止
		/etc/init.d/mysql stop
		重启
		/etc/init.d/mysql restart
```

**SQLYog远程访问Ubuntu下的MySQL**
```java
1）在虚拟机中执行 ifconfig ，查看虚拟机ip地址。
2）在虚拟机中执行 mysql -u root -p ，打开mysql数据库。
3）然后执行 grant all privileges on *.* to root@'%' identified by '123456' WITH GRANT OPTION; 来获取权限。
4）再执行 quit ，退出mysql。
5）用vi预览/etc/mysql/mysql.conf.d/mysqld.cnf 或者 /etc/mysql/my.cnf文件，找到bind-address       = 127.0.0.1这一行代码并注释掉该语句。
6）保存退出vi。
7）执行service mysql restart命令，对数据库进行重启。
```

&emsp;&emsp;打开sqlyog，新建连接，并在mysql主机地址中填入虚拟机的ip地址，密码为123456，其他项为默认，如下图所示:

![enter image description here](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/sqlyog.png?raw=true)

先点击测试连接，若连接成功，然后再点connect连接。

## 在Ubuntu上部署web项目

&emsp;&emsp;通过在Windows下通过SSH的客户端连接Ubuntu下SSH的服务端，通过文件远程传输将web项目的war包传输到tomcat的webapps项目下（***ps：***最好用root用户连接 以防权限不足），然后通过浏览器访问Tomcat中的该web项目。
`若项目访问出错，可以在该Java Web中打印一条输出语句，通过查看Ubuntu中Tomcat的运行日志，找到问题所在`
```java
1、先切换到：cd /opt/apache-tomcat-7.0.77/logs/
2、tail -f catalina.out
3、这样运行时就可以实时查看运行日志了
 
Ctrl+c 是退出tail命令。
```
## 参考博文


[Ubuntu下安装配置和卸载Tomcat](http://zyjustin9.iteye.com/blog/2177291)
[MySQL在Linux Ubuntu中安装](http://blog.fens.me/linux-mysql-install/)
[Ubuntu系统之MySql+sqlyog安装配置教程](http://blog.csdn.net/a854073071/article/details/52117659)
[linux下实时查看tomcat运行日志](http://blog.sina.com.cn/s/blog_4f925fc30100q23f.html)

