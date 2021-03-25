# linux高性能服务器编程

## 5.linux网络编程基础API

### 1.socket地址API

#### 主机字节序和网络字节序

- 主机字节序：即小端字节序

- 网络字节序：即大端字节序

- 现在PC默认都是小端

- 网络上的发送端，必须都用大端发送。

  - 因此如果是PC作为发送端，就一定需要进行字节序转换

- linux提供了4个函数完成字节序和网络序字节之间的转换

  - unsigned long int htonl()
    - host to network long。
    - 将long 整型的主机字节序数据转化为网络字节序数据
    - 一般用于转换IP地址
  - unsigned long int htons()
    - host to network short
    - 将short整型的主机字节序数据转化为网络字节序数据
    - 一般用于转换端口号
  - unsigned long int ntohl()
    - network to host long
    - 将long整型的网络字节序数据转化为主机字节序数据
  - unsigned long int ntohs()
    - network to host short
    - 将short 整型的网络字节序数据转化为主机字节序数据

- ```c
  // 主机序和网络字节序转换
  #include <netinet/in.h>
  unsigned long int htonl (unsigned long int hostlong); // host to network long
  unsigned short int htons (unsigned short int hostlong); // host to network short
  
  unsigned long int ntohl (unsigned long int netlong);//
  unsigned short int ntohs (unsigned short int netlong);//network to host short
  ```

#### socket地址（专用socket地址以及通用socket地址）

- 专用socket地址类型
  - UINX本地域协议族：struct sockaddr_un
  - IPv4：struct sockaddr_in
  - IPv6：struct sockaddr_in6
- 新的通用socket地址类型
  - struct sockaddr_storage
- 旧的通用socket地址类型
  - struct sockaddr
- 所有的专用socket地址类型以及新的通用socket地址类型（sockaddr_storage）类型的变量在实际使用中，必须得转化成旧的通用socket地址类型sockaddr类型（强制转换即可），因为所有socket编程接口使用的地址参数的类型都是旧的通用socket地址类型sockaddr。

#### IP地址转换函数（点分十进制字符串与整数类型之间的转换）

```c++
// IP地址转换函数
#include <arpa/inet.h>
// 将点分十进制字符串的IPv4地址, 转换为网络字节序整数表示的IPv4地址. 失败返回INADDR_NONE
in_addr_t  inet_addr( const char* strptr);

// 功能相同不过转换结果存在 inp指向的结构体中. 成功返回1 反之返回0
int inet_aton( const char* cp, struct in_addr* inp);

// 函数返回一个静态变量地址值, 所以多次调用会导致覆盖
char* inet_ntoa(struct in_addr in); 

// src为 点分十进制字符串的IPv4地址 或 十六进制字符串表示的IPv6地址 存入dst的内存中 af指定地址族
// 可以为 AF_INET AF_INET6 成功返回1 失败返回-1
int inet_pton(int af, const char * src, void* dst);
// 协议名, 需要转换的ip, 存储地址, 长度(有两个常量 INET_ADDRSTRLEN, INET6_ADDRSTRLEN)
const char* inet_ntop(int af, const void*  src, char* dst, socklen_t cnt);
```

### 2.创建socket（创建出来的是一个socket文件）

```c
// 创建socket
# include <sys/types.h>
# include <sys/socket.h>
// domain指定使用那个协议族 PF_INET PF_INET6 PF_UNIX
// type指定服务类型 SOCK_STREAM (TCP协议) SOCK_DGRAM(UDP协议)
// protocol设置为默认的0。几乎所有情况下，它都应该设置为0，表示默认。
// 成功返回socket文件描述符(linux一切皆文件), 失败返回-1
int socket(int domain, int type, int protocol);
//domain: 
//IPv4:PF_INET 
//IPv6:PF_INET6 
//UNIX本地域协议族：PF_UNIX
```

### 3.命名socket（将一个socket（文件）与socket地址绑定，bind函数）

- 创建socket时，仅仅指定了地址组，而未指定使用该地址组中的哪个具体socket地址

- 将一个socket与socket地址绑定称为给socket命名

- 对于服务端程序，通常要命名socket

  - 因为只有命名后，客户端才能知道该如何连接它

- 对于客户端程序，通常不需要命名socket，而是采用匿名方式

  - 即使用操作系统自动分配的socket地址

