# IO

文件IO（磁盘IO）和网络IO是最主要的两种IO形式

## 1.硬盘、网卡、套接字



## 2.IO相关的系统调用

### 常见的系统调用

编程语言没有魔法，全都依赖操作系统的支持，其中最强大的支持莫过于系统调用。

- open//打开文件
- read //读取文件
- write //写文件
- sendFile //两个fd间传文件
- accept //接受客户端连接
- bind//服务端绑定ip端口
- connect/ /连接套接字服务
- select poll epoll  // io多路复用

### 如何知道一个进程使用了哪些系统调用

![image-20210225174552550](http://pichost.yangyadong.site/img/image-20210225174552550.png)



## 3.Java的NIO

![image-20210225175354207](http://pichost.yangyadong.site/img/image-20210225175354207.png)