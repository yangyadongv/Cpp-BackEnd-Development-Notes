## **PC**

- PC1> ip 192.168.10.2 255.255.225.0 192.168.10.10
  - 配置PC机的IP地址、子网掩码、网关
- PC1> show
  - 查看IP

## **路由器**

### 路由器上配置**以太网:fasetethernet**

- R1#configure terminal
  - 进入配置模式
- R1(config)#int
  R1(config)#interface fas
  R1(config)#interface fastEthernet 0/0
  - 进入路由器某个端口的配置
- R1(config-if)#ip address 192.168.10.10 255.255.255.0
  - 给路由器的一个端口配置IP地址和子网掩码
- R1(config-if)#shutdown
  - 关掉这个端口
- R1(config-if)# no shutdown
  - 打开这个端口
- R1(config-if)#ip address 192.168.50.10 255.255.255.0 secondery
  - 给这个端口添加第二个IP地址
- R1(config-if)#exit
  - R1(config)#
- R1(config)#exit
  - R1#

### 路由器上配置**广域网：串口，serial**

与局域网的区别在于，配置广域网接口时，DCE端得加上时钟（DCE端就相当于电信，DTE端相当于用户）

![image.png](https://i.loli.net/2020/11/20/APUEbV5m7IdkYRS.png)

#### DCE端（即上图R1的s2/0）

- R1#
- R1#show controllers serial 2/0
  - 里边会告诉你，这个路由端口是DCE还是DTE。DCE的话，就需要定义时钟频率
- R1#configure terminal
- R1(config)#interface serial 2/0
- R1(config-if)#clock rate ?
- R1(config-if)#clock rate 64000
- R1(config-if)#ip address 192.168.20.10 255.255.255.0
  - 加完时钟，还得再来一遍IP
- R1(config-if)#no shutdown

#### DTE端(即上图R2的s2/0)

- R2#
- R2#configure terminal
- R2(config)#interface serial 2/0
- R2(config-if)#no shutdown
- R2(config-if)#^Z
- R2#
- R2#ping 192.168.20.10
  - Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.20.10, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 12/20/24 ms



## **保存配置**

- R2#show running-config
  - 查看当前运行配置
- R2#copy running-config startup-config
  - 配置存盘
  - 
- PC2> save

## 路由器上添加路由表

添加R1的

- R1#show ip route

  - 查看路由表

  - C    192.168.10.0/24 is directly connected, FastEthernet0/0

    C    192.168.20.0/24 is directly connected, Serial2/0

- R1#configure terminal

- R1(config)#ip route 192.168.80.0 255.255.255.0 192.168.20.20

  - 想要去 网段192.168.80.0，子网掩码255.255.255.0，那么下一跳得是192.168.20.20

- R1(config)#ctrl+z

- R1#show ip route

  - C    192.168.10.0/24 is directly connected, FastEthernet0/0

    C    192.168.20.0/24 is directly connected, Serial2/0

    **S    192.168.80.0/24 [1/0] via 192.168.20.20**

  - 多了这条static的路由表。想去   192.168.80.0/24，那么下一跳得是192.168.20.20

添加R2的

- R2#show ip route
  Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
         D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
         N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
         E1 - OSPF external type 1, E2 - OSPF external type 2
         i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
         ia - IS-IS inter area, * - candidate default, U - per-user static route
         o - ODR, P - periodic downloaded static route

  Gateway of last resort is not set

  C    192.168.80.0/24 is directly connected, FastEthernet0/0
  C    192.168.20.0/24 is directly connected, Serial2/0

- R2#configure terminal
  Enter configuration commands, one per line.  End with CNTL/Z.

- R2(config)#ip route 192.168.10.0 255.255.255.0 192.168.20.10

- R2(config)#^Z

- R2#

- R2#show ip route
  Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
         D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
         N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
         E1 - OSPF external type 1, E2 - OSPF external type 2
         i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
         ia - IS-IS inter area, * - candidate default, U - per-user static route
         o - ODR, P - periodic downloaded static route

  Gateway of last resort is not set

  **S    192.168.10.0/24 [1/0] via 192.168.20.10**
  C    192.168.80.0/24 is directly connected, FastEthernet0/0
  C    192.168.20.0/24 is directly connected, Serial2/0

- 可以看到多了一条static