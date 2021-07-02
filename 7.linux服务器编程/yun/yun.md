## 技术栈

- FastDFS：
  - 分布式的文件系统
- redis、mysql
  - redis：内存数据库
  - mysql：硬盘数据库
- nginx:
  - web服务器
- FastCGI
  - 处理post请求（动态请求）
- QT
  - QListWidget
  - QJsonDocument
  - QNetworkAccessManager
    - 处理http协议

## 整体架构

- ![image-20210512215830750](https://i.loli.net/2021/05/12/R6wHBrpO2sNtSdk.png)
- 通过浏览器/桌面客户端访问服务器
  - c/s
  - b/s
- 高并发
  - 需要多台服务器，比如100台服务器集群
  - 因此需要一个反向代理服务器（nginx）
  - 反向代理服务器平均地给这100台服务器分配客户请求。以使得负载均衡
- nginx服务器+fastcgi
  - nginx处理静态请求
  - 动态请求由fastcgi框架编写程序(application)处理
- redis里存储用户频繁访问的数据
  - 例如：每个用户云盘里的文件名
- mysql里存放不经常访问的数据
  - 例如：用户名密码
  - 数据库有连接上限，因此不可能往里边存图片等东西。
- 分布式文件系统
  - fastDFS
  - 存储 视频、图片、音乐等文件，这些文件并不是存到数据库里。

## web服务器

- 什么是服务器
  - 硬件层次：一台配置很高的电脑
  - 软件层次：这台电脑装的是服务器软件
- 常见的服务器软件
  - tomcat服务器
    - java语言常用
  - IIS服务器
    - 微软公司主推的服务器
  - nginx
    - 小巧且高效的HTTP服务器
      - 它可以直接调php的代码，但不能直接调c++。因此需要fastcgi
    - 也可以做高效的负载均衡、反向代理
- 什么是web服务器
  - 能解析http协议的服务器
  - ![image-20210512221747294](https://i.loli.net/2021/05/12/MHyDRBPTjfipNon.png)

## 分布式文件系统

![image-20210512222615908](https://i.loli.net/2021/05/12/y9YaTvUDcpjWPGn.png)



- fastDFS
  - 可以很容易搭建一套高性能的文件服务器集群提供文件上传、下载等服务
  - 它是应用层级的文件系统软件，因此它不能挂载和弹出
- fastDFS里的3个角色
  - tracker：追踪器（在服务器上）
    - 其实就是管理文件系统，它是管理者。类似于反向代理服务器。
  - storage：存储节点（在服务器上）
  - client：客户（就是用户）
- fastDFS中3个角色之间的关系
  - client和storage主动连接tracker
    - 客户端去连接tracker是为了上传文件或者下载文件。tracker收到客户端连接，然后把合适的或者说对应的storage的IP和端口返回给客户端。然后客户端直接去跟对应的storage进行socket通信。
    - storage去连接tracker，是为了主动汇报自己的存储情况。
  - storage主动向tracker报告其状态信息
    - 磁盘剩余空间
    - 文件同步状况
    - 文件上传下载次数
  - storage会启动一个单独的线程来完成对一台tracker的连接和定时报告
    - 报告它的剩余磁盘空间、文件同步状况等
  - 一个组包含的storage不是通过配置文件设定的，而是通过tracker获取到的
- ![image-20210512224907491](https://i.loli.net/2021/05/12/VeRDZUMYNT4B8cr.png)

