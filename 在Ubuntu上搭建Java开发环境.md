# 在Ubuntu上搭建Java开发环境
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

![enter image description here](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/Xshell.png?raw=true)

&emsp;&emsp;**xftp远程文件传输：**（*协议选SFTP*）

![enter image description here](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/xftp.png?raw=true)

## Java环境搭建

<img src="https://github.com/HelloJeremy/Ubuntu-Java/blob/master/images/xftp.png?raw=true" width = "300" height = "120" alt="CSDN图标" /> 

