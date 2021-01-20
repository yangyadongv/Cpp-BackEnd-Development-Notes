# nginx学习（openresty）

## 1.location优先级

```shell
#nginx.conf文件
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;#默认类型是下载
    

    sendfile        on;

    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;
        default_type text/html;#修改默认类型为html
        
        #第一优先级等号
        location = /a { #等号是最高优先级
            echo "=/a";
        }

        #第二优先级，以...开头
        location ^~ /a { #^~表示以...开头的，以/a开头
            echo " ^~ /a";
        }

        location ^~ /a/b {#^~表示以...开头的，以/a/b开头
            echo "^~ /a/b";
        }#同一优先级下，会按照匹配程度较高的来优先


        #第三优先级，正则表达式
        location ~ /\w { #~指正则，\w是指匹配数字字母和下划线
            echo " ~ /\w";
        }

        location ~ /[a-z] {#~指正则,匹配[a-z]字母
            echo "~ /[a-z]";
        }#同一优先级下，匹配程度也一样，会按照location顺序来判断，靠前的先执行。即 " ~ /\w"先，因为它写的考前。

        #第四优先级，默认匹配
        location / { # /默认匹配所有的请求
            echo "hello nginx";
        }
        # location / {
        #     root   html; #文件解析的目录
        #     index  index.html index.htm;#默认打开/html/index.html
        # }

        # error_page   500 502 503 504  /50x.html; #如果出现错误，打开/html/50x.html
        # location = /50x.html {
        #     root   html;
        # }
    }
}
```

- 第一优先级：等号，完全匹配

  - ```shell
    #第一优先级：等号，完全匹配
    location = /a { #等号是最高优先级
    	echo "=/a";
    }
    ```

    

  - ![image-20201226171208614](https://i.loli.net/2020/12/26/FZOHjtJ1mvwcfnR.png)

- 第二优先级：^~，^~表示以...开头的

  - ```shell
    #第二优先级：^~，^~表示以...开头的
    location ^~ /a { #^~表示以...开头的，以/a开头
    	echo " ^~ /a";
    }
    ```

  - ![image-20201226171331039](https://i.loli.net/2020/12/26/DMg9NimrBLfJlvY.png)

  - #同一优先级下，会按照匹配程度较高的来优先

  - ```shell
    #第二优先级，以...开头
    location ^~ /a { #^~表示以...开头的，以/a开头
    	echo " ^~ /a";
    }
    
    location ^~ /a/b {#^~表示以...开头的，以/a/b开头
    	echo "^~ /a/b";
    }#同一优先级下，会按照匹配程度较高的来优先
    ```

  - ![image-20201226171615409](https://i.loli.net/2020/12/26/7p9rFOoEYuUti41.png)

-  第三优先级，正则表达式

  - ```shell
    #第三优先级，正则表达式
    location ~ /\w { #~指正则，\w是指匹配数字字母和下划线
    	echo " ~ /\w";
    }
    
    location ~ /[a-z] {#~指正则,匹配[a-z]字母
    	echo "~ /[a-z]";
    }#同一优先级下，匹配程度也一样，会按照location顺序来判断，靠前的先执行。即 " ~ /\w"先，因为它写的靠前。
    ```

  - ![image-20201226171653590](https://i.loli.net/2020/12/26/8onI3j74GiUHYaz.png)

  - ```shell
    #第三优先级，正则表达式
    location ~ /[a-z] {#~指正则,匹配[a-z]字母
    	echo "~ /[a-z]";
    }
    
    location ~ /\w { #~指正则，\w是指匹配数字字母和下划线
    	echo " ~ /\w";
    }#同一优先级下，匹配程度也一样，会按照location顺序来判断，靠前的先执行。即 " ~ /[a-z]"先，因为它写的靠前。
    ```

  - ![image-20201226171833150](https://i.loli.net/2020/12/26/MvKQqyhVZUoeASD.png)

- 第四优先级，默认匹配

  - ```shell
    location / { # /默认匹配所有的请求
    	echo "hello nginx";
    }
    ```

    

  - ![image-20201226172035456](https://i.loli.net/2020/12/26/sM1rYcg8N9HeKTE.png)

  - ![image-20201226172117127](https://i.loli.net/2020/12/26/pAbeEy1hYWtRjig.png)

## 2.反向代理

```shell
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;#默认类型是下载
    

    sendfile        on;

    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;
        default_type text/html;#修改默认类型为html

        location /{
            proxy_pass http://192.168.80.131:80;            
        }
        location =/b{
            proxy_pass http://192.168.80.131/index2.html;
        }
    }
}
```

- ```
    location /{
              proxy_pass http://192.168.80.131:80;            
          }
  ```

  - ![image-20201226201223771](https://i.loli.net/2020/12/26/1XpBMTciKU3kn5e.png)

-         location =/b{
              proxy_pass http://192.168.80.131/index2.html;
          }

  - ![image-20201226201237709](https://i.loli.net/2020/12/26/E7LqFXzUhuv56t4.png)

## 3.负载均衡

```shell
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;#默认类型是下载
    

    sendfile        on;

    keepalive_timeout  65;
    upstream group1{
        server 192.168.0.12:80 weight=1;
        server 192.168.0.12:81 weight=1;
    }
    
    server {
        listen       80;
        server_name  localhost;
        default_type text/html;#修改默认类型为html

        location /{
            proxy_pass htttp://group1/;            
        }
    }
}
```

