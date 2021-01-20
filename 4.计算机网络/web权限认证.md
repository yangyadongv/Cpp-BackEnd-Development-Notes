# HTTP  Auth

## authentication与authorization

- authentication：认证，指未登录用户的认证。
  - 401，"Unauthenticated"。账号密码输错了，登录身份认证失败；或者是没有登录。
- authorization：授权，指登录用户可以授权执行哪些操作。
  - 403，权限不足。登陆成功，权限不足。

## http是无状态的。第一次请求中进行了登录，之后的请求怎么知道你是否已经登录。

### 1.基于session方式认证方式（session ID即cookie）

- session存在server端，是一个key-value的存储结构，存放在内存里。
- 客户端第一次请求进行登录，登录完成后，服务端会记下来一个session ID xxx及其对应的value {uid....}。value里可能有uid等等等等。
- 服务端把这个session ID xxx（相当于是cookie）给客户端返回过去。
  - session ID相当于是cookie，也相当于是key
- 等下次客户端再发起请求时，会把这个session ID xxx携带在cookie中。
- 然后服务端就根据这个xxx去找有没有这个session ID。然后确实找到了，并且还知道了它对应的value {uid...}，这样就知道user ID等用户的数据信息。既然找到了这个用户是谁，那就相当于对这个用户完成了身份认证。
- 重点：
  - cookie绝对不能被别人知道。因为cookie就是用来身份认证的。
  - 把cookie传给服务端，服务端去查询，就能找到对应的value包括user ID等，就完成了身份认证。

### 2.基于authentication的HTTP header的认证方式

#### 2.1 basic形式

- 每次请求都携带用户名密码，直接写在请求头里。https保证了这种方式的安全性。
- 其实是（用户名：密码）base64编码一下。
- 优点：
  - 不怕nginx反向代理负载均衡。因为每次都要验证用户名密码。
  - 而基于cookie的那种方式，nginx反向代理负载均衡后，如果没有让所有负载均衡的服务器都能查到那个cookie的话，是会有问题的。因为你这次访问时，是服务器A记录了你的sessionID，并返回给你cookie。下次访问可能就被负载均衡到服务器B去了，多台均衡服务器之间，必须得共享这些记录的cookie资源。
- 缺点：
  - 很明显，每次请求都要认证去查数据库。对数据库压力很大。

#### 2.2bearer形式

- 接的是一个令牌token。
- 用户第一次登陆请求，把用户名密码发了上来，认证成功了。
- 很可能就是采用某种对称加密算法，比如AES-GCM，把用户的所有信息比如UID等等所有信息拼接在一起然后加密，即用户的所有信息拼接在一起被服务器自己对称加密就叫做token了。
- 把token发给客户端。
- 客户端在之后的请求中，把token携带在authentication的HTTP header中，传到服务端。服务端再把token解密，就得到了所有用户信息，就完成了用户认证。

## 总结

- 基于session的方式（基于cookie的方式）
  - 依赖于session的key-value的存储。
  - 主要缺点：
    - session是存在当前服务器的，在分布式服务器的情况下，就需要特殊的策略进行session存储的共享。
    - 客户端的cookie，记录的就是服务端的session ID。
      - ![image-20210119163039393](http://pichost.yangyadong.site/img/image-20210119163039393.png)
    - **客户端存的，那叫cookie；服务端存的，那叫session ID。**
    - session ID就是key，可以查它对应的value。value里就是一个用户的所有信息。
- basic的方式
  - 每次请求都携带上账号密码
  - 主要缺点
    - 每次都要重新访问数据库，进行用户的认证，性能上会差一些
- bearer方式（基于token的方式）
  - 服务端对用户信息进行对称加密，然后把token返回给客户端。服务器本身可以不存储。
  - cookie必须是在服务端有存储的。
  - 典型代表
    - JWT：Json web token

# cookie/session/localstorage/sessionstorage

【前端】web中的k-v存储们有什么区别？

- 跨页：跨标签页
- 跨域：跨域名

![image-20210119161725370](http://pichost.yangyadong.site/img/image-20210119161725370.png)

- cookie
  - 随请求头，每次提交
- localstorage（html5中的新概念）
  - 不随请求头提交，可长时间保存
- sessionstorage
  - 不随请求头提交。标签页关闭即失效。
  - 不支持跨标签页
- session
  - 存储在服务端
  - 服务器通过客户端请求头中的cookie去查找，找到服务器上存储的该cookie对应的内容。
  - session ID相当于是cookie，也相当于是key，去查找对应value。
  - 换句话说，下图一目了然
    - ![image-20210119163039393](http://pichost.yangyadong.site/img/image-20210119163039393.png)
    - 客户端的cookie，记录的就是服务端的session ID。
    - **客户端存的，那叫cookie；服务端存的，那叫session ID。**

