# 在Ubuntu上搭建Java开发环境
本文将介绍在Linux的发行版本Ubuntu上搭建java环境 及部署java  web项目。作为个人的学习总结，方便今后复习查看。  
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
![Alt text](./QQ图片20170425101216.png)
更多内容请打开：[在Ubuntu上搭建Java开发环境](https://github.com/HelloJeremy/Ubuntu-Java/blob/master/%E5%9C%A8Ubuntu%E4%B8%8A%E6%90%AD%E5%BB%BAJava%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83.md)