- ```c
  //命名socket
  # include <sys/types.h>
  # include <sys/socket.h>
  // socket为socket文件描述符
  // my_addr 为地址信息
  // addrlen为socket地址长度
  // 成功返回0 失败返回 -1
  int bind(int socket, const struct sockaddr* my_addr, socklen_t addrlen);
  ```

### 4.监听socket

- socket被命名（绑定，bind）之后，还不能马上接受客户连接，我们需要使用如下系统调用来创建一个监听队列以存放待处理的客户连接

- ```c
  //监听socket
  #include<sys/socket.h>
  // backlog表示队列最大的长度。其实最大是backlog值+1
  //listen成功时返回0，失败则返回-1并设置errno
  int listen(int socket, int backlog);
  //backlog参数是指处于ESTABLISHED即完全连接状态的socket的上限
  //处于半连接状态（SYN_RECV）的socket的上限则由/proc/sys/net/ipv4/tcp_max_syn_backlog内核参数定义
  ```

### 以上做个小结

```c
/**
 * g++ chapter5/5.4_testlisten.cpp -o testlisten.app && ./testlisten.app 127.0.0.1 12345 3
 * 
 * telnet 127.0.0.1 12345 # 多次执行
 * 
 * netstat -nt | grep 12345
 * 
 * 代码 5-3 backlog参数
 */

#include <iostream>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <signal.h>
#include <string.h>
#include <assert.h>
#include <unistd.h>
using namespace std;

static bool stop = false;

static void handle_term(int sig)
{
    stop = true;
}

int main(int argc, char *argv[])
{
    signal(SIGTERM, handle_term);//终止进程，kill命令默认发生的信号就是SIGTERM。发生这个动作时，调用handle_term程序，执行stop。
    if (argc <= 3)
    {
        cout << "Usage: " << argv[0] << " IP port backlog" << endl;
        return 1;
    }
    const char *ip = argv[1];
    int port = atoi(argv[2]);//字符串转数字
    int backlog = atoi(argv[3]);

    int sock = socket(PF_INET, SOCK_STREAM, 0);//创建IPv4，TCP的socket
    assert(sock > 0);
    // 创建一个IPv4 socket地址
    struct sockaddr_in address;//创建一个IPv4 socket地址
    bzero(&address, sizeof(address));//置零
    address.sin_family = AF_INET;//设置地址组
    inet_pton(AF_INET, ip, &address.sin_addr);//设置IPv4。IP地址转换函数
    address.sin_port = htons(port);//设置port。主机序转换为网络序

    // 命名socket
    int ret = bind(sock, (struct sockaddr *)&address, sizeof(address));//绑定socket文件和socket地址
    assert(ret != -1);

    // 开始监听
    cout << "backlog : " << backlog << endl;
    ret = listen(sock, backlog);//监听socket
    assert(ret != -1);

    while (!stop)//等待一开始的signal函数
    {
        sleep(1);
    }
    //关闭socket
    close(sock);

    return 0;
}
```

