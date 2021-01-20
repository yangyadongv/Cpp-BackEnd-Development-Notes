# 1.linux_韩立刚

## 1.终端与shell

Terminal is now the software program inside the Console.

终端现在是控制台内部的软件程序。在软件世界里，终端和控制台实际上是同义词。

### 1.1终端

- **虚拟终端（没用过）**
  - 似乎是无GUI版本的linux才有
  - 虚拟终端是由mingetty程序产生（模拟的是多套操作人员，或者说多套键鼠设备，共同操作这个操作系统）
  - 切换终端ctrl+alt+fn   n=1-6
  - 命令行里输入tty显示：
    - /dev/tty1
    - /dev/tty2
    - ....
- **模拟终端（用的都是）**
  - 在ubuntu上打开的终端
  - ssh上去的终端
  - 命令行里输入tty显示：
    - /dev/pts/0
    - /dev/pts/1
    - .....
  - ![image.png](https://i.loli.net/2020/12/05/u5RMTZUfF79jh1k.png)
  - **我们平时用的terminal，都属于模拟终端**

### 1.2linux的shell

- **1.图像界面shell** GUI（Graphical User Interface，图形用户界面）: 

- （类似于windows的Windows Explorer .exe）

  - Gnome ：C语言开发

  - KDE： C++开发

  - Xface： 轻量级图像界面

  - 当前linux使用的是什么GUI shell呢？

    - 查看linux有哪些GUI shell可供用户使用

      - ```shell
        yangyadong@ubuntu:~$ ls /usr/bin/*session
        /usr/bin/dbus-run-session  /usr/bin/gnome-session-custom-session
        /usr/bin/gnome-session
        yangyadong@ubuntu:~$
        ```

    - 查看当前使用的GUI shell（ 此命令只能进入桌面系统后，在桌面系统启动命令窗口执行才能得到结果，使用SecureCRT工具连接到系统，执行此命名得不到任何结果。）

      - ```shell
        yangyadong@ubuntu:~$ echo $DESKTOP_SESSION
        gnome
        yangyadong@ubuntu:~$
        ```

- **2.命令行式shell** CLI（command-line interface，命令行界面）

- （类似于windows的cmd.exe，Windows PowerShell）

  - shell的功能：命令输入、交互、编程、

  - bsh

  - sh

  - csh

  - ksh

  - bash

  - 当前linux使用的是什么GUI shell呢？

    - 查看linux有哪些CLI shell可供用户使用

      - ```shell
        yangyadong@ubuntu:~$ cat /etc/shells
        # /etc/shells: valid login shells
        /bin/sh
        /bin/bash
        /bin/rbash
        /bin/dash
        yangyadong@ubuntu:~$
        ```

      - 可以给不同用户使用不同的CLI shell

    - 查看当前使用的CLI shell

      - ```shell
        yangyadong@ubuntu:~$ echo $SHELL
        /bin/bash
        yangyadong@ubuntu:~$
        ```

传统意义上的shell指的是命令行式的shell，**以后如果不特别注明，shell是指命令行式的shell**。

文字操作系统与外部最主要的接口就叫做shell。

shell是操作系统最外面的一层。

shell管理你与操作系统之间的交互：等待你输入，向操作系统解释你的输入，并且处理各种各样的操作系统的输出结果。

shell提供了你与操作系统之间通讯的方式。这种通讯可以以交互方式（从键盘输入，并且可以立即得到响应），或者以shell script(非交互）方式执行。shell script是放在文件中的一串shell和操作系统命令，它们可以被重复使用。本质上，**shell script是命令行命令简单的组合到一个文件里面**。

## 2.Linux终端与shell基础命令

### 2.1命令行编辑

- 光标快速移动
  - ctrl+a 快速跳转到行首
  - ctrl+e 快速跳转到行尾
- 删除命令行中内容
  - ctrl+w 删除光标前一个单词
  - ctrl+u 删除光标到行首的字符
  - ctrl+k 删除光标到行尾的字符
- 清屏
  - ctrl+l
  - （windows下是cls）
- 取消不执行的命令
  - ctrl+c

### 2.2内部命令和外部命令`type pwd`

- 内部命令：shell程序自带的命令。比如 cd、ls、pwd等

- 外部命令：在系统的某个路径下的可执行程序。比如ping、hugo、docker等

  - 外部命令的查找，依赖于PATH变量

  - 系统只会在这些目录里寻找可执行程序

  - ```shell
    yangyadong@ubuntu:~$ echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
    yangyadong@ubuntu:~$
    ```

  - 查看一个命令是内部命令还是外部命令（type命令）

  - ```shell
    yangyadong@ubuntu:~$ type pwd
    pwd 是 shell 内建
    yangyadong@ubuntu:~$
    ```

  - ```shell
    yangyadong@ubuntu:~$ type ping
    ping 是 /bin/ping
    yangyadong@ubuntu:~$ type docker
    docker 是 /usr/bin/docker
    yangyadong@ubuntu:~$
    ```

### 2.3命令补全和路径补全TAB键

- 1.hash表缓存

  - 命令都是在$PATH的路径里搜的，为了加速搜索。**执行过的命令，就会暂存的hash表里，下次再使用时就快了。**使用hash命令可以查看当前缓存的命令

  ```shell
  yangyadong@ubuntu:~$ hash
  命中    命令
     1    /usr/bin/tty
     1    /usr/bin/which
     5    /sbin/ifconfig
     1    /bin/cat
  yangyadong@ubuntu:~$
  
  ```

- 要是存在hash表里的命令或者叫可执行文件，被挪地方了，但是系统还是按hash表去读，就会报错，找不到那个命令或者叫可执行文件了。

  - 这种情况下就应该删除原来的hash记录，再使用该命令

  - 比如，直接清空所有缓存

  - ```shell
    yangyadong@ubuntu:~$ hash -r
    yangyadong@ubuntu:~$ hash
    hash：哈希表为空
    yangyadong@ubuntu:~$
    ```

- 2.命令补全

  - TAB键

- 3.路径补全

  - TAB键

### 2.4命令历史`history`

- ```shell
  yangyadong@ubuntu:~$ history
      1  ./vmware-install.pl
      2  sudo ./vmware-install.pl
      3  ./vmware-install.pl
      4  ifconfig
      5  ping 192.168.80.1
      6  ping 192.168.80.2
      7  ping 192.168.80.1
      8  ps -e | grep apt
      9  sudo kill 2705 2710
     10  getdit
     11  vim
     12  sudo apt install vim
  yangyadong@ubuntu:~$
  ```

  

### 2.5文件名通配符`*、?、[[:digit:]]`

通配符：特殊的字符，这些字符不表示它的表面意义，而是能够匹配符合指定特征的字符

- ```
  * 代表任意长度的字符
  ```

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ touch a ab  aab cab  adb ayb a91
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a91  aab  ab  adb  ayb  cab
    #列出当前路径下所有a打头的文件
    yangyadong@ubuntu:~/yydTestDir$ ls a*
    a  a91  aab  ab  adb  ayb
    #列出当前当前路径下所有a打头b结尾的文件
    yangyadong@ubuntu:~/yydTestDir$ ls a*b
    aab  ab  adb  ayb
    yangyadong@ubuntu:~/yydTestDir$
    ```

- ```
  ? 代表任意单个字符
  ```

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ touch a ab  aab cab  adb ayb a91
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a91  aab  ab  adb  ayb  cab
    #a某b，中间那个任意，但是必须是一个字符
    yangyadong@ubuntu:~/yydTestDir$ ls a?b
    aab  adb  ayb
    yangyadong@ubuntu:~/yydTestDir$
    ```

- ```
  [] 代表一个指定范围的单个字符
  
  [[:space:]] 空格
  [[:digit:]] [0-9]
  [[:lower:]] [a-z]
  [[:upper:]] [A-Z]
  [[:alpha:]] [a-z||A-Z]
  
  
  排除用^
  比如
  [^[:alpha:]] 非字母
  ```

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ touch a ab  aab cab  adb ayb a91
    #-------------第一个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a91  aab  ab  adb  ayb  cab
    #a打头，中间是[abcd]，结尾是b。中间那个只能是abcd里的一个
    yangyadong@ubuntu:~/yydTestDir$ ls a[abcd]b
    aab  adb
    #ayb没列出
    yangyadong@ubuntu:~/yydTestDir$
    #-------------第二个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a91  aab  ab  adb  ayb  cab
    #a打头，中间是[a-z里任意一个]，结尾是b。
    yangyadong@ubuntu:~/yydTestDir$ ls a[a-z]b
    aab  adb  ayb
    yangyadong@ubuntu:~/yydTestDir$
    #-------------第三个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ touch a2b a3b a4b a9b
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a2b  a3b  a4b  a91  a9b  aab  ab  adb  ayb  cab
    #a打头，中间是[0-9里任意一个]，结尾是b。
    yangyadong@ubuntu:~/yydTestDir$ ls a[0-9]b
    a2b  a3b  a4b  a9b
    yangyadong@ubuntu:~/yydTestDir$
    #-------------第四个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ ls
    a  a2b  a3b  a4b  a91  a9b  aab  ab  adb  ayb  cab
    #a打头，中间是[除了0-5的]，结尾是b。
    yangyadong@ubuntu:~/yydTestDir$ ls a[^0-5]b
    a9b  aab  adb  ayb
    yangyadong@ubuntu:~/yydTestDir$
    #-------------第五个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ touch 'a b'
    yangyadong@ubuntu:~/yydTestDir$ ls
     a   a2b   a3b   a4b   a91   a9b   aab   ab  'a b'   adb   ayb   cab
    #a打头，中间是[空格]，结尾是b。
    yangyadong@ubuntu:~/yydTestDir$ ls a[[:space:]]b
    'a b'
    yangyadong@ubuntu:~/yydTestDir$
    #-------------第六个例子-----------------
    yangyadong@ubuntu:~/yydTestDir$ ls
     a   a2b   a3b   a4b   a91   a9b   aab   ab  'a b'   adb   ayb   cab
     #a打头，中间是[数字]，结尾是b。
    yangyadong@ubuntu:~/yydTestDir$ ls a[[:digit:]]b
    a2b  a3b  a4b  a9b
    yangyadong@ubuntu:~/yydTestDir$
    ```

### 2.6命令别名`alias pingbd='ping www.baidu.com '`

- 作用：用一个别名来代替长命令

  - ```shell
    #查看一下alias命令是内部命令还是外部命令。内建说明是shell程序自带的命令
    yangyadong@ubuntu:~$ type alias
    alias 是 shell 内建
    #查看一下现在有哪些命令别名
    yangyadong@ubuntu:~$ alias
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    yangyadong@ubuntu:~$
    ```

- 添加一个别名`yangyadong@ubuntu:~$alias pingbaidu='ping www.baidu.com'`

  - ```shell
    #添加一个别名pingbaidu
    yangyadong@ubuntu:~$ alias pingbaidu='ping www.baidu.com'
    yangyadong@ubuntu:~$ alias
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias pingbaidu='ping www.baidu.com'
    #可以看到已经加上了
    #并且可以使用
    yangyadong@ubuntu:~$ pingbaidu
    PING www.a.shifen.com (182.61.200.7) 56(84) bytes of data.
    64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=1 ttl=128 time=43.2 ms
    64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=2 ttl=128 time=43.9 ms
    64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=3 ttl=128 time=39.2 ms
    ^C
    --- www.a.shifen.com ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 3130ms
    rtt min/avg/max/mdev = 39.246/42.135/43.959/2.066 ms
    yangyadong@ubuntu:~$ 
    ```

- 删除一个别名`yangyadong@ubuntu:~$unalias pingbaidu'`

- 添加的别名可以跟已有命令重名

  - ```shell
    #添加的别名可以跟已有命令重名
    yangyadong@ubuntu:~$ alias ifconfig='ifconfig ens33'
    yangyadong@ubuntu:~$ ifconfig
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.80.131  netmask 255.255.255.0  broadcast 192.168.80.255
            inet6 fe80::8635:4e7:fbe3:97c7  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:ab:1a:9e  txqueuelen 1000  (以太网)
            RX packets 694909  bytes 808859069 (808.8 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 165348  bytes 33744150 (33.7 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    yangyadong@ubuntu:~$
    #不加反斜杠就是使用别名，加一个反斜杠\就是使用原始命令而不是别名
    yangyadong@ubuntu:~$ \ifconfig
    br-85202a791bb1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 172.19.0.1  netmask 255.255.0.0  broadcast 172.19.255.255
            inet6 fe80::42:71ff:fe84:d126  prefixlen 64  scopeid 0x20<link>
            ether 02:42:71:84:d1:26  txqueuelen 0  (以太网)
            RX packets 8485  bytes 10916408 (10.9 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 15178  bytes 8482940 (8.4 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    br-8e07f84319c7: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
            inet 172.18.0.1  netmask 255.255.0.0  broadcast 172.18.255.255
            inet6 fe80::42:8cff:fe24:4a7c  prefixlen 64  scopeid 0x20<link>
            ether 02:42:8c:24:4a:7c  txqueuelen 0  (以太网)
            RX packets 5037  bytes 7277443 (7.2 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 9156  bytes 5597771 (5.5 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
            inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
            ether 02:42:51:ad:d3:d7  txqueuelen 0  (以太网)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.80.131  netmask 255.255.255.0  broadcast 192.168.80.255
            inet6 fe80::8635:4e7:fbe3:97c7  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:ab:1a:9e  txqueuelen 1000  (以太网)
            RX packets 694931  bytes 808860689 (808.8 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 165360  bytes 33745790 (33.7 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            inet6 ::1  prefixlen 128  scopeid 0x10<host>
            loop  txqueuelen 1000  (本地环回)
            RX packets 54691  bytes 3543680 (3.5 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 54691  bytes 3543680 (3.5 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    veth80ac3ef: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::a08b:77ff:feb2:ca26  prefixlen 64  scopeid 0x20<link>
            ether a2:8b:77:b2:ca:26  txqueuelen 0  (以太网)
            RX packets 25797  bytes 14257820 (14.2 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 30277  bytes 20063505 (20.0 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    vethd08d6e6: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::14eb:32ff:fef2:2837  prefixlen 64  scopeid 0x20<link>
            ether 16:eb:32:f2:28:37  txqueuelen 0  (以太网)
            RX packets 14899  bytes 11552705 (11.5 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 17831  bytes 3304301 (3.3 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    yangyadong@ubuntu:~$
    ```

- 以上是在命令行里，添加的命令别名，这种方法只在这个shell的生命周期内生效。也就是说，你这个shell关了，再开一个shell，之前的alias全部失效了，全部清空。要想一直生效，得改bash shell的配置文件 .bashrc。

- **别名永久生效的方法**

  - **1.仅修改本用户的shell配置文件，其他用户保持系统默认**

    - ```shell
      #直接输入cd，进入本账户主目录
      yangyadong@ubuntu:~$ cd
      #查看当前路径
      yangyadong@ubuntu:~$ pwd
      /home/yangyadong
      #ls -a显示隐藏文件
      yangyadong@ubuntu:~$ ls -a
      .              .bashrc  Documents         .gnupg         Music             .profile                   Templates  .xinputrc
      ..             .cache   Downloads         .ICEauthority  .pam_environment  Public                     Videos     yydDir
      .bash_history  .config  examples.desktop  .local         Pictures          .selected_editor           .viminfo   yydTestDir
      .bash_logout   Desktop  .gitconfig        .mozilla       .pki              .sudo_as_admin_successful  .w3m
      #修改.bashrc的内容
      yangyadong@ubuntu:~$ vim .bashrc
      ```

    - ![image.png](https://i.loli.net/2020/12/08/fl26Ir1cowdS9Cu.png)

    - ```shell
      #使得修改生效
      yangyadong@ubuntu:~$ source .bashrc
      yangyadong@ubuntu:~$ alias
      alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
      alias egrep='egrep --color=auto'
      alias fgrep='fgrep --color=auto'
      alias grep='grep --color=auto'
      alias l='ls -CF'
      alias la='ls -A'
      alias ll='ls -alF'
      alias ls='ls --color=auto'
      alias pingbaidu='ping www.baidu.com'
      yangyadong@ubuntu:~$
      #这样的话，在yangyadong这个账户下，这个alias出来的 pingbaidu命令将永远生效，但是一旦你切了用户，比如切到了sudo su，那这个别名就没了。因为你修改的是特定账户的的bashshell配置，即/home/yangyadong/.bashrc
      ```

  - **2.修改系统的.bashec，使得所有用户都生效**

    - ```c++
      yangyadong@ubuntu:~$ sudo vim /etc/bash.bashrc
      [sudo] yangyadong 的密码：
      ```

    - ![image.png](https://i.loli.net/2020/12/08/BiXG2wjUA93JEYN.png)

    - ```shell
      #使得修改生效
      yangyadong@ubuntu:~$ source /etc/bash.bashrc
      #查看别名
      yangyadong@ubuntu:~$ alias
      alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
      alias egrep='egrep --color=auto'
      alias fgrep='fgrep --color=auto'
      alias gongwang='curl ifconfig.me'
      alias grep='grep --color=auto'
      alias l='ls -CF'
      alias la='ls -A'
      alias ll='ls -alF'
      alias ls='ls --color=auto'
      alias pingbaidu='ping www.baidu.com'
      
      #可以看到gongwang这个别名已经对所有用户都生效了
      yangyadong@ubuntu:~$
      yangyadong@ubuntu:~$ gongwang
      115.156.215.180
      yangyadong@ubuntu:~$
      ```

    - ```shell
      yangyadong@ubuntu:~$ sudo su
      #可以看到在root账号下，只有修改系统配置/etc/bash.bashrc产生的别名gongwang，而没有修改yangyadong账户/home/yangyadong/.bashrc产生的别名pingbaidu
      root@ubuntu:/home/yangyadong# alias
      alias egrep='egrep --color=auto'
      alias fgrep='fgrep --color=auto'
      alias gongwang='curl ifconfig.me'
      alias grep='grep --color=auto'
      alias l='ls -CF'
      alias la='ls -A'
      alias ll='ls -alF'
      alias ls='ls --color=auto'
      root@ubuntu:/home/yangyadong#
      ```


### 2.7命令替换  $(命令)或者\``命令`\`

使用命令执行的结果替换该命令。命令替换只能出现在弱引用中，强引用是不行的。

- 方式1：$(命令)

- 方式2：使用反引号\`命令\`

  - 解释一下

  - ```
    双引号: " "    是弱引用
    单引号: ' '    是强引用
    反引号: ` `    是一种实现命令替换的方式
    美元:   $()    也是一种实现命令替换的方式，注意$()跟只有一个$是不一样的。
    ```

- 举例1

  - ```shell
    #$(pwd)
    yangyadong@ubuntu:~$ pwd
    /home/yangyadong
    yangyadong@ubuntu:~$ echo "The current directory is $(pwd)"
    The current directory is /home/yangyadong
    yangyadong@ubuntu:~$
    ```

- 举例2

  - ```shell
    #$(data +%Y_%m_%d)
    yangyadong@ubuntu:~/yydTestDir$ touch yydFile_$(date +%Y_%m_%d)
    yangyadong@ubuntu:~/yydTestDir$ ls
    yydFile_2020_12_09
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - ```shell
    #$(date +%Y_%m_%d_%H_%M_%S)
    yangyadong@ubuntu:~/yydTestDir$ touch "yydFile_$(date +%Y_%m_%d_%H_%M_%S)"
    yangyadong@ubuntu:~/yydTestDir$ ls
    yydFile_2020_12_09_07_25_19
    yangyadong@ubuntu:~/yydTestDir$ touch "yydFile_$(date +%Y_%m_%d_%H_%M_%S)""
    yangyadong@ubuntu:~/yydTestDir$ ls
    yydFile_2020_12_09_07_25_19  yydFile_2020_12_09_07_25_23
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 举例3

  - ```shell
    #使用反引号
    #`data +%Y_%m_%d`
    yangyadong@ubuntu:~/yydTestDir$ ls -a
    .  ..
    yangyadong@ubuntu:~/yydTestDir$ touch "yydFile_`date +%Y_%m_%d`"
    yangyadong@ubuntu:~/yydTestDir$ ls
    yydFile_2020_12_09
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 举例4

  - bash中：双引号 “ ” 是弱引用，单引号 ' ' 是强引用。

  - 即双引号，引起来的东西，可以有命令替换

  - 单引号引起来的东西，不可以有命令替换，它本身是什么样就是什么样

  - ```shell
    #双引号 " "+命令替换，成功。因为双引号是弱引用
    yangyadong@ubuntu:~/yydTestDir$ echo "The current directory is $(pwd)"
    The current directory is /home/yangyadong/yydTestDir
    yangyadong@ubuntu:~/yydTestDir$ echo "The current directory is `pwd`"
    The current directory is /home/yangyadong/yydTestDir
    #单引号' '+命令替换，失败。因为单引号是强引用
    yangyadong@ubuntu:~/yydTestDir$ echo 'The current directory is $(pwd)'
    The current directory is $(pwd)
    yangyadong@ubuntu:~/yydTestDir$ echo 'The current directory is `pwd`'
    The current directory is `pwd`
    yangyadong@ubuntu:~/yydTestDir$
    ```

### 2.8路径展开`{}`

- /tmp/{a,b} 等价于 /tmp/a和/tmp/b

  - 举例1

  ```shell
  #./{a.b}  等价于 ./a和./b
  #mkdir -v是显示创建过程
  yangyadong@ubuntu:~/yydTestDir$ mkdir ./a ./b -v
  mkdir: 已创建目录 './a'
  mkdir: 已创建目录 './b'
  yangyadong@ubuntu:~/yydTestDir$ ls
  a  b
  yangyadong@ubuntu:~/yydTestDir$ rmdir ./a ./b
  yangyadong@ubuntu:~/yydTestDir$ ls -a
  .  ..
  yangyadong@ubuntu:~/yydTestDir$ mkdir ./{a,b} -v
  mkdir: 已创建目录 './a'
  mkdir: 已创建目录 './b'
  yangyadong@ubuntu:~/yydTestDir$ ls
  a  b
  yangyadong@ubuntu:~/yydTestDir$
  ```

  - 举例2：{}{}类似于多项式乘法

  - ```shell
    #{a,b}{c,d} = ac,ad,bc,bd
    yangyadong@ubuntu:~/yydTestDir$ mkdir /tmp/{a,b}{c,d} -v
    mkdir: 已创建目录 '/tmp/ac'
    mkdir: 已创建目录 '/tmp/ad'
    mkdir: 已创建目录 '/tmp/bc'
    mkdir: 已创建目录 '/tmp/bd'
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 举例3:想创建一个/tmp/zz/a/b/  一个/tmp/yy/a/b。怎么简洁一点

  - ```shell
    #/tmp/{zz,yy}/a/b即/tmp/zz/a/b/ 和/tmp/yy/a/b
    #mkdir -v是显示创建过程 -p是递归创建
    #为啥一定要-p递归创建，因为/tmp/zz这层目录不创建的话，你没法创建/tmp/zz/a，更别说/tmp/zz/z/b了
    yangyadong@ubuntu:~/yydTestDir$ mkdir /tmp/{zz,yy}/a/b -v -p
    mkdir: 已创建目录 '/tmp/zz'
    mkdir: 已创建目录 '/tmp/zz/a'
    mkdir: 已创建目录 '/tmp/zz/a/b'
    mkdir: 已创建目录 '/tmp/yy'
    mkdir: 已创建目录 '/tmp/yy/a'
    mkdir: 已创建目录 '/tmp/yy/a/b'
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 举例4：

  - ```
    想在/tmp路径下创建以下目录
    etc/init.d
    etc/sysconfig
    usr/lib
    usr/bin
    usr/include
    var/spool
    var/run
    proc
    sys
    bin
    lib
    media
    mnt
    只需要以下一条命令
    mkdir /tmp/{etc/{init.d,sysconfig},usr/{lib,bin,include},var/{spool,run},proc,sys,bin,lib,media,mnt}
    ```

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ mkdir ./{etc/{init.d,sysconfig},usr/{lib,bin,include},var/{spool,run},proc,sys,bin,lib,media,mnt} -v -p
    mkdir: 已创建目录 './etc'
    mkdir: 已创建目录 './etc/init.d'
    mkdir: 已创建目录 './etc/sysconfig'
    mkdir: 已创建目录 './usr'
    mkdir: 已创建目录 './usr/lib'
    mkdir: 已创建目录 './usr/bin'
    mkdir: 已创建目录 './usr/include'
    mkdir: 已创建目录 './var'
    mkdir: 已创建目录 './var/spool'
    mkdir: 已创建目录 './var/run'
    mkdir: 已创建目录 './proc'
    mkdir: 已创建目录 './sys'
    mkdir: 已创建目录 './bin'
    mkdir: 已创建目录 './lib'
    mkdir: 已创建目录 './media'
    mkdir: 已创建目录 './mnt'
    yangyadong@ubuntu:~/yydTestDir$ ls
    bin  etc  lib  media  mnt  proc  sys  usr  var
    ```

### 2.9重定向（> >>）

#### 2.9.0重定向可以做什么

- 可以将命令输出保存到文件
- 可以向配置文件增加内容
- 可以合并文件内容
- ....

#### 2.9.1重定向的概念，以及标准设备原理、进程号、文件描述符

![image.png](https://i.loli.net/2020/12/10/nHTgYF6w3cK9MyS.png)

- 重定向是指：将默认的标准输入（键盘）、标准输出（显示器）、标准错误输出（显示器）重定向

  - 比如说执行结果即标准输出不通过显示器输出了，我让它把结果输出在文件里
  - 标准输入也可以不来自键盘，比如来自记事本里的文字

- 举例：什么是标准输入、什么是标准输出、什么是标准错误输出

  - ```shell
    #我能顺利从键盘在shell中输入命令，这就是标准输入
    yangyadong@ubuntu:~/yydTestDir$ ls
    a.cpp
    #cat用于显示文本内容，所以底下顺利的显示了a.cpp的内容，这就是标准输出
    yangyadong@ubuntu:~/yydTestDir$ cat ./a.cpp
    #include<iostream>
    using namespace std;
    int main()
    {
            int a=1;
            int b=2;
            return 0;
    }
    #命令报错，这就是标准错误输出
    yangyadong@ubuntu:~/yydTestDir$ cat ./b.cpp
    cat: ./b.cpp: 没有那个文件或目录
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 每个进程都有个系统分配的进程号，比如1681号进程

  - 那么在/proc/1681/fd/这个目录下，就维护着该进程所使用的文件
  - ![image.png](https://i.loli.net/2020/12/10/3aTE1jcqm9yWkou.png)
  - 一般来说，每个进程至少占了3个文件。0、1、2
  - 0是标准输入设备（设备也是一个文件），1是标准输出设备，2是标准错误输出设备

- 举例：查看进程所维护的文件操作符。一般来说 对于每个进程，0 1 2这三个文件描述符必有，因为分别是操作系统指定的标准输入文件，标准输出文件，标准错误输出文件

  - ```shell
    #tail命令，显示文件的末尾几行。
    #加上-f命令是为了让tail进程一直执行
    yangyadong@ubuntu:~/yydTestDir$ tail -f /etc/passwd
    saned:x:114:119::/var/lib/saned:/usr/sbin/nologin
    avahi:x:115:120:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
    colord:x:116:121:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
    hplip:x:117:7:HPLIP system user,,,:/var/run/hplip:/bin/false
    geoclue:x:118:122::/var/lib/geoclue:/usr/sbin/nologin
    pulse:x:119:123:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
    gnome-initial-setup:x:120:65534::/run/gnome-initial-setup/:/bin/false
    gdm:x:121:125:Gnome Display Manager:/var/lib/gdm3:/bin/false
    yangyadong:x:1000:1000:myUbuntu,,,:/home/yangyadong:/bin/bash
    sshd:x:122:65534::/run/sshd:/usr/sbin/nologin
    ^Z #按了一下ctrl+z，让这个进程在后台进行
    [1]+  已停止               tail -f /etc/passwd
    #ps显示当前进程
    yangyadong@ubuntu:~/yydTestDir$ ps
       PID TTY          TIME CMD
     89822 pts/0    00:00:00 bash
     92086 pts/0    00:00:00 tail
     92093 pts/0    00:00:00 ps
    #查看tail进程所使用的文件
    yangyadong@ubuntu:~/yydTestDir$ ls -l /proc/92086/fd
    总用量 0
    #0 1 2 3 4叫做文件描述符fid
    lrwx------ 1 yangyadong yangyadong 64 12月  9 08:55 0 -> /dev/pts/0 #标准输入设备（文件）
    lrwx------ 1 yangyadong yangyadong 64 12月  9 08:55 1 -> /dev/pts/0 #标准输出设备（文件）
    lrwx------ 1 yangyadong yangyadong 64 12月  9 08:55 2 -> /dev/pts/0 #标准错误输出设备（文件）
    lr-x------ 1 yangyadong yangyadong 64 12月  9 08:55 3 -> /etc/passwd#该进程操作的磁盘文件
    lr-x------ 1 yangyadong yangyadong 64 12月  9 08:55 4 -> anon_inode:inotify
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 所以你想重定向标准输出设备。那就把fid=1的这个文件描述符指向的 -> /dev/pts/0 #标准输出设备（文件），修改为你希望的输出文件，比如修改成abc文件。那么该进程的标准输出就输出到abc文件了，而不再是默认终端设备/dev/pts/0（其实就是显示器）

  - ![image.png](https://i.loli.net/2020/12/10/SD6qzpd397vMNga.png)

  - ![image.png](https://i.loli.net/2020/12/10/y7DUntFL2xumBGZ.png)

  - 重定向的实质就是修改了这个的指向。

  - **底下的重定向命令例如 `echo "123456" 1>/tmp/test.md`实质上就是修改了把1->dev/pts/2（这里默认指向的是虚拟终端）修改为了1->/tmp/test.md而已**

- ```shell
  #查看系统默认的标准输入、标准输出、标准错误输出设备
  #-> /proc/self/fd/2，所以只要创建一个进程，就会有这个3个文件，拷贝过去到对应进程文件夹下
  yangyadong@ubuntu:~/yydTestDir$ ls -l /dev/std*
  lrwxrwxrwx 1 root root 15 12月  1 19:54 /dev/stderr -> /proc/self/fd/2
  lrwxrwxrwx 1 root root 15 12月  1 19:54 /dev/stdin -> /proc/self/fd/0
  lrwxrwxrwx 1 root root 15 12月  1 19:54 /dev/stdout -> /proc/self/fd/1
  ```

#### 2.9.2重定向如何修改

- ![image.png](https://i.loli.net/2020/12/10/jM8uZ1gW43xrGR9.png)

- 举例：修改重定向标准输出，演示一下操作符>

  - ```shell
    #没有修改重定向，那直接输入这个命令，就默认输出到显示器上了
    yangyadong@ubuntu:~/yydTestDir$ ifconfig ens33
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.80.131  netmask 255.255.255.0  broadcast 192.168.80.255
            inet6 fe80::8635:4e7:fbe3:97c7  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:ab:1a:9e  txqueuelen 1000  (以太网)
            RX packets 742640  bytes 836760341 (836.7 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 195496  bytes 37623678 (37.6 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    #使用了修改重定向，输入这个命令 再加上1>yydTest.md。
    #1是因为ifconfig这个进程的标准输出文件的文件描述符是1。
    #操作符>是将命令的执行结果输出到指定文件中，而不是直接显示在屏幕上
    yangyadong@ubuntu:~/yydTestDir$ ifconfig ens33 1>yydTest.md
    #显示yydTest.md的内容
    yangyadong@ubuntu:~/yydTestDir$ cat yydTest.md
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.80.131  netmask 255.255.255.0  broadcast 192.168.80.255
            inet6 fe80::8635:4e7:fbe3:97c7  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:ab:1a:9e  txqueuelen 1000  (以太网)
            RX packets 742686  bytes 836763739 (836.7 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 195521  bytes 37626552 (37.6 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 演示一下操作符>>

    ```shell
    #尝试查看eth1这个网卡信息，我没有这个网卡，会产生标准错误输出，默认到显示器
    yangyadong@ubuntu:~/yydTestDir$ ifconfig eth1
    eth1: 获取接口信息时发生错误: Device not found
    #将默认错误输出重定向到yydTest.md文件。
    #2是因为，ifconfig这个进程的标准错误输出文件的文件描述符是2。
    #操作符>>是将命令的执行结果追加到指定文件中
    yangyadong@ubuntu:~/yydTestDir$ ifconfig eth1 2>>yydTest.md
    yangyadong@ubuntu:~/yydTestDir$ cat yydTest.md
    ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.80.131  netmask 255.255.255.0  broadcast 192.168.80.255
            inet6 fe80::8635:4e7:fbe3:97c7  prefixlen 64  scopeid 0x20<link>
            ether 00:0c:29:ab:1a:9e  txqueuelen 1000  (以太网)
            RX packets 742686  bytes 836763739 (836.7 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 195521  bytes 37626552 (37.6 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    eth1: 获取接口信息时发生错误: Device not found
    #可以看到是追加的形式写入文件的
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 因为一般都是重定向标准输出设备，而操作系统给进程的对应文件操作符，也一般都是1。所以文件描述符1可以省略不写

    - ```shell
      yangyadong@ubuntu:~/yydTestDir$ hostname
      ubuntu
      #hostname >hn.md  等价于 hostname 1>hn.md  。文件描述符1可以省略不写
      yangyadong@ubuntu:~/yydTestDir$ hostname >hn.md
      yangyadong@ubuntu:~/yydTestDir$ cat hn.md
      ubuntu
      yangyadong@ubuntu:~/yydTestDir$
      ```

  - 举例：将标准输出和标准错误输出分别重定向到不同文件

    - ```shell
      #输出里，有的是标准输出，有的是标准错误输出
      yangyadong@ubuntu:~/yydTestDir$ find /etc -name "*.conf"
      /etc/systemd/timesyncd.conf
      find: ‘/etc/ssl/private’: 权限不够
      /etc/depmod.d/ubuntu.conf
      find: ‘/etc/vmware-tools/GuestProxyData/trusted’: 权限不够
      /etc/xdg/user-dirs.conf
      find: ‘/etc/cups/ssl’: 权限不够
      find: ‘/etc/polkit-1/localauthority’: 权限不够
      /etc/polkit-1/localauthority.conf.d/50-localauthority.conf
      #将标准输出重定向到a文件，标准错误输出重定向到b文件
      yangyadong@ubuntu:~/yydTestDir$ find /etc -name "*.conf" 1>a 2>b
      #查看a文件，果真都是标准输出
      yangyadong@ubuntu:~/yydTestDir$ cat a
      /etc/systemd/timesyncd.conf
      /etc/depmod.d/ubuntu.conf
      /etc/xdg/user-dirs.conf
      /etc/polkit-1/localauthority.conf.d/50-localauthority.conf
      #查看b文件，果真都是标准错误输出
      yangyadong@ubuntu:~/yydTestDir$ cat b
      find: ‘/etc/ssl/private’: 权限不够
      find: ‘/etc/vmware-tools/GuestProxyData/trusted’: 权限不够
      find: ‘/etc/cups/ssl’: 权限不够
      find: ‘/etc/polkit-1/localauthority’: 权限不够
      yangyadong@ubuntu:~/yydTestDir$
      ```

    - 把标准输出和标准错误输出重定向到同一文件

    - ```shell
      #也可以把标准输出，标准错误输出都重定向到同一个文件。
      #使用&符号统指文件操作符1和2
      yangyadong@ubuntu:~/yydTestDir$ find /etc -name "*.conf" &>c
      yangyadong@ubuntu:~/yydTestDir$ cat c
      /etc/systemd/timesyncd.conf
      find: ‘/etc/ssl/private’: 权限不够
      /etc/depmod.d/ubuntu.conf
      find: ‘/etc/vmware-tools/GuestProxyData/trusted’: 权限不够
      /etc/xdg/user-dirs.conf
      find: ‘/etc/cups/ssl’: 权限不够
      find: ‘/etc/polkit-1/localauthority’: 权限不够
      /etc/polkit-1/localauthority.conf.d/50-localauthority.conf
      yangyadong@ubuntu:~/yydTestDir$
      ```

  - 再举几个常用的例子

    - 可以看到，下边的这几个例子，都直接省略了文件描述符1。1代表的是echo这个进程的1号文件描述符，默认的1号文件描述符就是标准输出设备即显示器，所以可是省略。但是2是标准错误输出设备（尽管它也是显示器，但是是不同文件），不能省略。
    - echo "123456">test 与 echo "123456" 1>test是等价的。

    ```shell
    yangyadong@ubuntu:~/yydTestDir$ echo "123456">test
    yangyadong@ubuntu:~/yydTestDir$ cat test
    123456
    yangyadong@ubuntu:~/yydTestDir$ echo "111111">test
    yangyadong@ubuntu:~/yydTestDir$ cat test
    111111
    yangyadong@ubuntu:~/yydTestDir$ echo "222222">>test
    yangyadong@ubuntu:~/yydTestDir$ cat test
    111111
    222222
    yangyadong@ubuntu:~/yydTestDir$ date >>test
    yangyadong@ubuntu:~/yydTestDir$ cat test
    111111
    222222
    2020年 12月 09日 星期三 09:44:21 PST
    yangyadong@ubuntu:~/yydTestDir$ echo "33333" 1>>test
    yangyadong@ubuntu:~/yydTestDir$ cat test
    111111
    222222
    2020年 12月 09日 星期三 09:44:21 PST
    33333
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - 利用输出重定向进行文件内容的合并

    - ```shell
      yangyadong@ubuntu:~/yydTestDir$ cat a
      11111
      yangyadong@ubuntu:~/yydTestDir$ cat b
      22222
      yangyadong@ubuntu:~/yydTestDir$ cat a b 1>c  #或者是cat a b >c
      yangyadong@ubuntu:~/yydTestDir$ cat c
      11111
      22222
      yangyadong@ubuntu:~/yydTestDir$
      ```

  - 重定向的实质，参看上文图片的这部分

    - **重定向命令例如 `echo "123456" 1>/tmp/test.md`实质上就是修改了把1->dev/pts/0（这里指向的设备是虚拟终端）修改为了1->/tmp/test.md而已**

### 2.10管道技术（"|"、grep、xargs）

#### 2.10.1概要

上一个命令的输出作为下一个命令的输入，可以一直这样写下去，类似于管道

![image.png](https://i.loli.net/2020/12/10/dKBFiljhHsmXGVL.png)

- 管道操作符号"|"

  - **连接左右两个命令，将左侧的命令的标准输出，作为右侧命令的标准输入**
  - 格式：cmd1 | cmd2 [...| cmdn]
    - `head -5 /etc/passwd |tail -1`

- 用grep 过滤输出

  - `ls -l /etc |grep pass`

- 管道和标准错误

  - ```shell
    find /etc -name "p*"|grep passwd
    find /etc -name "p*"|grep passwd > /tmp/aa
    find /etc -name "p*"|grep passwd 2> /tmp/aa
    find /etc -name "p*"|grep passwd &> /tmp/aa
    ```



#### 2.10.2举例：

- 列出文件的前五行，的倒数后两行。即该文件的第四第五行

  - ```shell
    #先看看文件的前五行
    yangyadong@ubuntu:~$ head -5 /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    #前五行里的后两行
    yangyadong@ubuntu:~$ head -5 /etc/passwd | tail -2
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    yangyadong@ubuntu:~$
    #文件的前五行里，的后两行，的包含dev的行。使用grep过滤
    yangyadong@ubuntu:~$ head -5 /etc/passwd | tail -2 | grep dev
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    yangyadong@ubuntu:~$
    ```

#### 2.10.3要点：

- 1**.在管道后边的命令（即第一个|之后的所有命令），都不应该再跟文件名了**

  - ![image.png](https://i.loli.net/2020/12/10/pKArqP83zbagMVm.png)
  - 可以剖析一下
    - `head -5 /etc/passwd | tail -2 /etc/passwd`。管道后边的命令tail又跟了文件名。它的输出与只写`tail -2 /etc/passwd`一模一样。即管道没有作用
    - `head -5 /etc/passwd |tail -2`管道后边的命令tail没有再跟文件名。那它的输出是对的，输出的就是/etc/passwd这个文件的前五行，的后两行。

- 2.**在管道中，只有标准输出才传递给下一个命令。那些标准错误输出会直接输出到终端。**

  - 即**标准错误输出不往下传递，直接出管道，输出到终端**

  - 举例：

    - ```shell
      #查找/etc下，所有名字里有.conf的文件，在用grep过滤，必须也得是包含nginx的文件
      #理论上，现在所有的输出都应该有nginx，但是发现某些 文件权限不够也输出了，它怎么没有nginx也可以输出？
      #原因就是，标准错误输出不往下传递，直接出管道，输出到终端
      yangyadong@ubuntu:~$ find /etc -name "*.conf" | grep nginx
      find: ‘/etc/ssl/private’: 权限不够
      find: ‘/etc/vmware-tools/GuestProxyData/trusted’: 权限不够
      find: ‘/etc/cups/ssl’: 权限不够
      find: ‘/etc/polkit-1/localauthority’: 权限不够
      /etc/nginx/modules-enabled/50-mod-stream.conf
      /etc/nginx/modules-enabled/50-mod-mail.conf
      /etc/nginx/modules-enabled/50-mod-http-image-filter.conf
      /etc/nginx/modules-enabled/50-mod-http-geoip.conf
      /etc/nginx/modules-enabled/50-mod-http-xslt-filter.conf
      /etc/nginx/conf.d/wordpress.conf
      /etc/nginx/fastcgi.conf
      /etc/nginx/nginx.conf
      /etc/nginx/snippets/fastcgi-php.conf
      /etc/nginx/snippets/snakeoil.conf
      yangyadong@ubuntu:~$
      ```

    - 刚才那个标准错误输出直接出管道，输出到显示器了，让它别输出出来了，太难看，那可以让标准错误输出重定向到空文件

      - ```shell
        #首先呢标准错误输出不会沿管道往下传输，会直接出管道进行输出。所以为了不让它输出到显示器，给它重定向到Null文件。这样就清净多了。
        #只显示标准输出中包含nginx的
        yangyadong@ubuntu:~$ find /etc -name "*.conf" 2>/dev/null | grep nginx
        /etc/nginx/modules-enabled/50-mod-stream.conf
        /etc/nginx/modules-enabled/50-mod-mail.conf
        /etc/nginx/modules-enabled/50-mod-http-image-filter.conf
        /etc/nginx/modules-enabled/50-mod-http-geoip.conf
        /etc/nginx/modules-enabled/50-mod-http-xslt-filter.conf
        /etc/nginx/conf.d/wordpress.conf
        /etc/nginx/fastcgi.conf
        /etc/nginx/nginx.conf
        /etc/nginx/snippets/fastcgi-php.conf
        /etc/nginx/snippets/snakeoil.conf
        yangyadong@ubuntu:~$
        ```

- 3.**有些命令不支持管道技术**

  - 比如ls命令就不支持。它不支持把别人的标准输出，作为它的标准输入。

  - ```shell
    #使用一下which命令
    yangyadong@ubuntu:~$ which cat
    /bin/cat
    #使用一个ls命令
    yangyadong@ubuntu:~$ ls -l /bin/cat
    -rwxr-xr-x 1 root root 35064 1月  18  2018 /bin/cat
    #我幻想着，把which cat的输出“/bin/cat“当做命令ls -l的输入，那可以写成管道技术
    #which cat的输出  | 当做ls -l的输入
    yangyadong@ubuntu:~$ which cat | ls -l
    总用量 52
    drwxr-xr-x 3 yangyadong yangyadong 4096 12月  3 04:20 Desktop
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Documents
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Downloads
    -rw-r--r-- 1 yangyadong yangyadong 8980 12月  1 11:06 examples.desktop
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Music
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Pictures
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Public
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Templates
    drwxr-xr-x 2 yangyadong yangyadong 4096 12月  1 11:08 Videos
    drwxrwxr-x 5 yangyadong yangyadong 4096 12月  4 08:06 yydDir
    drwxrwxr-x 2 yangyadong yangyadong 4096 12月  9 09:51 yydTestDir
    #发现执行结果不对劲，直接执行了ls -l
    #原因在于，ls这个命令不支持管道技术。
    #所以which cat这个标准输出"/bin/cat"不能成为 ls -l的标准输入
    ```

  - 解决方案：

    - 使用xargs

    - 用途：将参数列表转换成小块分段传递给其他命令

    - 读入stdin的数据转换为参数添加到命令行

    - 所以，就可以让一些不支持管道的命令比如ls可以使用管道

    - ```shell
      #加上xargs，就符合预期了。让ls命令可以使用管道技术
      yangyadong@ubuntu:~$ which cat | xargs ls -l
      -rwxr-xr-x 1 root root 35064 1月  18  2018 /bin/cat
      yangyadong@ubuntu:~$
      ```

## 3.文件管理类命令

### 3.1命令选项和参数

命令 [选项] [参数]

command [options] [arguments]

**重点注意区分：**

- 命令可以有命令选项和命令参数
  - 命令选项，即 -h或者--help等
  - 命令参数，可以理解为命令的作用对象。比如 `cat /etc/passwd`里的`/etc/passwd`
- 而命令选项，又可以有自己的选项参数
  - 比如`tail -n 2 /etc/passwd`里的 2，就是命令选项-n的参数
- 举例：
  -  `tail -n 2 /etc/passwd`
    - **tail是命令，-n是命令短选项，2是命令短选项-n的参数，/etc/passwd是tail命令的参数（或者说叫做tail命令的作用对象）**。 意思是把文件/etc/passwd的倒数2行进行输出
- 即参数：分为命令的参数和选项的参数。

#### 选项与参数

- 短选项 -h -l -a（短选项是一个-，并且后边接的是字母）
  - 短选项可以组合 -hla
  - 有些命令的短选项可以不带 "-"，通常称作BSD风格的选项
    - 比如 `ps aux` 
      - ps是命令，aux是选项，aux就可以不加横杠-
    - 再比如 `tar xf`
      - tar是命令，xf是选项，xf也可以不加横杠
  - 有些选项需要带参数
    - 比如 `tail -n 2 /etc/passwd`
      - **tail是命令，-n是选项，2是选项-n的参数，/etc/passwd是tail命令的参数（或者说叫做tail命令的作用对象）**。 意思是把文件/etc/passwd的倒数2行进行输出
- 长选项 --help    --list  （长选项是两个-，并且后边接的是一个长单词）
  - 长选项不能组合
  - 如果需要参数的话，长选项的参数通常需要=号。
    - 比如  --size=1
- 一般来说长选项都有个对应的短选项，比如--all与-a就是一回事

#### 命令与参数

选项可以加参数，命令其实也可以加参数。只不过命令的参数一般就是命令的作用对象

- 命令后的参数，就是命令的作用对象
  - 比如 `ls /root`
    - ls是命令，/root可以说是该命令的参数，也可以说是该命令的作用对象
  - 再比如 cat /etc/passwd
    - cat是命令，/etc/passwd可以说是该命令的参数，也可以说是该命令的作用对象

#### 命令帮助

- 内部命令
  - help command 
    - 比如: `help cd`
- 外部命令
  - command --help
    - 比如:`cat --help`
- 其实，一般都用 command --help就行，不用区分内部还是外部命令

### 3.2cd命令切换目录

cd: change directory，更换

- 直接一个`cd`或者`cd ~`：进入当前用户主目录

  - ```shell
    yangyadong@ubuntu:/tmp$ pwd
    /tmp
    yangyadong@ubuntu:/tmp$ cd
    yangyadong@ubuntu:~$
    yangyadong@ubuntu:~$ pwd
    /home/yangyadong
    yangyadong@ubuntu:~$
    ```

- 进入某个用户的主目录 `cd ~用户名`

  - ```shell
    root@ubuntu:~# pwd
    /root
    root@ubuntu:~# cd ~yangyadong
    root@ubuntu:/home/yangyadong# pwd
    /home/yangyadong
    root@ubuntu:/home/yangyadong#
    ```

- 返回到上一次的目录`cd -`，类似于windows里的返回

  - ```shell
    #原理是，$OLDPWD里存着上一次cd进去的目录
    yangyadong@ubuntu:/tmp$ echo $OLDPWD
    /home/yangyadong/yydDir
    yangyadong@ubuntu:/tmp$ cd -
    /home/yangyadong/yydDir
    yangyadong@ubuntu:~/yydDir$ pwd
    /home/yangyadong/yydDir
    yangyadong@ubuntu:~/yydDir$
    ```

- cd命令的参数（或者叫cd命令的作用对象）可以写绝对路径或者相对路径

  - 绝对路径：以根目录开始的路径

  - 相对路径：以当前目录为参照写的路径

    - .. 代表上级目录

    - . 代表当前目录

    - ```shell
      yangyadong@ubuntu:~/yydTestDir$ cd testDir
      yangyadong@ubuntu:~/yydTestDir/testDir$
      与
      yangyadong@ubuntu:~/yydTestDir$ cd ./testDir
      yangyadong@ubuntu:~/yydTestDir/testDir$
      一样
      ```

### 3.3ls命令介绍

ls: list的简写，列出文件夹中的内容、文件、文件夹

```shell
yangyadong@ubuntu:~/yydTestDir/testDir$ ls --help
用法：ls [选项]... [文件]...
列出FILE的信息（默认为当前目录）List information about the FILEs (the current directory by default).
如果不指定-cftuvSUX 或 --sort选项，则根据字母大小排序。Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
长选项必须使用的参数对于短选项时也是必需使用的。
  -a, --all                     不隐藏任何以. 开始的项目
  -A, --almost-all              列出除. 及.. 以外的任何项目
      --author                  与-l 同时使用时列出每个文件的作者
  -b, --escape                  以八进制溢出序列表示不可打印的字符
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                            类似-l，但不列出所有者
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l and/or -s, print human readable sizes
                               (e.g., 1K 234M 2G)
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --hyperlink[=WHEN]     hyperlink file names; WHEN can be 'always'
                               (default if omitted), 'auto', or 'never'
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                            使用较长格式列出信息
  -L, --dereference             当显示符号链接的文件信息时，显示符号链接所指示
                                的对象而并非符号链接本身的信息
  -m                            所有项目以逗号分隔，并填满整行行宽
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print entry names without quoting
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always,
                               shell-escape, shell-escape-always, c, escape
  -r, --reverse                 逆序排列
  -R, --recursive               递归显示子目录
  -s, --size                    以块数形式显示每个文件分配的尺寸
  -S                         sort by file size, largest first
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u);
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time (newest first)
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time, newest first
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           set output width to COLS.  0 means no limit
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -Z, --context              print any security context of each file
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

使用色彩来区分文件类型的功能已被禁用，默认设置和 --color=never 同时禁用了它。
使用 --color=auto 选项，ls 只在标准输出被连至终端时才生成颜色代码。
LS_COLORS 环境变量可改变此设置，可使用 dircolors 命令来设置。

退出状态：
 0  正常
 1  一般问题 (例如：无法访问子文件夹)
 2  严重问题 (例如：无法使用命令行参数)
yangyadong@ubuntu:~/yydTestDir/testDir$
```

**ls命令里比较常用的选项：**

- -a  --all：隐藏文件也显示出来，即.开头的文件

- -l          ：使用较长格式，列出文件的详细信息

- -R  --recursive：递归显示子目录

- -r   --reverse：排序时逆序

- -d：只显示目录本身一些属性，而不显示其目录下的内容

- 我习惯用的

  - ```shell
    ls -als | cat -n #详细列出所有文件和隐藏文件，并编号
    ls -ls | cat -n#详细列出所有文件，不显示隐藏文件，并编号
    ls | cat -n#列出文件，不显示隐藏文件，并编号
    ```

    

举例：

- 列出当前文件夹中，std开头的文件和文件夹

  - ```shell
    #深蓝色是文件夹，浅蓝色是链接文件，黄色是普通文件
    yangyadong@DESKTOP-9LD4MQ0:/dev$ ls
    block   tty2     ttyS110  ttyS124  ttyS138  ttyS151  ttyS165  ttyS179  ttyS2   ttyS33  ttyS47  ttyS60  ttyS74  ttyS88
    fd      ttyS0    ttyS111  ttyS125  ttyS139  ttyS152  ttyS166  ttyS18   ttyS20  ttyS34  ttyS48  ttyS61  ttyS75  ttyS89
    kmsg    ttyS1    ttyS112  ttyS126  ttyS14   ttyS153  ttyS167  ttyS180  ttyS21  ttyS35  ttyS49  ttyS62  ttyS76  ttyS9
    lxss    ttyS10   ttyS113  ttyS127  ttyS140  ttyS154  ttyS168  ttyS181  ttyS22  ttyS36  ttyS5   ttyS63  ttyS77  ttyS90
    null    ttyS100  ttyS114  ttyS128  ttyS141  ttyS155  ttyS169  ttyS182  ttyS23  ttyS37  ttyS50  ttyS64  ttyS78  ttyS91
    ptmx    ttyS101  ttyS115  ttyS129  ttyS142  ttyS156  ttyS17   ttyS183  ttyS24  ttyS38  ttyS51  ttyS65  ttyS79  ttyS92
    pts     ttyS102  ttyS116  ttyS13   ttyS143  ttyS157  ttyS170  ttyS184  ttyS25  ttyS39  ttyS52  ttyS66  ttyS8   ttyS93
    random  ttyS103  ttyS117  ttyS130  ttyS144  ttyS158  ttyS171  ttyS185  ttyS26  ttyS4   ttyS53  ttyS67  ttyS80  ttyS94
    shm     ttyS104  ttyS118  ttyS131  ttyS145  ttyS159  ttyS172  ttyS186  ttyS27  ttyS40  ttyS54  ttyS68  ttyS81  ttyS95
    stderr  ttyS105  ttyS119  ttyS132  ttyS146  ttyS16   ttyS173  ttyS187  ttyS28  ttyS41  ttyS55  ttyS69  ttyS82  ttyS96
    stdin   ttyS106  ttyS12   ttyS133  ttyS147  ttyS160  ttyS174  ttyS188  ttyS29  ttyS42  ttyS56  ttyS7   ttyS83  ttyS97
    stdout  ttyS107  ttyS120  ttyS134  ttyS148  ttyS161  ttyS175  ttyS189  ttyS3   ttyS43  ttyS57  ttyS70  ttyS84  ttyS98
    tty     ttyS108  ttyS121  ttyS135  ttyS149  ttyS162  ttyS176  ttyS19   ttyS30  ttyS44  ttyS58  ttyS71  ttyS85  ttyS99
    tty0    ttyS109  ttyS122  ttyS136  ttyS15   ttyS163  ttyS177  ttyS190  ttyS31  ttyS45  ttyS59  ttyS72  ttyS86  urandom
    tty1    ttyS11   ttyS123  ttyS137  ttyS150  ttyS164  ttyS178  ttyS191  ttyS32  ttyS46  ttyS6   ttyS73  ttyS87  zero
    #列出当前文件夹中，std开头的文件和文件夹
    yangyadong@DESKTOP-9LD4MQ0:/dev$ ls std*
    stderr  stdin  stdout
    yangyadong@DESKTOP-9LD4MQ0:/dev$ 
    #使用ls -l选项，列出详细信息
    yangyadong@DESKTOP-9LD4MQ0:/dev$ ls -l std*
    lrwxrwxrwx 1 root root 15 Dec 12 00:44 stderr -> /proc/self/fd/2
    lrwxrwxrwx 1 root root 15 Dec 12 00:44 stdin -> /proc/self/fd/0
    lrwxrwxrwx 1 root root 15 Dec 12 00:44 stdout -> /proc/self/fd/1
    yangyadong@DESKTOP-9LD4MQ0:/dev$
    #使用 ls列出其他目录中的内容
    yangyadong@DESKTOP-9LD4MQ0:/dev$ ls /home
    yangyadong
    yangyadong@DESKTOP-9LD4MQ0:/dev$
    #递归显示某个文件夹下内容
    yangyadong@DESKTOP-9LD4MQ0:~$ ls -R /home
    /home:
    yangyadong
    
    /home/yangyadong:
    dog  test.sh
    
    /home/yangyadong/dog:
    payroll  yyd  yyd.cpp
    yangyadong@DESKTOP-9LD4MQ0:~$
    #仅仅显示某个文件夹的信息，不显示其里边内容的信息
    yangyadong@DESKTOP-9LD4MQ0:~$ ls -dl /dev
    drwxr-xr-x 1 root root 4096 Dec 12 00:50 /dev
    yangyadong@DESKTOP-9LD4MQ0:~$
    ```

- ```shell
  #输出文件详细信息，并编号
  yangyadong@DESKTOP-9LD4MQ0:~$ ls -als | cat -n
       1  total 24
       2   0 drwxr-xr-x 1 yangyadong yangyadong 4096 Dec 12 00:44 .
       3   0 drwxr-xr-x 1 root       root       4096 Jul 25  2019 ..
       4   8 -rw------- 1 yangyadong yangyadong 6442 Dec 12 00:50 .bash_history
       5   0 -rw-r--r-- 1 yangyadong yangyadong  220 Jul 25  2019 .bash_logout
       6   4 -rw-r--r-- 1 yangyadong yangyadong 3812 Jul 26  2019 .bashrc
       7   0 drwxrwxrwx 1 yangyadong yangyadong 4096 Apr 25  2020 .cache
       8   0 drwxrwxrwx 1 yangyadong yangyadong 4096 Apr 25  2020 .config
       9   0 drwxrwxrwx 1 yangyadong yangyadong 4096 Apr 25  2020 .mono
      10   0 -rw-r--r-- 1 yangyadong yangyadong  807 Jul 25  2019 .profile
      11   0 -rw------- 1 yangyadong yangyadong   12 Aug  2  2019 .python_history
      12   0 -rw-rw-rw- 1 yangyadong yangyadong   75 Sep 26  2019 .selected_editor
      13   0 drwx------ 1 yangyadong yangyadong 4096 Sep 25  2019 .ssh
      14   0 -rw-r--r-- 1 yangyadong yangyadong    0 Jul 25  2019 .sudo_as_admin_successful
      15  12 -rw------- 1 root       root       8499 Apr 25  2020 .viminfo
      16   0 drwxrwxrwx 1 yangyadong yangyadong 4096 Apr 25  2020 .vscode-server
      17   0 -rw-rw-rw- 1 yangyadong yangyadong  183 Apr 25  2020 .wget-hsts
      18   0 drwxrwxrwx 1 yangyadong yangyadong 4096 Jun 24 14:24 dog
      19   0 -rwxrwxrwx 1 yangyadong yangyadong   32 Sep 26  2019 test.sh
  yangyadong@DESKTOP-9LD4MQ0:~$ ls -ls | cat -n
      1  total 0
      2  0 drwxrwxrwx 1 yangyadong yangyadong 4096 Jun 24 14:24 dog
      3  0 -rwxrwxrwx 1 yangyadong yangyadong   32 Sep 26  2019 test.sh
  ```


ls -l详解

- ![image.png](https://i.loli.net/2020/12/18/PwKDCdXYqJj9yxZ.png)
- 



### 3.4mkdir和rmdir

#### mkdir

```shell
#看看mkdir是内部还是外部，结果是外部命令
yangyadong@DESKTOP-9LD4MQ0:~$ type mkdir
mkdir is /bin/mkdir
yangyadong@DESKTOP-9LD4MQ0:~$ mkdir --help
Usage: mkdir [OPTION]... DIRECTORY...
用法：mkdir [选项]...目录....
Create the DIRECTORY(ies), if they do not already exist.
若指定目录不存在则创建目录。
Mandatory arguments to long options are mandatory for short options too.
长选项必须是用的参数对于短选项时也是必需使用的。
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask 
  					设置权限模式（类似chmod），而不是rwx-umask
  -p, --parents     no error if existing, make parent directories as needed
  					如果目录已经存在也不会报错。需要时创建目标目录的上层目录
  -v, --verbose     print a message for each created directory
  					每次创建新目录都显示信息。
  -Z                   set SELinux security context of each created directory
                         to the default type
                       将每个创建的目录的SELinux安全环境设置为CTX
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
                         跟-Z一样
      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Report mkdir translation bugs to <http://translationproject.org/team/>
Full documentation at: <http://www.gnu.org/software/coreutils/mkdir>
or available locally via: info '(coreutils) mkdir invocation'
yangyadong@DESKTOP-9LD4MQ0:~$
```

```shell
#直接创建一个目录。test是一个文件夹而不是文件
yangyadong@DESKTOP-9LD4MQ0:~$ mkdir /tmp/test
#创建一个多层目录。报错了，因为m100都还不存在，就创建它的子目录n100了
yangyadong@DESKTOP-9LD4MQ0:~$ mkdir /tmp/m100/n100
mkdir: cannot create directory ‘/tmp/m100/n100’: No such file or directory
#-p，需要时创建目标目录的上层目录。-v详细展示创建过程
yangyadong@DESKTOP-9LD4MQ0:~$ mkdir /tmp/m100/n100 -pv
mkdir: created directory '/tmp/m100'
mkdir: created directory '/tmp/m100/n100'
yangyadong@DESKTOP-9LD4MQ0:~$
```

#### rmdir（只能删除空文件夹）

只能删除空文件夹。

直接删除一个文件夹无所谓，但是**想用-p递归删除时，切记切换目录，使用相对路径-p删除**。切记不要使用绝对路径-p来递归删除

```shell
yangyadong@DESKTOP-9LD4MQ0:~$ type rmdir
rmdir is hashed (/bin/rmdir)
yangyadong@DESKTOP-9LD4MQ0:~$ rmdir --help
Usage: rmdir [OPTION]... DIRECTORY...
Remove the DIRECTORY(ies), if they are empty.
删除指定的空目录
      --ignore-fail-on-non-empty
                  ignore each failure that is solely because a directory
                    is non-empty
                    忽略仅由目录非空产生的所有错误
  -p, --parents   remove DIRECTORY and its ancestors; e.g., 'rmdir -p a/b/c' is
                    similar to 'rmdir a/b/c a/b a'
                    删除指定目录及其上级文件夹。例如'rmdir -p a/b/c'与'rmdir a/b/c a/b a'是一样的
  -v, --verbose   output a diagnostic for every directory processed
      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Report rmdir translation bugs to <http://translationproject.org/team/>
Full documentation at: <http://www.gnu.org/software/coreutils/rmdir>
or available locally via: info '(coreutils) rmdir invocation'
yangyadong@DESKTOP-9LD4MQ0:~$
```

```shell
#根目录下创建一个abc文件夹
yangyadong@DESKTOP-9LD4MQ0:~$ sudo mkdir /abc
#查看根目录下内容，abc文件夹已经创建了
yangyadong@DESKTOP-9LD4MQ0:~$ ls /
abc  bin  boot  dev  etc  home  init  lib  lib64  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var
yangyadong@DESKTOP-9LD4MQ0:~$
#删除/abc目录
yangyadong@DESKTOP-9LD4MQ0:~$ sudo rmdir /abc
#再查看根目录下内容，abc文件夹已经删除了
yangyadong@DESKTOP-9LD4MQ0:~$ ls /
bin  boot  dev  etc  home  init  lib  lib64  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var
yangyadong@DESKTOP-9LD4MQ0:~$
#创建一个多层文件夹
yangyadong@DESKTOP-9LD4MQ0:~$ mkdir /abc/a1/a2 -pv
mkdir: cannot create directory ‘/abc’: Permission denied
yangyadong@DESKTOP-9LD4MQ0:~$ sudo mkdir /abc/a1/a2 -pv
mkdir: created directory '/abc'
mkdir: created directory '/abc/a1'
mkdir: created directory '/abc/a1/a2'
yangyadong@DESKTOP-9LD4MQ0:~$
#注意！千万不能写rmdir /abc/a1/a2 -pv。这样会试图删除根目录/。因为abc是在根目录/下的
#所以想递归删除，应该切换过去使用相对路径进行删除
yangyadong@DESKTOP-9LD4MQ0:~$ cd /
yangyadong@DESKTOP-9LD4MQ0:/$
#注意不要写rmdir /abc/a1/a2 -pv。这样会试图删除根目录/
#注意，切换目录后使用相对路径也不要写./abc/a1/a2 -pv，这样还会试图删除'.'目录
yangyadong@DESKTOP-9LD4MQ0:/$ sudo rmdir abc/a1/a2/ -pv
rmdir: removing directory, 'abc/a1/a2/'
rmdir: removing directory, 'abc/a1'
rmdir: removing directory, 'abc'
```

#### tree查看文件结构树

```shell
yangyadong@DESKTOP-9LD4MQ0:~$ sudo mkdir /abc/{a1/a2,b1,c1} -pv
mkdir: created directory '/abc'
mkdir: created directory '/abc/a1'
mkdir: created directory '/abc/a1/a2'
mkdir: created directory '/abc/b1'
mkdir: created directory '/abc/c1'
yangyadong@DESKTOP-9LD4MQ0:~$ tree /abc
/abc
├── a1
│   └── a2
├── b1
└── c1

4 directories, 0 files
yangyadong@DESKTOP-9LD4MQ0:~$
```

### 3.5linux文件管理-文件类型介绍

- 根据ls -l的第一个字母来判断

- 第一个字母表示文件类型。接下来三个表示文件拥有者的权限，中间三个表示文件所属组拥有的权限，最后三个表示其他用户拥有的权限。

  - 普通文件，标识 -

    - ```
      -rw-rw-r-- 1 yangyadong yangyadong    6 12月 10 01:50 a  #/home/yangyadong/yydTestDir/a
      ```

  - 目录文件，即文件夹，标识d

    - ```
      drwxrwxr-x 2 yangyadong yangyadong 4096 12月 11 17:19 testDir  #/home/yangyadong/yydTestDir/testDir
      ```

  - 链接文件，相当于快捷方式

    - 软连接，标识 l。    

    - 软连接即快捷方式，跟原文件是两个文件

      - ```
        lrwxrwxrwx  1 root       root          15 12月 12 13:59 stdout -> /proc/self/fd/1   #/dev/stdout
        ```

    - 硬链接，标识 -，跟普通文件一样

    - 硬链接，实际上是为文件建一个别名，链接文件和原文件实际上是同一个文件

      - ```
        
        ```

  - 特殊文件（硬件设备文件）

    - 块设备文件。比如硬盘，随机按块进行读取。标识 b

      - ```
        brw-rw----  1 root       disk      8,   0 12月 12 13:59 sda  #/dev/sda
        brw-rw----  1 root       disk      8,   1 12月 12 13:59 sda1  #/dev/sda1
        ```

    - 字符设备文件。比如模拟终端设备，往里边输入命令，都是按字符一个一个输入的。输入有前后顺序，读取也是按序读取。标识 c

      - ```
        crw--w----  1 root       tty       4,  10 12月 12 13:59 tty10  #/dev/tty10
        crw--w---- 1 yangyadong tty  136, 0 12月 18 20:58 0  #/dev/pts/0
        ```

  - 套接字文件

    - socket。它可以让两个进程进行通信。它是使用软件模拟的设备（也可以说文件）。也就是说这些设备（文件）没有真正的硬件，是软件虚拟出来的，目的是为了让两个计算机的进程通信。标识 s

      - ```
        
        ```

  - 命名管道

    - pipe。标识 p

      - ```
        
        ```

### 3.6linux文件管理-文件拷贝命令cp

copy的简写，cp。把文件从一个目录拷贝到另一个目录

- 复制一模一样的文件

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls
    abc.txt  testDir
    yangyadong@ubuntu:~/yydTestDir$ cp ./abc.txt ./testDir/
    yangyadong@ubuntu:~/yydTestDir$ ls ./testDir/
    abc.txt
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 复制文件并重命名（也可以理解为复制内容）

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls
    abc.txt  testDir
    #复制abc.txt到路径./testDir/abc222.txt。名字不一样，即复制过去后重命名（也可以理解为复制内容过去）
    yangyadong@ubuntu:~/yydTestDir$ cp ./abc.txt ./testDir/abc222.txt
    yangyadong@ubuntu:~/yydTestDir$ ls ./testDir/
    abc222.txt
    #查看内容，./testDir/abc222.txt跟abc.txt内容一模一样
    yangyadong@ubuntu:~/yydTestDir$ cat ./testDir/abc222.txt
    111111122222
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 一口气复制多个文件到某一目录下

  - ```
    yangyadong@ubuntu:~/yydTestDir$ ls
    abc.txt  def.txt  testDir
    yangyadong@ubuntu:~/yydTestDir$ cp abc.txt def.txt ./testDir/
    yangyadong@ubuntu:~/yydTestDir$ ls -l ./testDir/*.txt
    -rw-rw-r-- 1 yangyadong yangyadong 13 12月 18 21:22 ./testDir/abc.txt
    -rw-rw-r-- 1 yangyadong yangyadong  0 12月 18 21:22 ./testDir/def.txt
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 当复制的文件，目标目录下已经有同名文件时，询问是否覆盖(加上-i )

  - 什么都不加时，默认是直接覆盖，不询问

  -  -i, --interactive            覆盖前询问 (会使得前面的-n选项失效)

  -   -n, --no-clobber              不要覆盖已存在的文件(使前面的 -i 选项失效)

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ cp -i abc.txt ./testDir/
    cp：是否覆盖'./testDir/abc.txt'？ y
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 把一个文件夹的所有文件拷贝到另一个文件夹

  -   -R, -r, --recursive           递归复制目录及其子目录内的所有内容
          --reflink[=WHEN]          控制克隆/CoW 副本。请查看下面的内如。
          --remove-destination      尝试打开目标文件前先删除已存在的目的地
                                            文件 (相对于 --force 选项)

  - ```shell
    yangyadong@ubuntu:~$ ls
    Desktop  Documents  Downloads  examples.desktop  Music  Pictures  Public  Templates  Videos  yydDir  yydTestDir
    #把./yydTestDir/整个文件夹拷贝到 /tmp/去
    yangyadong@ubuntu:~$ cp -r ./yydTestDir/ /tmp/
    #可以看到/tmp/下已经有yydTestDir文件夹了
    yangyadong@ubuntu:~$ ls -l /tmp/
    总用量 56
    -rw-r--r-- 1 root       root       2079 12月 12 13:59 _cafenv-appconfig_
    -rw------- 1 yangyadong yangyadong    0 12月 12 14:00 config-err-G9VOd6
    drwx------ 2 yangyadong yangyadong 4096 12月 12 14:00 ssh-diQ7A4EAilMg
    drwx------ 3 root       root       4096 12月 12 13:59 systemd-private-91088b2b494647db97d2f3a730b2f062-bolt.service-GohunG
    drwx------ 3 root       root       4096 12月 12 13:59 systemd-private-91088b2b494647db97d2f3a730b2f062-colord.service-q6nyzC
    drwx------ 3 root       root       4096 12月 18 19:16 systemd-private-91088b2b494647db97d2f3a730b2f062-fwupd.service-WqRhiS
    drwx------ 3 root       root       4096 12月 12 13:59 systemd-private-91088b2b494647db97d2f3a730b2f062-ModemManager.service-x1fzAu
    drwx------ 3 root       root       4096 12月 12 14:04 systemd-private-91088b2b494647db97d2f3a730b2f062-rtkit-daemon.service-6jM6hD
    drwx------ 3 root       root       4096 12月 12 13:59 systemd-private-91088b2b494647db97d2f3a730b2f062-systemd-resolved.service-K6vZJt
    drwx------ 3 root       root       4096 12月 12 13:59 systemd-private-91088b2b494647db97d2f3a730b2f062-systemd-timesyncd.service-Yw1tYR
    drwxrwxrwt 2 root       root       4096 12月 12 13:59 VMwareDnD
    drwx------ 2 root       root       4096 12月 18 21:37 vmware-root
    drwx------ 2 root       root       4096 12月 12 13:59 vmware-root_1424-2722697901
    drwx------ 2 yangyadong yangyadong 4096 12月 12 14:00 vmware-yangyadong
    drwxrwxr-x 3 yangyadong yangyadong 4096 12月 18 21:37 yydTestDir
    yangyadong@ubuntu:~$
    ```

- 拷贝的时候，文件权限会有变化，（只有管理员可以）使用参数可以保留原来的权限

  -   -p                            等于--preserve=模式,所有权,时间戳
          --preserve[=属性列表      保持指定的属性(默认：模式,所有权,时间戳)，如果
                                            可能保持附加属性：环境、链接、xattr 等
          --sno-preserve=属性列表   不保留指定的文件属性
          --parents                 复制前在目标目录创建来源文件路径中的所有目录

  - ```
    cp -p ... ...
    ```

- -d，等于--no-dereference --preserve=links，即保留文件的链接属性

  -   -P, --no-dereference          不跟随源文件中的符号链接
  -   --preserve[=属性列表      保持指定的属性(默认：模式,所有权,时间戳)，如果可能保持附加属性：环境、链接、xattr 等

- -a， -a, --archive                 等于-dR --preserve=all

  - 保留所有属性

### 3.7linux文件管理-文件删除rm和移动mv

rm：remove，删除移除

- 删除一个目录下所有文件

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls ./testDir/
    abc.txt  def.txt
    yangyadong@ubuntu:~/yydTestDir$ rm ./testDir/*
    yangyadong@ubuntu:~/yydTestDir$ ls ./testDir/
    yangyadong@ubuntu:
    ```

- 递归删除一个文件夹下所有内容（包括目录，包括文件）

  - -r 递归删除

  - -i 每次删除都询问

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ tree
    .
    ├── abc.txt
    ├── def.txt
    └── testDir
        ├── 123
        │   └── b.txt
        └── a.txt
    
    2 directories, 4 files
    #要是写 rm ./testDir/* -r -i，那就只删除./testDir/里的内容，不删testDir目录。
    #写rm ./testDir/ -r -i，那就删除./testDir/里的内容，testDir目录也删了。
    yangyadong@ubuntu:~/yydTestDir$ rm ./testDir/ -r -i
    rm：是否进入目录'./testDir/'? y
    rm：是否进入目录'./testDir/123'? y
    rm：是否删除普通空文件 './testDir/123/b.txt'？ y
    rm：是否删除目录 './testDir/123'？ y
    rm：是否删除普通空文件 './testDir/a.txt'？ y
    rm：是否删除目录 './testDir/'？ y
    yangyadong@ubuntu:~/yydTestDir$ tree
    .
    ├── abc.txt
    └── def.txt
    
    0 directories, 2 files
    yangyadong@ubuntu:~/yydTestDir$
    
    ```

- 强制删除

  - -f

mv: move移动

- 可以移动文件

  - ```shell
    yangyadong@ubuntu:~$ ls
    Desktop  Documents  Downloads  examples.desktop  Music  Pictures  Public  Templates  Videos  yydDir  yydTestDir  yydTestDir2
    #把yydTestDir目录下所有文件移动到yydTestDir2目录下
    yangyadong@ubuntu:~$ mv yydTestDir/* yydTestDir2/
    #移动完，可以看到yydTestDir空了。
    yangyadong@ubuntu:~$ tree yydTestDir/
    yydTestDir/
    
    0 directories, 0 files
    #都被移动到yydTestDir2下了
    yangyadong@ubuntu:~$ tree yydTestDir2/
    yydTestDir2/
    ├── abc.txt
    ├── def.txt
    └── testDir
    
    1 directory, 2 files
    yangyadong@ubuntu:~$
    ```

- 可以移动文件夹

  - ```shell
    yangyadong@ubuntu:~$ ls
    Desktop  Documents  Downloads  examples.desktop  Music  Pictures  Public  Templates  Videos  yydDir  yydTestDir  yydTestDir2
    #将整个文件夹yydTestDir2移动到yydTestDir下
    yangyadong@ubuntu:~$ mv yydTestDir2/ yydTestDir/
    #可以看到yydTestDir2/被移动过去了
    yangyadong@ubuntu:~$ tree yydTestDir/
    yydTestDir/
    └── yydTestDir2
        ├── abc.txt
        ├── def.txt
        └── testDir
    
    2 directories, 2 files
    yangyadong@ubuntu:~$
    
    ```

- 可以移动文件的时候顺便重命名

  - ```shell
    yangyadong@ubuntu:~$ tree yydTestDir/
    yydTestDir/
    └── yydTestDir2
        ├── abc.txt
        ├── def.txt
        └── testDir
    
    2 directories, 2 files
    #移动文件的时候顺便重命名
    yangyadong@ubuntu:~$ mv ./yydTestDir/yydTestDir2/abc.txt ./yydTestDir/aaa.txt
    #可以看到./yydTestDir/yydTestDir2/abc.txt被移动到了./yydTestDir/下，还被重命名成了aaa.txt
    yangyadong@ubuntu:~$ tree yydTestDir/
    yydTestDir/
    ├── aaa.txt
    └── yydTestDir2
        ├── def.txt
        └── testDir
    
    2 directories, 2 files
    yangyadong@ubuntu:~$
    ```

- 文件跟文件夹不可以同名同级存在。（因为其实文件夹也是个文件，只是目录文件而已，属性是d。而普通文件属性-。不以属性不同区分不同文件。仅以名字区分。）

### 3.8 linux文件管理-touch命令更改文件时间戳

时间戳有：

- 最近一次访问时间。（打开过它）
- 最近一次修改时间。（改变文件内容）
- 最近一次改变时间。（元数据的改变。包括文件名、大小、权限、文件内容等等的改变）



- 查看时间戳

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ stat aaa.txt
      文件：aaa.txt
      大小：19              块：8          IO 块：4096   普通文件
    设备：801h/2049d        Inode：262681      硬链接：1
    权限：(0664/-rw-rw-r--)  Uid：( 1000/yangyadong)   Gid：( 1000/yangyadong)
    最近访问：2020-12-19 15:28:42.191769104 +0800
    最近更改：2020-12-19 15:28:33.199859923 +0800
    最近改动：2020-12-19 15:28:33.199859923 +0800
    创建时间：-
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 更新时间戳为当前

  - ```shell
    #touch一下那个文件
    yangyadong@ubuntu:~/yydTestDir$ touch aaa.txt
    #3个时间戳，全部更新为当前时间
    yangyadong@ubuntu:~/yydTestDir$ stat aaa.txt
      文件：aaa.txt
      大小：19              块：8          IO 块：4096   普通文件
    设备：801h/2049d        Inode：262681      硬链接：1
    权限：(0664/-rw-rw-r--)  Uid：( 1000/yangyadong)   Gid：( 1000/yangyadong)
    最近访问：2020-12-19 15:33:07.444967351 +0800
    最近更改：2020-12-19 15:33:07.444967351 +0800
    最近改动：2020-12-19 15:33:07.444967351 +0800
    创建时间：-
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 更改时间戳为指定时间

  - ![image-20201219153819963](https://i.loli.net/2020/12/19/e3WsORgBkXiL2l7.png)

- 如果文件不存在，那么不创建

  - 使用touch -c选项

### 3.9文件查找find

- 按名称和文件类型查找

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls
    aaa.txt  abc.txt  yydTestDir2
    yangyadong@ubuntu:~/yydTestDir$ find -name "a*"
    ./abc.txt
    ./aaa.txt
    yangyadong@ubuntu:~/yydTestDir$
    ```

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls
    aaa.txt  abc.txt  def.txt  yydTestDir2
    #-o ，或者的意思。
    yangyadong@ubuntu:~/yydTestDir$ find -name "a*" -o -name "d*"
    ./def.txt
    ./abc.txt
    ./yydTestDir2/def.txt
    ./aaa.txt
    yangyadong@ubuntu:~/yydTestDir$
    ```

### 3.10文本查看命令cat、head等等

#### cat用途

- 可以显示文本文件的内容
- 也可以显示多个文本文件的内容

举例

- ```shell
  #显示文本文件的内容
  yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt
  1111111222223
  4444
  #显示多个文本文件的内容
  yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt abc.txt
  1111111222223
  4444
  abcabcabc
  yangyadong@ubuntu:~/yydTestDir$
  #显示并编号
  yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt abc.txt -n
       1  1111111222223
       2  4444
       3  abcabcabc
  yangyadong@ubuntu:~/yydTestDir$
  
  ```

#### 按行逆向显示文本内容：tac

- ```shell
  yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt abc.txt
  1111111222223
  4444
  abcabcabc
  yangyadong@ubuntu:~/yydTestDir$ tac aaa.txt abc.txt
  4444
  1111111222223
  abcabcabc
  yangyadong@ubuntu:~/yydTestDir$
  ```

#### more

- 分屏显示文本文件内容，从前往后翻。
- enter键往下翻。空格键一屏幕一屏幕的翻
- b键往前翻，翻到文件的尾部，显示结束就不能往前翻了

![image-20201222001908483](https://i.loli.net/2020/12/22/vGhglfka6Rd8MXn.png)

#### less

- 和more的区别在于，当显示到文本的尾部时，还可以往前翻。

![image-20201222002214944](https://i.loli.net/2020/12/22/YcuJzgtZWOqLMGR.png)

#### head

显示文本文件的前几行，默认显示前10行

```shell
yangyadong@ubuntu:~$ head /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
#只显示前两行
yangyadong@ubuntu:~$ head -n 2 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
yangyadong@ubuntu:~$

```

#### tail

显示文本文件的最后几行

![image-20201222002725386](https://i.loli.net/2020/12/22/osMBuX5EClQGPiq.png)

-f 一直跟踪，不再退出。crtl+C退出

### 3.11文本编辑器vim、nano等

- vim

  - vim是linux默认安装的

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ vim test.md
    #可以直接新建一个文件并编辑
    ```

  - 输入i，变为insert编辑模式

  - esc退出insert模式

  - :wq 写入退出

  - :w 保存

  - :q!退出不保存

- nano

  - nano不会默认安装
  - ctrl+各种命令。nano里有提示

- xftp

  - windows下方便的远程连接linux进行文件操作

### 3.12文本操作命令cut和tr

#### 文本操作命令

- 它不改变文本本身的内容，而是改变显示而已。
- 按照我们想要的格式显示文件内容，可以对文件内容进行过滤，改变样式输出

#### cut

- 将文件使用指定的字符切开，显示我们想要的那些列

- ```shell
  #查看一下文本
  yangyadong@ubuntu:~$ cat /etc/passwd
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  sys:x:3:3:sys:/dev:/usr/sbin/nologin
  sync:x:4:65534:sync:/bin:/bin/sync
  games:x:5:60:games:/usr/games:/usr/sbin/nologin
  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
  lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
  mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
  yangyadong@ubuntu:~$
  #用冒号切开文本，只看第一列和第二列
  yangyadong@ubuntu:~$ cut -d : -f 1,2 /etc/passwd
  root:x
  daemon:x
  bin:x
  sys:x
  sync:x
  games:x
  man:x
  lp:x
  mail:x
  ```

#### Tr（只是影响显示，不影响原文件）

- translate的简写。翻译，转化。

- 能够把文件中指定字符进行替换

- tr命令不可以直接跟文件，需要输入重定向 <

- ```shell
  yangyadong@ubuntu:~$ cat /etc/passwd
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  sys:x:3:3:sys:/dev:/usr/sbin/nologin
  sync:x:4:65534:sync:/bin:/bin/sync
  games:x:5:60:games:/usr/games:/usr/sbin/nologin
  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
  lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
  mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
  
  #用空格带代替冒号进行显示
  #tr命令不可以直接跟文件，需要输入重定向 <
  yangyadong@ubuntu:~$ tr ':' ' ' < /etc/passwd
  root x 0 0 root /root /bin/bash
  daemon x 1 1 daemon /usr/sbin /usr/sbin/nologin
  bin x 2 2 bin /bin /usr/sbin/nologin
  sys x 3 3 sys /dev /usr/sbin/nologin
  sync x 4 65534 sync /bin /bin/sync
  games x 5 60 games /usr/games /usr/sbin/nologin
  man x 6 12 man /var/cache/man /usr/sbin/nologin
  lp x 7 7 lp /var/spool/lpd /usr/sbin/nologin
  mail x 8 8 mail /var/mail /usr/sbin/nologin
  yangyadong@ubuntu:~$
  ```

### 3.13 wc、sort、uniq

#### wc

- word count 字数统计。可以统计文件的行数、单词数、字符数量（包含空格和换行符）

- ```shell
  yangyadong@ubuntu:~/yydTestDir$ cat 111.md
  aaaa 1111
  bbbbbbbbb 2222
  ccccccccc
  ddddddddd
  #4行，6个单词，45个字符
  yangyadong@ubuntu:~/yydTestDir$ wc 111.md
   4  6 45 111.md
  yangyadong@ubuntu:~/yydTestDir$
  #只看行数
  yangyadong@ubuntu:~/yydTestDir$ wc -l 111.md
  4 111.md
  yangyadong@ubuntu:~/yydTestDir$
  #只看单词数量
  yangyadong@ubuntu:~/yydTestDir$ wc -w 111.md
  6 111.md
  yangyadong@ubuntu:~/yydTestDir$
  #只看字符数量
  yangyadong@ubuntu:~/yydTestDir$ wc -c 111.md
  45 111.md
  yangyadong@ubuntu:~/yydTestDir$
  ```

#### sort

- 对**文本中的行**进行排序

- ```shell
  yangyadong@ubuntu:~/yydTestDir$ cat aaa.md
  dddd 22
  aaaa 11
  cccc 22
  bbbb 11
  #默认升序
  yangyadong@ubuntu:~/yydTestDir$ sort aaa.md
  aaaa 11
  bbbb 11
  cccc 22
  dddd 22
  yangyadong@ubuntu:~/yydTestDir$
  #-r 降序
  yangyadong@ubuntu:~/yydTestDir$ sort -r aaa.md
  dddd 22
  cccc 22
  bbbb 11
  aaaa 11
  yangyadong@ubuntu:~/yydTestDir$
  #先把文件按空格切割，并按照切割后的第二列对文件进行排序
  yangyadong@ubuntu:~/yydTestDir$ sort -t " " -k 2 aaa.md
  aaaa 11
  bbbb 11
  cccc 22
  dddd 22
  yangyadong@ubuntu:~/yydTestDir$
  
  ```

#### uniq

- unique的简写，唯一。可以针对文本中的连续的重复行进行操作。

- ![image-20201223001442947](https://i.loli.net/2020/12/23/biu54QcRg1OXnds.png)

  - 可以看到只删去了相邻的重复行。不相邻的不管。

- -c选项，显示重复了几次

  - ![image-20201223001724963](https://i.loli.net/2020/12/23/VbeuWj6yYidtazs.png)

- -d选项，只显示重复行

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ cat 111.txt
    wang student 23
    wang student 23
    han taacher 33
    liu teacher 29
    zhang student 23
    wang student 23
    yangyadong@ubuntu:~/yydTestDir$ uniq 111.txt
    wang student 23
    han taacher 33
    liu teacher 29
    zhang student 23
    wang student 23
    yangyadong@ubuntu:~/yydTestDir$ uniq 111.txt -d
    wang student 23
    yangyadong@ubuntu:~/yydTestDir$
    ```

### 3.14 Grep命令--文本过滤工具，将搜索到的行打印出来

- Global RE Printing。**全局搜索正则表达式，并将搜索到的行打印出来。**

- RE：Regular Expresssion 正则表达式。由一些元字符和其他字符组成的表达式。

  - 元字符：有特殊意义的字符，并不代表这个字符本身

- 正则表达式RE有基本的正则表达式和扩展的正则表达式。

  - Basic RE。基本的正则表达式
  - Extended RE。扩展的正则表达式

- Grep能够根据人为指定的正则表达式，逐行扫描文件的内容。只要行中有满足正则表达式的内容，那么这行将被输出

- ```shell
  #查看cpu信息
  yangyadong@ubuntu:~/yydTestDir$ cat /proc/cpuinfo
  processor       : 0
  vendor_id       : GenuineIntel
  cpu family      : 6
  model           : 158
  model name      : Intel(R) Core(TM) i7-8700 CPU @ 3.20GHz
  stepping        : 10
  microcode       : 0xb4
  cpu MHz         : 3192.001
  cache size      : 12288 KB
  physical id     : 0
  siblings        : 1
  core id         : 0
  cpu cores       : 1
  apicid          : 0
  initial apicid  : 0
  fpu             : yes
  fpu_exception   : yes
  cpuid level     : 22
  wp              : yes
  flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single pti ssbd ibrs ibpb stibp fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves arat md_clear flush_l1d arch_capabilities
  bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit srbds
  bogomips        : 6384.00
  clflush size    : 64
  cache_alignment : 64
  address sizes   : 45 bits physical, 48 bits virtual
  power management:
  
  processor       : 1
  vendor_id       : GenuineIntel
  cpu family      : 6
  model           : 158
  model name      : Intel(R) Core(TM) i7-8700 CPU @ 3.20GHz
  stepping        : 10
  microcode       : 0xb4
  cpu MHz         : 3192.001
  cache size      : 12288 KB
  physical id     : 2
  siblings        : 1
  core id         : 0
  cpu cores       : 1
  apicid          : 2
  initial apicid  : 2
  fpu             : yes
  fpu_exception   : yes
  cpuid level     : 22
  wp              : yes
  flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single pti ssbd ibrs ibpb stibp fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves arat md_clear flush_l1d arch_capabilities
  bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit srbds
  bogomips        : 6384.00
  clflush size    : 64
  cache_alignment : 64
  address sizes   : 45 bits physical, 48 bits virtual
  power management:
  
  yangyadong@ubuntu:~/yydTestDir$
  #只显示/proc/cpuinfo中，有cpu单词出现的行
  yangyadong@ubuntu:~/yydTestDir$grep 'cpu' /proc/cpuinfo
  ```

  ![image-20201223002700981](https://i.loli.net/2020/12/23/gmJpyZ21lVsezv9.png)

- 查看**没有**cpu出现的行 

  - 使用-v选项
  - grep -v 'cpu' /proc/cpuinfo
  - ![image-20201223002845895](https://i.loli.net/2020/12/23/guf2WDVHFJXBKdG.png)

- -i 不区分大小写

- -A 1 。把找到的那一行的下一行也显示出来

  - ![image-20201223003348001](https://i.loli.net/2020/12/23/LlB6Uap7ktPDcOQ.png)

- -B 2。把找到的那一行的上两行也显示出来

  - ![image-20201223003328973](https://i.loli.net/2020/12/23/THX5kjYUdFGJiS1.png)

- -C 1。把找到的那一行的上一行和下一行都显示出来

  - ![image-20201223003410553](https://i.loli.net/2020/12/23/hGYOlo6Caqx4BwD.png)

- Grep不仅可以对文件操作。使用管道技术，还可以筛选命令行的输出

  - ![image-20201223003620185](https://i.loli.net/2020/12/23/2Qb9R3EgYi7e1PZ.png)

### 3.15 正则表达式

匹配或者说是代表，都是一个意思。

首先回顾一下，文件名通配符的元字符（它不是正则表达式，不可以使用）

```
* 代表任意长度任意字符

？代表任意单个字符

[] 代表指定范围的任意单个字符

[^]代表指定范围外的任意单个字符
```

#### 正则表达式的元字符

- 正则表达式的元字符

  - ```
    .  匹配（代表）任意单个字符
    
    [] 匹配指定范围内的任意单个字符
    
    [^]匹配指定范围外的任意单个字符
    
    .*匹配任意长度的任意字符。因为.代表匹配任意单个字符，*表示匹配前一个字符0次或无穷次
    ```

  - 示例

  - ![image-20201228000708040](https://i.loli.net/2020/12/28/7g2iO3eRmVWcBFz.png)

#### 匹配次数

- 匹配次数

  - ```
    * 匹配前一个字符0次或无穷次
    例：
    正则表达式ab*c    它代表b可以出现0次到多次。  ac abc abbc abbbc都可以跟这个表达式匹配。abbdc不行。
    ```

    - ![image-20201228001217785](https://i.loli.net/2020/12/28/51WMRpf8lZtHyuk.png)

  - ```
    \? 匹配前一个字符0次或1次
    例：
    正则表达式ab\?c    它代表b可以出现0次或1次。  ac  abc可以匹配  abbc不可以匹配
    ```

    - ![image-20201228001440586](https://i.loli.net/2020/12/28/tulPRiy4TkYGQg1.png)

  - ```
    \{m,n\} 匹配前一个字符最少m次，最多n次
    \{0,n\} 匹配前一个字符最少0次，最多n次
    \{m\} 匹配前一个字符最少m次
    例：
    正则表达式ab\{0,2\}c    它代表b可以出现0次到2次。  ac abc abbc可以匹配   abbbc abdc不可以
    这个表达式a[a-z]\{0,2\}c   代表[a-z]可以出现0次到2次  那么 ac abc abbc abdc都可以匹配， abdc不可以
    ```

    - ![image-20201228002347215](https://i.loli.net/2020/12/28/JkiA4GSP7BeLd26.png)

  - ```
    .* 代表任意长度任意字符
    例：
    正则表达式a.*c      那么ac abc abbc abbbc abdc a123bdc都可以匹配
    ```

    - ![image-20201228002734149](https://i.loli.net/2020/12/28/2quWnTOgh4ZYvF1.png)

#### 分组匹配

- 例1 字符串91xueithanhanhanhan51cto

- **用括号括起来即分组了，紧挨着的内容重复多少次**

  - 可以用

    ```
     '91xueit\(han\)*51cto'
     *代表 \(han\)可以重复任意次
    ```

  - 来匹配

  - ![image-20201228134548542](https://i.loli.net/2020/12/28/zinh36WD29Jg7Z4.png)

- 例2 

- **向后引用，把前边需要重复的内容用括号括起来，拿到后边去重复。**

  - student.txt

  - ```
    zhangsan,math=good,english=bad
    hanligang,math=good,english=good
    yixiu,math=bad,english=good
    xiaomo,math=bad,english=bad,music=good,geography=good
    yuanzi,math=good,english=bad,music=good,geography=good
    ```

  - 比如我想找到，这个文件里，math和english成绩一样的学生

  - ```
    grep '.*,math=\(.*\),english=\1' student.txt
    一 .*，即任意长度任意字符串
    二 ,
    三 math=
    四 \(.*\)      分组匹配，第一次使用括号即第一组
    五 ,
    六 english=
    七 \1           分组匹配，跟第一个括号里的内容要一样
    ```

    

    - ![image-20201228135658571](https://i.loli.net/2020/12/28/jYqQUTve9X23owN.png)

  - 比如我想找到，这个文件里，math和english成绩一样，music和geography成绩一样的学生

  - ```
    grep '.*,math=\(.*\),english=\1,music=\(.*\),geography=\2' student.txt
    
    \(\)  就代表分组匹配，第一次出现的括号即第一组
    \1  就代表要跟第一组括号里的内容一样
    \2  即代表要跟第二组括号里的内容一样
    ```

    - ![image-20201228140450360](https://i.loli.net/2020/12/28/zVgtxFoscyJLQnv.png)

####  锚定符

- 锚定词首

  - ```
    \<
    ```

  - ```
    grep '\<r..t' test.txt
    锚定到，以r..t开头的单词。
    ```

    

  - ![image-20201228234438812](https://i.loli.net/2020/12/28/2NWeMJKsTCSbqUh.png)

- 锚定词尾

  - ```
    \>
    ```

  - ![image-20201228234634738](https://i.loli.net/2020/12/28/xAD4jyLn1PtGNEY.png)

- 锚定单词

  - ```
    \< \>
    ```

  - ![image-20201228234748524](https://i.loli.net/2020/12/28/OTVzYkpv9LjBrte.png)

- 锚定行首（以..开头）

  - ```
    ^
    ```

  - 查找以root开头的行

  - ![image-20201228234927273](https://i.loli.net/2020/12/28/BpAGPzKNQs9CdWv.png)

  - 假如开头还有很多空格

  - ![image-20201228235217629](https://i.loli.net/2020/12/28/AJUYRzxck9fSWeo.png)

- 锚定行尾（以..j结尾）

  - ```
    $
    ```

  - ![image-20201228235016172](https://i.loli.net/2020/12/28/RXe6FCcg5smkd3b.png)

  -  锚定结尾是root的，有或者没有标点都行

  - ![image-20201228235504456](https://i.loli.net/2020/12/28/FyQAsOgq3UGNB6d.png)

#### 在正则表达式中使用字符类

- ![image-20201229000005910](https://i.loli.net/2020/12/29/jnh68vxSXBLGwHV.png)

- posix定义了一些只能在正则表达式中使用的字符类

  - ```
    alnum 字母和数字。
    alpha 字母。
    blank 仅表示空格或制表符。
    cntrl 控制字符。
    digit 十进制数
    graph 打印字符，不包含空格。
    lower 小写字母。
    print 打印字符，包含空格。
    punct 打印字符，不包含字母和数字。
    space 空白。
    upper 大写字母。
    xdigit 十六进制数。
    想使用，必须得
    [[:...:]]
    比如 [[:space:]]
    ```

- ![image-20201229001452033](https://i.loli.net/2020/12/29/wLOfp1v9BysEziI.png)



### 3.16正则表达式练习题

#### 找出ifconfig命令结果中1-255之间的整数

- ```
  ifconfig | grep '[1-9]\{1,3\}'
  #筛选出：ifconfig的结果中 满足[1-9]这个单个字符，出现1-3次 这个条件的行
  ```

  - ![image-20210101150136750](https://i.loli.net/2021/01/01/Z78QYuEhyaWMKbs.png)

- ```
  ifconfig | grep '[1-9]\{1,3\}' -o 只显示匹配的内容，不显示整行
  ```

  - ![image-20210101150325447](https://i.loli.net/2021/01/01/2UXGqEy1QjWh6Jk.png)

#### 找出ifconfig命令结果中的IP地址

- ```
  ifconfig | grep '\([[:digit:]]\{1,3\}\.\)\{3,3\}[[:digit:]]\{1,3\}'
  #192.168.0.1
  #[[:digit:]]数字，重复1到3遍，后边加个点（不加点就代表任意字符了，加个转义字符就表示原本意思了）    这即192.了
  #括号括起来，合为一组，必须重复3遍    即 192.168.0.了
  #最后再加一组[[:digit:]]\{1,3\}    即192.168.0.1了
  ```

  - ![image-20210101151901304](https://i.loli.net/2021/01/01/aPrHwinuOYZBvMR.png)

#### 分析以下文件，要求写一个正则表达式，找出第4位和最后一位字符数值一样的行

- ```
  L1:1:wait:/etc/rc.d/rc 1
  L3:3:wait:/etc/rc.d/rc 3
  L1:1:wait:/etc/rc.d/rc 3
  L3:1:wait:/etc/rc.d/rc 3
  ```

- ```
  grep 'L.:\(.\).*\1' test.md
  #L 任意字符 :
  #第四位用括号括起来，并且任意
  #然后.* 任意位任意字符
  #最后有一个\1，表示要跟之前的分组匹配，内容一致
  ```

  - ![image-20210101153021099](https://i.loli.net/2021/01/01/O957IJiAUPF1WwM.png)

#### 找出当前系统上名字为yangyadong的用户的相关信息 /etc/passwd，注意yangyadong1都不算

- ```
  使用锚定
  grep '\<yangyadong\>' /etc/passwd
  <\yangyaodng\>限定是个完整的单词，它后边再加1都不行
  ```

- ![image-20210101154903148](https://i.loli.net/2021/01/01/AZxX1yRbd4sNk5K.png)

### 3.17扩展正则表达式

- 使用egrep或者grep -E，就支持扩展的正则表达式了

  - ![image-20210101163856889](https://i.loli.net/2021/01/01/AXW8vgsyiNqcQ6G.png) 

- 跟基本正则表达式的异同

- 元字符，与基本正则表达式完全一样

  - ```
    .  匹配（代表）任意单个字符
    
    [] 匹配指定范围内的任意单个字符
    
    [^]匹配指定范围外的任意单个字符
    ```

- 匹配次数

  - ```
    *  匹配前一个字符0次或无穷次。(跟基本正则表达式一样)  .* 代表任意长度任意字符
    
    \? 匹配前一个字符0次或1次(跟基本正则表达式一样)
     
    +  匹配前一个至少一次(基本正则表达式没有)
    
    {m,n}匹配前一个字符至少m次，最多n次 (与基本正则表达式相比，不再需要转义字符了)
    
    {0,n} 匹配前一个字符最少0次，最多n次 (与基本正则表达式相比，不再需要转义字符了)
    
    {m} 匹配前一个字符最少m次(与基本正则表达式相比，不再需要转义字符了)
    
    ```

  - ![image-20210101162407794](https://i.loli.net/2021/01/01/4v9kawZFfemRNGz.png)

- 锚定符

  - ```
    锚定词首
    \<   或者  \b    (基本正则表达式里只支持\<)
    
    锚定词尾
    \>	或者  \b      (基本正则表达式里只支持\>)
    
    锚定单词
    \<内容\>   ：比如，\<yaya\>，那么yangyadong就不算匹配上了
    或者\b内容\b       (基本正则表达式里只支持\<\>)
    
    锚定行首
    ^
    
    锚定行尾
    $
    ```

- 或者（扩展正则表达式新增的）

  - ```
    |
    abc | wde，即abc或者wde
    ab(c|w)de 即abcde或者abwde
    ```

  - ![image-20210101163317888](https://i.loli.net/2021/01/01/nAN1qkWx84hXE6V.png)

- 分组匹配（**括号不用转义字符了**）

  - ```
    '91xueit(han)*51cto'
    grep '.*,math=(.*),english=\1,music=(.*),geography=\' student.txt
    
    (内容)  就代表分组匹配，第一次出现的括号即第一组。括号不再需要转义字符了。
    \1  就代表要跟第一组括号里的内容一样
    \2  即代表要跟第二组括号里的内容一样
    ```

  - ![image-20210101163527406](https://i.loli.net/2021/01/01/482bGKedCZATYXx.png)

  - ![image-20210101163658755](https://i.loli.net/2021/01/01/XEnDiryWwsvQAep.png)

- 

## 10.总结

- 文件跟文件夹不可以同名同级存在。（因为其实文件夹也是个文件，只是目录文件而已，属性是d。而普通文件属性-。不以属性不同区分不同文件。仅以名字区分。）

- 快捷编辑

  - ```
    - 光标快速移动
      - ctrl+a 快速跳转到行首
      - ctrl+e 快速跳转到行尾
    - 删除命令行中内容
      - ctrl+w 删除光标前一个单词
      - ctrl+u 删除光标到行首的字符
      - ctrl+k 删除光标到行尾的字符
    - 清屏
      - ctrl+l
      - （windows下是cls）
    - 取消不执行的命令
      - ctrl+c
    ```

- type

  - ```shell
    type pwd#pwd是内建
    type ping#ping 是 /bin/ping
    ```

- 文件名通配符

  - ```shell
    * 
    ? 
    [[:digit:]]
    ```

- 命令别名

  - ```shell
    alias pingbd='ping www.baidu.com'
    ```

- 命令替换

  - ```shell
    $(命令）
    或者
    `命令`
    ```

- 路径展开

  - ```shell
    {}
    #/tmp/{zz,yy}/a/b即/tmp/zz/a/b/ 和/tmp/yy/a/b
    #mkdir -v是显示创建过程 -p是递归创建
    #为啥一定要-p递归创建，因为/tmp/zz这层目录不创建的话，你没法创建/tmp/zz/a，更别说/tmp/zz/z/b了
    yangyadong@ubuntu:~/yydTestDir$ mkdir /tmp/{zz,yy}/a/b -v -p
    mkdir: 已创建目录 '/tmp/zz'
    mkdir: 已创建目录 '/tmp/zz/a'
    mkdir: 已创建目录 '/tmp/zz/a/b'
    mkdir: 已创建目录 '/tmp/yy'
    mkdir: 已创建目录 '/tmp/yy/a'
    mkdir: 已创建目录 '/tmp/yy/a/b'
    yangyadong@ubuntu:~/yydTestDir$
    ```

- 重定向

  - ```shell
    ifconfig ens33 1>yydTest.md
    ```

- 管道技术

  - ```shell
    |
    上一个命令的标准输出做下一个命令的标准输入
    ```

- 命令、命令参数、选项、选项参数

- cd

  - ```shell
    cd
    cd ~
    cd ~yangyadong
    cd -
    ```

- ls

  - ```shell
    ls -als | cat -n #详细列出所有文件和隐藏文件，并编号
    ls -ls | cat -n#详细列出所有文件，不显示隐藏文件，并编号
    ls | cat -n#列出文件，不显示隐藏文件，并编号
    ```

- mkdir

  - ```shell
    mkdir /tmp/a/b/c -pv#-p创建父目录，如果没有。 -v显示详细创建过程。
    #c也是一个文件夹
    ```
  
- rmdir

  - **只能删除空文件夹。**

  - 直接删除一个文件夹无所谓，但是**想用-p递归删除时，切记切换目录，使用相对路径-p删除**。切记不要使用绝对路径-p来递归删除

  - ```shell
    yangyadong@DESKTOP-9LD4MQ0:~$ sudo mkdir /abc/a1/a2 -pv
    mkdir: created directory '/abc'
    mkdir: created directory '/abc/a1'
    mkdir: created directory '/abc/a1/a2'
    #注意！千万不能写rmdir /abc/a1/a2 -pv。这样会试图删除根目录/。因为abc是在根目录/下的
    #所以想递归删除，应该切换过去使用相对路径进行删除
    yangyadong@DESKTOP-9LD4MQ0:~$ cd /
    yangyadong@DESKTOP-9LD4MQ0:/$
    #注意不要写rmdir /abc/a1/a2 -pv。这样还是绝对路径，会试图删除根目录/
    #切换目录后使用相对路径也不要写./abc/a1/a2 -pv，这样还会试图删除'.'目录
    #而是应该切换目录后，rmdir abc/a1/a2 -pv
    yangyadong@DESKTOP-9LD4MQ0:/$ sudo rmdir abc/a1/a2/ -pv
    rmdir: removing directory, 'abc/a1/a2/'
    rmdir: removing directory, 'abc/a1'
    rmdir: removing directory, 'abc'
    ```

- tree

  - 查看目录结构

  - ```shell
    yangyadong@DESKTOP-9LD4MQ0:~$ sudo mkdir /abc/{a1/a2,b1,c1} -pv
    mkdir: created directory '/abc'
    mkdir: created directory '/abc/a1'
    mkdir: created directory '/abc/a1/a2'
    mkdir: created directory '/abc/b1'
    mkdir: created directory '/abc/c1'
    yangyadong@DESKTOP-9LD4MQ0:~$ tree /abc
    /abc
    ├── a1
    │   └── a2
    ├── b1
    └── c1
    
    4 directories, 0 files
    yangyadong@DESKTOP-9LD4MQ0:~$cd /
    yangyadong@DESKTOP-9LD4MQ0:/$
    yangyadong@DESKTOP-9LD4MQ0:/$ sudo rmdir abc/{a1/a2,b1,c1} -pv
    rmdir: removing directory, 'abc/a1/a2'
    rmdir: removing directory, 'abc/a1'
    rmdir: removing directory, 'abc'
    rmdir: failed to remove directory 'abc': Directory not empty
    rmdir: removing directory, 'abc/b1'
    rmdir: removing directory, 'abc'
    rmdir: failed to remove directory 'abc': Directory not empty
    rmdir: removing directory, 'abc/c1'
    rmdir: removing directory, 'abc'
    yangyadong@DESKTOP-9LD4MQ0:/$ ls
    bin  boot  dev  etc  home  init  lib  lib64  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var
    yangyadong@DESKTOP-9LD4MQ0:/$
    ```

- 文件类型

  - ![image.png](https://i.loli.net/2020/12/18/PwKDCdXYqJj9yxZ.png)
  - 第一个字母。
  - 普通文件 - （查找的时候要用f）
  - 目录文件 d 
  - 软连接文件 l
  - 块设备文件。比如硬盘，随机按块进行读取。b
  - 字符设备文件。c
  - 套接字文件。s
  - 命名管道。p

- cp

  - 文件拷贝命令

- rm

  - 删除。文件或者文件夹。

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ rm ./testDir/*
    ```

- mv

  - 移动或者重命名

  - ```shell
    #aaa.txt重命名为aaa2.txt
    yangyadong@ubuntu:~/yydTestDir$mv aaa.txt aaa2.txt
    ```

- stat

  - ```shell
    #查看时间戳
    yangyadong@ubuntu:~/yydTestDir$ stat aaa.txt
      文件：aaa.txt
      大小：19              块：8          IO 块：4096   普通文件
    设备：801h/2049d        Inode：262681      硬链接：1
    权限：(0664/-rw-rw-r--)  Uid：( 1000/yangyadong)   Gid：( 1000/yangyadong)
    最近访问：2020-12-19 15:28:42.191769104 +0800
    最近更改：2020-12-19 15:28:33.199859923 +0800
    最近改动：2020-12-19 15:28:33.199859923 +0800
    创建时间：-
    yangyadong@ubuntu:~/yydTestDir$
    ```

- touch（用来更改时间戳的，可以更新为当前。也可以指定为任意时间），也可以用来创建文件

  - ```shell
    #touch一下那个文件
    yangyadong@ubuntu:~/yydTestDir$ touch aaa.txt
    #3个时间戳，全部更新为当前时间
    yangyadong@ubuntu:~/yydTestDir$ stat aaa.txt
      文件：aaa.txt
      大小：19              块：8          IO 块：4096   普通文件
    设备：801h/2049d        Inode：262681      硬链接：1
    权限：(0664/-rw-rw-r--)  Uid：( 1000/yangyadong)   Gid：( 1000/yangyadong)
    最近访问：2020-12-19 15:33:07.444967351 +0800
    最近更改：2020-12-19 15:33:07.444967351 +0800
    最近改动：2020-12-19 15:33:07.444967351 +0800
    创建时间：-
    yangyadong@ubuntu:~/yydTestDir$
    ```

- file

  - 查看文件类型

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ file aaa.txt
    #这是个ascii文本
    aaa.txt: ASCII text
    yangyadong@ubuntu:~/yydTestDir$
    yangyadong@ubuntu:~/yydTestDir$ file /bin/cat
    #这是一个64位可执行程序
    /bin/cat: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=747e524bc20d33ce25ed4aea108e3025e5c3b78f, stripped
    yangyadong@ubuntu:~/yydTestDir$
    ```

- find

  - 查找

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ ls
    aaa.txt  abc.txt  def.txt  yydTestDir2
    #-o ，或者的意思。
    yangyadong@ubuntu:~/yydTestDir$ find -name "a*" -o -name "d*"
    ./def.txt
    ./abc.txt
    ./yydTestDir2/def.txt
    ./aaa.txt
    yangyadong@ubuntu:~/yydTestDir$
    ```

- cat

  - 查看文本内容

  - ```shell
    #显示并编号
    yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt abc.txt -n
         1  1111111222223
         2  4444
         3  abcabcabc
    yangyadong@ubuntu:~/yydTestDir$
    ```

- tac

  - 按行逆向显示文本内容

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ cat aaa.txt abc.txt
    1111111222223
    4444
    abcabcabc
    yangyadong@ubuntu:~/yydTestDir$ tac aaa.txt abc.txt
    4444
    1111111222223
    abcabcabc
    yangyadong@ubuntu:~/yydTestDir$
    ```

- more

  - 分屏显示文本文件内容，从前往后翻。
  - enter键往下翻。空格键一屏幕一屏幕的翻
  - b键往前翻，翻到文件的尾部，显示结束就不能往前翻了

- less

  - 和more的区别在于，当显示到文本的尾部时，还可以往前翻。

- head

  - 显示文本文件的前几行，默认显示前10行

  - ```shell
    yangyadong@ubuntu:~$ head /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    #只显示前两行
    yangyadong@ubuntu:~$ head -n 2 /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    yangyadong@ubuntu:~$
    ```

- tail

  - 显示文本文件的最后几行
  - -f 一直跟踪，不再退出。crtl+C退出
  - ![image-20201222002725386](https://i.loli.net/2020/12/22/osMBuX5EClQGPiq.png)

- vim

- nano

- cut（不改变文本本身，仅影响显示）

  - 将文件使用指定的字符切开，显示我们想要的那些列

- tr

  - 能够把文件中指定字符进行替换

  - tr命令不可以直接跟文件，需要输入重定向 <

    - ```shell
      #用空格带代替冒号进行显示
      #tr命令不可以直接跟文件，需要输入重定向 <
      yangyadong@ubuntu:~$ tr ':' ' ' < /etc/passwd
      root x 0 0 root /root /bin/bash
      daemon x 1 1 daemon /usr/sbin /usr/sbin/nologin
      bin x 2 2 bin /bin /usr/sbin/nologin
      sys x 3 3 sys /dev /usr/sbin/nologin
      sync x 4 65534 sync /bin /bin/sync
      games x 5 60 games /usr/games /usr/sbin/nologin
      man x 6 12 man /var/cache/man /usr/sbin/nologin
      lp x 7 7 lp /var/spool/lpd /usr/sbin/nologin
      mail x 8 8 mail /var/mail /usr/sbin/nologin
      yangyadong@ubuntu:~$
      ```

  - 综合实例

    - ![image-20201222114027659](https://i.loli.net/2020/12/22/yQcZXWED1hL3Irt.png)

    - ```shell
      #显示后两行
      yangyadong@ubuntu:~$ tail -2 /etc/passwd
      yangyadong:x:1000:1000:myUbuntu,,,:/home/yangyadong:/bin/bash
      sshd:x:122:65534::/run/sshd:/usr/sbin/nologin
      
      #/etc/passwd的后两行；作为cut命令的输入，用冒号做切割，并只要切割之后的第一列和第三列
      yangyadong@ubuntu:~$ tail -2 /etc/passwd | cut -d : -f 1,3
      yangyadong:1000
      sshd:122
      
      #/etc/passwd的后两行；作为cut命令的输入，用冒号做切割，并只要切割之后的第一列和第三列；
      #作为tr命令的输入，用空格来替换冒号；
      yangyadong@ubuntu:~$ tail -2 /etc/passwd | cut -d : -f 1,3 | tr ':' ' '
      yangyadong 1000
      sshd 122
      
      ##/etc/passwd的后两行；作为cut命令的输入，用冒号做切割，并只要切割之后的第一列和第三列；
      #作为tr命令的输入，用空格来替换冒号；并且输出重定向到abc.md
      yangyadong@ubuntu:~$ tail -2 /etc/passwd | cut -d : -f 1,3 | tr ':' ' ' > abc.md
      
      #看一下abc.md的内容，果然和命令行里的一模一样
      yangyadong@ubuntu:~$ cat abc.md
      yangyadong 1000
      sshd 122
      yangyadong@ubuntu:~$
      ```

- wc

  - word count 字数统计。可以统计文件的行数、单词数、字符数量（包含空格和换行符）

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ cat 111.md
    aaaa 1111
    bbbbbbbbb 2222
    ccccccccc
    ddddddddd
    #4行，6个单词，45个字符
    yangyadong@ubuntu:~/yydTestDir$ wc 111.md
     4  6 45 111.md
    yangyadong@ubuntu:~/yydTestDir$
    ```

- sort

  - 对**文本中的行**进行排序

  - ```shell
    yangyadong@ubuntu:~/yydTestDir$ cat aaa.md
    dddd 22
    aaaa 11
    cccc 22
    bbbb 11
    #默认升序
    yangyadong@ubuntu:~/yydTestDir$ sort aaa.md
    aaaa 11
    bbbb 11
    cccc 22
    dddd 22
    yangyadong@ubuntu:~/yydTestDir$
    ```

- uniq

  - unique的简写，唯一。可以针对文本中的连续的重复行进行操作。
  - ![image-20201223001442947](https://i.loli.net/2020/12/23/biu54QcRg1OXnds.png)
    - 可以看到只删去了相邻的重复行。不相邻的不管。
  - -c选项，显示重复了几次
    - ![image-20201223001724963](https://i.loli.net/2020/12/23/VbeuWj6yYidtazs.png)

- grep

  - 过滤

  - grep命令语法

    - grep [option]...'正则表达式' file...

  - ```shell
    #只显示/proc/cpuinfo中，有cpu单词出现的行
    yangyadong@ubuntu:~/yydTestDir$grep 'cpu' /proc/cpuinfo
    ```

  - -v选项，查看**没有**cpu出现的行 

    - grep -v 'cpu' /proc/cpuinfo

  - -i 不区分大小写

  - -o 只显示匹配的字符串。不显示整行。

  - -A 1。显示找到的行，及其下一行。

  - -B 3。显示找到的行，及其上三行。

  - -C 1。显示找到的行，及其上一行和下一行

  - -E。表示后面的表达式是扩展的正则表达式

  - --color=auto。将匹配的字符串标红显示。

  - Grep不仅可以对文件操作。使用管道技术，还可以筛选命令行的输出

    - ![image-20201223003620185](https://i.loli.net/2020/12/23/2Qb9R3EgYi7e1PZ.png)

- 正则表达式

  - 转义字符

    - ```
  \  将正则表达式中元字符，转换为字符本身（元字符其实有很多，底下只是列举一丢丢）
      ```
    
  - 正则表达式的元字符
  
    - ```
    .  匹配（代表）任意单个字符
      
      [] 匹配指定范围内的任意单个字符
      
      [^]匹配指定范围外的任意单个字符
      
      ```
  
  - 匹配次数
  
    - ```
      * 匹配前一个字符0次或无穷次
      （因此.*表示匹配任意长度的任意字符。因为.代表匹配任意单个字符，*表示匹配前一个字符0次或无穷次）
      
      \? 匹配前一个字符0次或1次
      
      \{m,n\} 匹配前一个字符最少m次，最多n次
      
      \{0,n\} 匹配前一个字符最少0次，最多n次
      
      \{m\} 匹配前一个字符最少m次
      
      .* 代表任意长度任意字符
      ```
  
  - 分组匹配
  
    - ```
       '91xueit\(han\)*51cto'
       grep '.*,math=\(.*\),english=\1,music=\(.*\),geography=\2' student.txt
       
       \(内容\)  就代表分组匹配，第一次出现的括号即第一组
      \1  就代表要跟第一组括号里的内容一样
      \2  即代表要跟第二组括号里的内容一样
      ```
  
  - 锚定符（即以..开头或者结尾）
  
    - ```
      锚定词首
      \<
      
      锚定词尾
      \>
      
      锚定单词
      \<内容\>   ：比如，\<yaya\>，那么yangyadong就不算匹配上了
      
      锚定行首
      ^
      
      锚定行尾
      $
      ```
  
  - 字符类
  
    - ```
      alnum 字母和数字。
      alpha 字母。
      blank 仅表示空格或制表符。
      cntrl 控制字符。
      digit 十进制数
      graph 打印字符，不包含空格。
      lower 小写字母。
      print 打印字符，包含空格。
      punct 打印字符，不包含字母和数字。
      space 空白。
      upper 大写字母。
      xdigit 十六进制数。
      
      想使用，必须得
      [[:...:]]
      比如 [[:space:]]
      ```
      
    - ```
      [] 是指一个字符
      [1-9]就是指1-9里的一个数
      ```
  
  - 字符串本身就是这些特殊符号，想表达它，就得加上转义字符\
  
    - ![image-20210101164544321](https://i.loli.net/2021/01/01/1AYRlj6kCmHVN5S.png)
  
  - 如果表达式里有命令替换，那必须得使用双引号。
  
    - 双引号是弱引用。允许命令替换
    - 单引号是强引用。不允许命令替换
    - ![image-20210101164938278](https://i.loli.net/2021/01/01/Rhscu1V3A9taHDi.png)
  
    
  
  
  
  

