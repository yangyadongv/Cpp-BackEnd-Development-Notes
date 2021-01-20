# docker

## 1.概念

![image-20201226204237952](https://i.loli.net/2020/12/26/Ine9NwiqLRSjt3E.png)

- 镜像
  - 类似于iso文件
- 容器
  - 正在运行的虚拟机
- tar文件
  - 类似于vmware的vmdk文件，可以将一个镜像直接save成tar文件，方便给别人
  - 别人可以使用一个load指令，重新把tar文件加载成一个镜像
  - 再通过run指令把镜像Run起来，即容器
- dockerfile
  - dockerfile类似于一个配置文件
  - 这个配置文件里，通过写“如何构建”的步骤，来指定一个镜像是如何构建的。
  - 通过docker build可以将dockerfile构建成一个镜像
- 远程仓库
  - 保存了很多镜像，包括ubuntu镜像、nginx镜像、mysql镜像、tomcat镜像等等
  - 可以通过docker pull指令把镜像下载到本地。
  - 也可以通过docker push把自己的镜像上传到远程仓库

## 2.docker命令

### docker pull拉取镜像

- ```shell
  #拉取nginx镜像
  yangyadong@ubuntu:~$ sudo docker pull nginx
  ```

  - ![image-20201226210457460](https://i.loli.net/2020/12/26/ngBzWXZK3xtRV6J.png)

### docker run，把镜像变成容器

- ```shell
  #docker run把镜像run起来，变成容器
  #-d 让它后台运行，不要阻塞shell指令窗口
  #-p指定内外端口映射 外部80:内部80
  ```

  - ![image-20201226210753127](https://i.loli.net/2020/12/26/Fcv5WTRe1ydHlow.png)

### docker exec进入容器命令行

- ```shell
  #docker exec 进入容器命令行
  #-i :即使没有附加也保持STDIN 打开 
  #-t :分配一个伪终端
  #c6c6a是容器ID
  yangyadong@ubuntu:~$ docker exec -it c6c6a bash
  #进入目录/usr/share/nginx/html
  #docker版的nginx默认index页面是在/usr/share/nginx/html/index.html。与linux版的默认路径/var/www/html稍有不同。根本原因是默认的配置文件/etc/nginx/nginx.conf里的定义不同
  root@c6c6a3a559f1:/# cd /usr/share/nginx/html
  root@c6c6a3a559f1:/usr/share/nginx/html# ls
  50x.html  index.html
  root@c6c6a3a559f1:/usr/share/nginx/html# echo 'DockerTest123' > index.html
  root@c6c6a3a559f1:/usr/share/nginx/html# cat index.html
  DockerTest123
  root@c6c6a3a559f1:/usr/share/nginx/html#
  ```

  - ![image-20201226215529912](https://i.loli.net/2020/12/26/VBZhFjaCAMEWI9Y.png)
  - 可以看到，修改index.html成功了
    - ![image-20201226215631945](https://i.loli.net/2020/12/26/X9HARPVYhoO8kdF.png)

### docker ps查看正在运行的容器

### docker rm删除容器

- ```shell
  yangyadong@ubuntu:~$ docker ps
  \CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                NAMES
  66691874378c   wordpress   "docker-entrypoint.s…"   4 seconds ago    Up 3 seconds    80/tcp               festive_leavitt
  c6c6a3a559f1   nginx       "/docker-entrypoint.…"   58 minutes ago   Up 58 minutes   0.0.0.0:80->80/tcp   condescending_nash
  #使用docker rm -f删除容器
  yangyadong@ubuntu:~$ docker rm -f 66691
  66691
  ```

  

  - ![image-20201226220638859](https://i.loli.net/2020/12/26/s3kF864ECWqKIh9.png)

### docker commit将容器变成镜像

- ```shell
  #将docker容器打包成镜像
  yangyadong@ubuntu:~$ docker commit c6c6 yydmodifiednginx
  ```

  - ![image-20201226221729832](https://i.loli.net/2020/12/26/t9NYdx4BOloPFeW.png)

- 把这个新打出来的镜像用90端口run起来，变成容器

  - ![image-20201226222105098](https://i.loli.net/2020/12/26/9LB6OhqKHaxs7vz.png)
  - ![image-20201226222137305](https://i.loli.net/2020/12/26/1gukvKdtibLXWI4.png)
  - 可以看到，确实是我们修改以后的容器的内容，不再是原版的nginx了。**这说明我们修改过后的容器保存成镜像，重新运行，它可以保持这个变化**

### dockerfile配置文件来制作镜像

- 比如编写一个dockerfile

  - ```shell
    #dockerfile文件
    FROM nginx  #第一步先添加上完整的nginx
    ADD ./index.html /usr/share/nginx/html/  #第二步把主机与dockerfile路径下index.html文件拷贝到镜像中的文件夹/usr/share/nginx/html/下
    ```

- 外部的index.html是

  - ```
    wai bu de  wen jian
    ```

- ![image-20201226223918526](https://i.loli.net/2020/12/26/opQMAcPjCOT8heN.png)

- 然后，把这个镜像run起来变成容器

  - ![image-20201226224141058](https://i.loli.net/2020/12/26/SvUDRPTg7CzfOWE.png)
  - ![image-20201226224032722](https://i.loli.net/2020/12/26/Jy5WTnkhrBs8QMp.png)
  - 可以看到，配置很成功。

### docker save将镜像变成tar文件

### docker load从tar文件中加载出镜像

- ```
  docker save yydmodifiednginx > 1.tar #将镜像保存成tar文件
  docker load < 1.tar #从tar文件恢复出镜像
  ```

- ![image-20201226225042884](https://i.loli.net/2020/12/26/fY15Wri69oXHkg4.png)

### 重点是run命令

- 一般主要是使用docker run去把一些镜像run成容器

- ```shell
  docker run -d -p 80:80 --name mynginx -v 'pwd':/usr/share/nginx/html nginx:1.13
  ```

  

  - -d ：后台运行，不要阻塞命令行
  - -p ： 外部端口：容器内部端口
  - --name：容器的名字
    - 不指定的话会随机一个名字，随便的英文单词拼凑成的
  - -v：映射文件
    - 把外部文件夹 与 容器内部的文件夹映射起来。
    - 这样的话，就可以将一些静态文件放在外面，在外边修改文件，里边文件就会跟着变化（因为是映射的）
    - 映射还可以用于数据保存。比如mysql的data目录也可以映射到外面，防止数据丢失
  - 最后的参数就是镜像的名字，如果有具体版本最好不要省略，省略就是latest最新版

## 3.docker容器编排

### 3.1docker多容器的网络结构

![image-20210101205256794](https://i.loli.net/2021/01/01/XO3xr8qcLztS9yB.png)



### 3.2 --link 选项 实现容器ip映射

![image-20210101215606470](https://i.loli.net/2021/01/01/GBRfZv2HgzCd4WF.png)

![image-20210101215647238](https://i.loli.net/2021/01/01/qNUL7xdSgOvYMsX.png)

### 3.3使用docker compose实现多容器的按序构造、Link等等

- 问：docker-compose.yml里，没有写link啊，这三个容器之间，是怎么知道对方的存在呢？

  - 答：docker-compose默认把这件事做了，在每个容器hosts文件中都写好了ip与service名的对应

- ![image-20210101223807077](https://i.loli.net/2021/01/01/r2PvxyjZzqA4gcK.png)

- conf文件夹

  - nginx.conf  //nginx配置文件

    - ```
      worker_processes  1;
      
      events {
          worker_connections  1024;
      }
      http {                                                  
          include       mime.types;
          default_type  application/octet-stream; #默认类型是下载
      
          sendfile        on;
      
          keepalive_timeout  65;
      
          server {
              listen       80;
              server_name  localhost;
              default_type text/html; #修改默认类型为html
      
              location / {                     #默认匹配方式
                  root   /usr/share/nginx/html;
                  index  index.html index.htm;
              }
      
              error_page   500 502 503 504  /50x.html;   #如果出现错误，打开/usr/share/nginx/html/50x.html
              location = /50x.html {
                  root   /usr/share/nginx/html;
              }
      
              location ~ \.php$ {                     #~指正则， 区分大小写的正则表达式匹配；所有.php结尾的请求，都转到这儿
                  fastcgi_pass    php:9000;
                  fastcgi_index   index.php;
                  fastcgi_param   SCRIPT_FILENAME     /var/www/html/$fastcgi_script_name;    
                  include         fastcgi_params;
              }
          }
      }
      ```

- html文件夹

  - index.html  //nginx首页

    - ```
      hello docker-compose 123!
      ```

  - test.php //用于测试php配置成功与否，展示phpinfo()

    - ```
      <?php
      phpinfo();
      ?>
      ```

  - mysql.php//用于测试php配置成功与否，并且访问mysql，判断Mysql配置成功与否

    - ```
      <?php
      $dbhost = 'mysql'; 
      #本来应该是localhost的，但是在docker-compose中将mysql这个服务映射过来了，所以域名就是mysql.
      #即用mysql这个域名，就能解析到这个容器的ip
      $dbuser = 'root';
      $dbpass = '123456';
      $conn = mysqli_connect($dbhost, $dbuser, $dbpass);
      if (!$conn) {
      	die('could not connect! ' . mysqi_error());
      }
      echo 'connect success! ';
      mysqli_close($conn);
      
      ?>
      ```

- docker-compose.yml  主配置文件

  - ```
    version: "3"   #版本
    services:
      nginx:       
        image: nginx:alpine  #指定nginx镜像版本
        ports:
        	- 80:80  #端口映射
        volumes:
        	- ./html:/usr/share/nginx/html   #文件夹映射
        	- ./conf/nginx.conf:/etc/nginx/nginx.conf  #文件映射
      php:
        image: devilbox/php-fpm:8.0-work-0.106   #指定php镜像版本
        volumes:
        	- ./html:/var/www/html         #文件夹映射
      mysql:
        image: mysql:5.6             #指定mysql版本
        environment:
        	- MYSQL_ROOT_PASSWORD=123456   #设置环境变量 MYSQL_ROOT_PASSWORD，这是mysql容器启动必须配置的参数
    ```

### 3.4分析一下流程

- 1.访问默认ip。192.168.80.131

  - 先到nginx。

  - 由于nginx.conf配置的时候，

    - ```
          location / {                     #默认匹配方式
              root   /usr/share/nginx/html;
              index  index.html index.htm;
          }
      ```

  - 所以就会去访问容器内部的 /usr/share/nginx/html/index.html文件

  - 由于docker-compose.yml 中

    - ```
        nginx:       
          image: nginx:alpine  #指定nginx镜像版本
          ports:
          	- 80:80  #端口映射
          volumes:
          	- ./html:/usr/share/nginx/html   #文件夹映射
          	- ./conf/nginx.conf:/etc/nginx/nginx.conf  #文件映射
      ```

    - 将容器内部的/usr/share/nginx/html文件夹与外部文件夹./html所绑定。

  - 因此容器内部的 /usr/share/nginx/html/index.html文件 其实 就是 外部的 ./html/index.html文件

  - 容器内部的/etc/nginx/nginx.conf配置文件  其实 就是 外部的 ./conf/nginx.conf配置文件

- 2.访问 192.168.80.131/test.php

  - 先到nginx

  - 由于nginx.conf配置的时候，

    - ```
      location ~ \.php$ {                     #~指正则， 区分大小写的正则表达式匹配；所有.php结尾的请求，都转到这儿
          fastcgi_pass    php:9000;
          fastcgi_index   index.php;
          fastcgi_param   SCRIPT_FILENAME     /var/www/html/$fastcgi_script_name;    
          include         fastcgi_params;
      }
      ```

  - 所以就会去访问容器内部的  /var/www/html/test.php

  - 由于docker-compose.yml 中

    - ```
        php:
          image: devilbox/php-fpm:8.0-work-0.106   #指定php镜像版本
          volumes:
          	- ./html:/var/www/html         #文件夹映射
      ```

    - 将容器内部的/var/www/html 文件夹 与 容器外部的 ./html 文件夹所映射绑定

  - 因此容器内部的/var/www/html/test.php文件 其实 就是 外部的 ./html/test.php文件