- ![image-20210307205513335](http://pichost.yangyadong.site/img/image-20210307205513335.png)
- ![image-20210307205532078](http://pichost.yangyadong.site/img/image-20210307205532078.png)
- 最大ESTAVLISHED数，即backlog+1

### 5.接受连接（服务器接受一个连接。成功后返回一个新的连接socket）

- ```c
  // 接受连接 失败返回-1 成功时返回socket
  int accept(int sockfd, struct sockaddr* addr, socklen_t* addrlen)
  ```

- accept只从监听队列中取出连接，而不论连接处于何种状态，不关心网络状况变化。

### 6.发起连接（客户端主动发起。成功返回0）

- 服务器通过listen调用来被动接受连接，客户端需要以下系统调用主动与服务器建立连接

- ```c
  // 发起连接
  #include <sys/types.h>
  #include <sys/socket.h>
  // 第三个参数为 地址指定的长度
  // 成功返回0 失败返回-1
  int connect(int sockfd, const struct sockaddr * serv_addr, socklen_t addrlen);
  ```

- sockfd：由socket系统调用返回一个socket

- serv_addr：服务器监听的socket地址

- addrlen：指定serv_addr这个地址的长度

- connect成功时返回0。一旦成功建立连接，sockfd就唯一地标识了这个连接，客户端就可以通过读写sockfd来与服务器通信。

- connect失败则返回-1并设置errno。其中常见的errno是

  - ECONNREFUSED：目标端口不存在，连接被拒绝
  - ETIMEDOUT：连接超时

### 7.关闭连接

- 关闭一个连接实际上就是关闭连接对应的socket。这可以通过如下关闭普通文件描述符系统调用完成

  - 

  ```c
  // 关闭连接。其实只是把fd引用计数减1
  #include <unistd.h>
  // 参数为保存的socket
  // 并非立即关闭, 将socket的引用计数-1, 当fd的引用计数为0, 才能关闭(需要查阅)
  int close(int fd);
  ```

- 立即关闭

  - ```c
    // 立即关闭。不是把fd引用计数减1，而是立即关闭
    #include <sys/socket.h>
    // 第二个参数为可选值 
    //	SHUT_RD 关闭读, socket的接收缓冲区的数据全部丢弃
    //	SHUT_WR 关闭写 socket的发送缓冲区全部在关闭前发送出去
    //	SHUT_RDWR 同时关闭读和写
    // 成功返回0 失败为-1 设置errno
    int shutdown(int sockfd, int howto)
    ```

  - howto

    - SHUT_RD：关闭读
    - SHUT_WR：关闭写
    - SHUT_RDWR：关闭读和写

### 8.数据读写

#### TCP数据读写

- ```c
  #include<sys/socket.h>
  #include<sys/types.h>
  
  // 读取sockfd的数据
  // buf 指定读缓冲区的位置
  // len 指定读缓冲区的大小
  // flags 参数较多
  // 成功的时候返回读取到的长度, 可能小于预期长度, 需要多次读取； 读取到0 通信对方已经关闭连接；错误返回-1
  ssize_t recv(int sockfd, void *buf, size_t len, int flags);
  // 发送
  ssize_t send(int sockfd, const void *buf, size_t len, int flags);
  ```

- sockfd：进行通信的socket

- buf：发送（接受）内容的地址（即内存位置）

- len：发送（接受）内容的长度

- flags

  - 

  | 选项名        | 含义                                                         | 可用于发送 | 可用于接收 |
  | ------------- | ------------------------------------------------------------ | ---------- | ---------- |
  | MSG_CONFIRM   | 指示链路层协议持续监听, 直到得到答复.(仅能用于SOCK_DGRAM和SOCK_RAW类型的socket) | Y          | N          |
  | MSG_DONTROUTE | 不查看路由表, 直接将数据发送给本地的局域网络的主机(代表发送者知道目标主机就在本地网络中) | Y          | N          |
  | MSG_DONTWAIT  | 非阻塞                                                       | Y          | Y          |
  | MSG_MORE      | 告知内核有更多的数据要发送, 等到数据写入缓冲区完毕后,一并发送.减少短小的报文提高传输效率 | Y          | N          |
  | MSG_WAITALL   | 读操作一直等待到读取到指定字节后才会返回                     | N          | Y          |
  | MSG_PEEK      | 看一下内缓存数据, 并不会影响数据                             | N          | Y          |
  | MSG_OOB       | 发送或接收紧急数据                                           | Y          | Y          |
  | MSG_NOSIGNAL  | 向读关闭的管道或者socket连接中写入数据不会触发SIGPIPE信号    | Y          | N          |

#### UDP数据读写

- ```c
  #include <sys/types.h>
  #include <sys/socket.h>
  // 由于UDP不保存状态, 每次发送数据都需要 加入目标地址.
  // 不过recvfrom和sendto 也可以用于 面向STREAM的连接, 这样可以省略发送和接收端的socket地址
  ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr* src_addr, socklen_t* addrlen);
  ssize_t sendto(int sockfd, const void* buf, size_t len, ing flags, const struct sockaddr* dest_addr, socklen_t addrlen);
  ```

#### 通用数据读写函数（既能用于TCP，又能用于UDP）

- ```c
  #inclued <sys/socket.h>
  ssize_t recvmsg(int sockfd, struct msghdr* msg, int flags);
  ssize_t sendmsg(int sockfd, struct msghdr* msg, int flags);
  
  struct msghdr
  {
  /* socket address --- 指向socket地址结构变量, 对于TCP连接需要设置为NULL*/
  	void* msg_name; 
  
  
  	socklen_t msg_namelen;
  	
  	/* 分散的内存块 --- 对于 recvmsg来说数据被读取后将存放在这里的块内存中, 内存的位置和长度由
       * msg_iov指向的数组指定, 称为分散读(scatter read)  ---对于sendmsg而言, msg_iovlen块的分散内存中
       * 的数据将一并发送称为集中写(gather write);
  	*/
  	struct iovec* msg_iov;
  	int msg_iovlen; /* 分散内存块的数量*/
  	void* msg_control; /* 指向辅助数据的起始位置*/
  	socklen_t msg_controllen; /* 辅助数据的大小*/
  	int msg_flags; /* 复制函数的flags参数, 并在调用过程中更新*/
  };
  
  struct iovec
  {
  	void* iov_base /* 内存起始地址*/
  	size_t iov_len /* 这块内存长度*/
  }
  ```

### 9.判断带外标记（紧急数据）

```c
#include <sys/socket.h>
// sockatmark用于判断 sockfd是否处于带外标记, 即下一个被读取到的数据是否是带外数据, 
// 是的话返回1, 不是返回0
// 这样就可以选择带MSG_OOB标志的recv调用来接收带外数据. 
//带外数据即紧急数据
int sockatmark(int sockfd);
```

### 10.地址信息函数（查询连接socket的本端和远端地址）

```c
// getsockname 获取sockfd对应的本端socket地址, 存入address指定的内存中, 长度存入address_len中 成功返回0失败返回-1
// getpeername 获取远端的信息, 同上
int getsockname(int sockfd, struct sockaddr* address, socklen_t* address_len);
int getpeername(int sockfd, struct sockaddr* address, socklen_t* address_len);
```

### 11.设置socket文件的属性（socket选项）

```c
// sockfd 目标socket, level执行操作协议(IPv4, IPv6, TCP) option_name 参数指定了选项的名字. 后面值和长度
// 成功时返回0 失败返回-1
int getsockopt(int sockfd, int level, int option_name, void* option_value, 
						socklen_t restrict option_len);
int setsockopt(int sockfd, int level, int option_name, void* option_value, 
						socklen_t restrict option_len);
```

![](https://lsmg-img.oss-cn-beijing.aliyuncs.com/Linux%E9%AB%98%E6%80%A7%E8%83%BD%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%BC%96%E7%A8%8B%E8%AF%BB%E4%B9%A6%E8%AE%B0%E5%BD%95/socket%E9%80%89%E9%A1%B9.jpg)

| 选项名字     | 作用                                       | 备注                                                         |
| ------------ | ------------------------------------------ | ------------------------------------------------------------ |
| SO_REUSEADDR | 重用本地地址                               | sock被设置此属性后, 即使sock在被bind()后处于TIME_WAIT状态, 此时与他绑定的socket地址依然能够立即重用来绑定新的sock |
| SO_RCVBUF    | TCP接收缓冲区大小                          | 最小值为256字节. 设置完后系统会自动加倍你所设定的值. 多出来的一倍将用用作空闲缓冲区处理拥塞 |
| SO_SNDBUF    | TCP发送缓冲区大小                          | 最小值为2048字节                                             |
| SO_RCVLOWAT  | 接收的低水位标记                           | 默认为1字节, 当TCP接收缓冲区中可读数据的总数大于其低水位标记时, IO复用系统调用将通知应用程序可以从对应的socket上读取数据 |
| SO_SNDLOWAT  | 发送的高水位标记                           | 默认为1字节, 当TCP发送缓冲区中空闲空间大于低水位标记的时候可以写入数据 |
| SO_LINGER    | 用于控制close系统调用在关闭TCP连接时的行为 |                                                              |

SO_LINGER选项：

```c
struct linger
{
	int l_onoff /* 开启非0, 关闭为0*/
	int l_linger; /* 滞留时间*/
	/*
	* 当onoff为0的时候此项不起作用, close调用默认行为关闭socket
	* 当onoff不为0 且linger为0, close将立即返回, TCP将丢弃发送缓冲区的残留数据, 同时发送一个复位报文段
	* 当onoff不为0 且linger大于0 . 当socket阻塞的时候close将会等待TCP模块发送完残留数据并得到确认后关 
	* 闭, 如果是处于非阻塞则立即关闭
	*/
};
```

### 12.网络信息API

#### gethostbyname & gethostbyaddr

```c
#include <netdb.h>
// 通过主机名查找ip
struct hostent* gethostbyname(const char* name);

// 通过ip获取主机完整信息。addr即目标主机的IP地址 
// type为IP地址类型 AF_INET和AF_INET6
struct hostent* gethostbyaddr(const void* addr, size_t len, int type);

//hostent结构体
struct hostent
{
  char *h_name;			/* Official name of host.  */
  char **h_aliases;		/* Alias list.  */
  int h_addrtype;		/* Host address type.  */
  int h_length;			/* Length of address.  */
  char **h_addr_list;		/* List of addresses from name server.  */
}
```

#### getservbyname & getservbyport

```c
#include <netdb.h>
// 根据名称获取某个服务的完整信息
struct servent getservbyname(const char* name, const char* proto);

// 根据端口号获取服务信息
struct servent getservbyport(int port, const char* proto);

struct servent
{
	char* s_name; /* 服务名称*/
	char ** s_aliases; /* 服务的别名列表*/
	int s_port; /* 端口号*/
	char* s_proto; /* 服务类型, 通常为TCP或UDP*/
}
```

#### getaddrinfo（对gethostbyname和getservbyname的封装）

```c
#include <netdb.h>
// 内部使用的gethostbyname 和 getserverbyname
// hostname 用于接收主机名, 也可以用来接收字符串表示的IP地址(点分十进制, 十六进制字符串)
// service 用于接收服务名, 字符串表示的十进制端口号
// hints参数 对getaddrinfo的输出进行更准确的控制, 可以设置为NULL, 允许反馈各种有用的结果
// result 指向一个链表, 用于存储getaddrinfo的反馈结果
int getaddrinfo(const char* hostname, const char* service, const struct addrinfo* hints, struct addrinfo** result)
    
struct addrinfo
{
	int ai_flags;
	int ai_family;
	int ai_socktype; /* 服务类型, SOCK_STREAM或者SOCK_DGRAM*/
	int ai_protocol;
	socklen_t ai_addrlen;
	char* ai_canonname; /* 主机的别名*/
	struct sockaddr* ai_addr; /* 指向socket地址*/
	struct addrinfo* ai_next; /* 指向下一个结构体*/
}

// 需要手动的释放堆内存
void freeaddrinfo(struct addrinfo* res);
```

![](https://ftp.bmp.ovh/imgs/2019/08/7ebedb14d8eedeac.png)

#### getnameinfo（对gethostbyaddr和getservbyprot的封装）

```c
#include <netdb.h>
// host 存储返回的主机名
// serv存储返回的服务名

int getnameinfo(const struct sockaddr* sockaddr, socklen_t addrlen, char* host, socklen_t hostlen, char* serv
	socklen_t servlen, int flags);
```

![](https://ftp.bmp.ovh/imgs/2019/08/bc7196e9a30d5152.png)

## 8.高性能服务器程序框架

### 1.服务器模型

### 2.服务器编程框架

### 3.IO模型

阻塞IO和非阻塞IO

- 阻塞IO
  - 针对阻塞IO执行的系统调用，可能因为无法立即完成而被操作系统挂起
- 非阻塞IO
  - 针对非阻塞IO执行的系统调用总是立即返回，而不管事件是否已经发生

同步IO和异步IO

- 同步IO
  - 要求用户代码自行执行IO操作（将数据从内核缓冲区读入用户缓冲区，或将数据从用户缓冲区写入内核缓冲区）
  - 同步IO向应用程序通知的是IO就绪事件：即IO可用不可用，轮没轮到本应用程序使用
- 异步IO
  - 由内核执行真正的IO操作（数据在内核缓冲区和用户缓冲区之间的移动是由内核在“后台”完成的）
  - 必须立即返回。
  - 异步IO向用应用程序通知的是IO完成事件：IO操作完成了没有，内核替本应用程序做完了没有

### 4.事件处理模式

- Reactor
- Proactor

### 5.并发模式

- 半同步/半异步模式
- 领导者/追随者模式

### 10.总结

- IO处理单元
  - 4种IO模型
    - 阻塞IO、非阻塞IO、同步IO、异步IO
  - 2种高效事件处理模式
    - Reactor
    - Proactor
- 逻辑单元
  - 2种高效并发模式
    - 半同步半异步
    - 领导者追随者
  - 高效的逻辑处理方式：有限状态机