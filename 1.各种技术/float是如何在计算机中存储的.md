# float是如何在计算机中存储的

## 1.整数类型

### 整数类型的存储

整数以补码的形式存放在计算机中。

- 正数的补码，还是本身
- 负数的补码，除了首位为1，其余位取反，再加1。

![image-20210226150155947](http://pichost.yangyadong.site/img/image-20210226150155947.png)

### 整数的最大值和最小值

![image-20210226151829469](http://pichost.yangyadong.site/img/image-20210226151829469.png)

- 最大：2^31-1，即0x7F FF FF FF
- 最小：-2^32，  即0x80 00 00 00

![image-20210226152850116](http://pichost.yangyadong.site/img/image-20210226152850116.png)

## 2.浮点数

![image-20210226162357445](http://pichost.yangyadong.site/img/image-20210226162357445.png)

![image-20210226162601643](http://pichost.yangyadong.site/img/image-20210226162601643.png)

## 3.

