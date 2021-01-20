## 1.ssr和v2ray都是sock5代理

- ssr的shadowsocks或者v2ray的Vmess只是一种协议。他们本质上都是sock5代理。
- sock5代理工作在会话层，http代理是应用层，sock5代理层级更低。
- 理论上sock5可以代理所有的上层应用

## 2.代理服务器

- 代理服务器是位于计算机和互联网之间的服务器，在连接上代理服务器之后，它会隐藏你的IP地址，因此当你访问某个网站的时候，它看到的是代理服务器的IP地址，而不是你的真实IP地址。
- 代理故名思意，其实就是将帮忙转发你的数据，利用这点特性，就可以用来绕过防火长城。比如，当你直接访问Google的时候，肯定会被墙，但是你可以先连接到代理，代理帮你访问Google，然后再把数据返回给你，这样就实现了翻墙。
- 还需要了解的—点是，代理通常只能针对某个应用程序使用。比如常用的就是将浏览器配合代理使用，来浏览国外的一些网站。也就是说，代理不是系统级别的，任何应用程序需要翻墙，都需要进行配置，具体来说就是需要配置好本地的代理端口号。
- shadowsocksR和v2rayN都会在本地起个服务。
  - 例如：shadowsocksR占用1080端口。
    - ![image-20210118150301164](http://pichost.yangyadong.site/img/image-20210118150301164.png)
    - ![image-20210118150454981](http://pichost.yangyadong.site/img/image-20210118150454981.png)
  - 所以流程是
    - 浏览器->本地代理服务->代理服务器->国外网站

## 3.各种方式的区别

- 全局模式
  - ![image-20210118150606522](http://pichost.yangyadong.site/img/image-20210118150606522.png)
- PAC模式
  - ![image-20210118150643418](http://pichost.yangyadong.site/img/image-20210118150643418.png)
- 所以，本质是修改了系统代理模式。
- chrome浏览器默认跟随系统代理，所以就可以科学上网。
- qq、微信、网易云音乐等默认都不跟随系统代理。所以就不走代理流量。
- 根据我的实践经验，typora是跟随系统代理的。所以会走代理流量。

## 4.如何实现真正全局代理，所有应用走代理

- 使用Proxifier+SSR或者Proxifier+v2rayN都可以
- Proxifier可以把所有流量导入SSR起的本地服务127.0.0.1:1080端口。
- 把qq、微信、音乐软件、游戏、电报等等一切客户端都可以添加到Proxifier中，这样就可以实现sock5代理的功能了。
- 如果不用Proxifier，那么SSR只能修改系统代理，很多应用都不跟随系统代理，因此还是走的原流量。

## 5.v2rayN比ssr更先进一点

![image-20210118151826198](http://pichost.yangyadong.site/img/image-20210118151826198.png)

- v2rayN有个一直打开的sock5代理服务
  - 这个sock5属于会话层代理。
  - 它不会修改系统代理配置。
  - 要么设置软件自己可以走这个代理
    - ![image-20210118152136056](http://pichost.yangyadong.site/img/image-20210118152136056.png)
  - 要么配合Proxifier，将应用程序的流量导入这个sock5代理服务中。
  - 要不然，你是没法使用这个sock5代理的
- 还有个专用的http服务
  - 这个专用的http代理服务，就真的只能代理http流量。属于应用层代理。
  - 跟sock5区分开，它不能代理所有应用层流量，只能代理http流量。
- 以及专用的PAC服务
  - **Proxy Automatic Configuration**，代理自动配置
  - 根据脚本智能分流。
  - 会修改系统代理配置。但是还是老弊端，大部分应用程序，不走系统代理。只有绝大多数浏览器才走系统代理。
- v2rayN会开3个本地服务，占用3个端口。
- ![image-20210118151826198](http://pichost.yangyadong.site/img/image-20210118151826198.png)
- 我把HTTP代理关了，它也会有个sock5代理一直开启。
  - ![image-20210118151957790](http://pichost.yangyadong.site/img/image-20210118151957790.png)

## 6.再总结一遍

- 虽然SSR和v2ray的原理是修改系统代理配置，但是不代表所有应用程序都会走系统代理。
- chrome浏览器默认跟随系统代理，以及typora我感觉是走的系统代理
- qq、微信、网易云音乐等默认都不跟随系统代理。所以就不走代理流量。
- 想让这些应用程序也走代理？
  - 要么应用程序本身支持设置
    - ![image-20210118152923574](http://pichost.yangyadong.site/img/image-20210118152923574.png)
    - ![image-20210118152953521](http://pichost.yangyadong.site/img/image-20210118152953521.png)
  - 要么使用Proxifier，将应用程序的流量导入这个sock5代理服务中。
- ssr只会起一个1080端口的sock5代理服务
  - 所以ssr必定修改系统代理配置
- v2ray比较高级，10808端口是sock5代理服务，10809端口htttp代理服务，10810端口是PAC代理服务。
  - 只开sock5代理，那么不修改系统代理配置
  - 开http代理，那么会修改系统代理配置
    - 修改手动设置代理那里
  - 开PAC代理，也会修系统改代理配置。
    - 修改自动设置代理那里
- 应用走了sock5代理流量，才会显示是异地登录
  - ![image-20210118154313170](http://pichost.yangyadong.site/img/image-20210118154313170.png)

