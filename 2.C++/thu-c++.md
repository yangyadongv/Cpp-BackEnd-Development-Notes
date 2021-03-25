##  第1章 绪论

### 1.计算机系统简介

#### **计算程序语言**

-   计算机解决问题是程序控制的；
-   程序就是操作步骤；

-   程序要使用语言来表达。


#### 机器语言

-  计算机能识别的是机器语言；

-  机器语言指令是由0和1编码的；

-  例如：

  ​	加法指令可能是“0001”。

#### 计算机指令系统

-   机器硬件能够识别的语言（机器语言）的集合；

-   它是软件和硬件的主要界面。

  （即所有硬件设备可以识别指令。而软件则将会被转换为指令）

#### 计算机软件

-  是一系列按照特定顺序组织的计算机数据和指令的集合。一般来讲软件被划分为**系统软件** （*计算机系统的管家，管理所有计算机资源和任务等*）、**应用软件**、介于这两者之间的**中间件；**（*大型软件开发需要*）
-  软件包括**程序**和**文档**(*说明文件*)。


#### 计算机程序

-   指令的序列；
-   描述解决问题的方法和数据。

### 2.计算机语言和程序设计方法的发展

#### 最初的计算机语言——机器语言

-   由二进制代码构成
-   **计算机硬件可以识别**

-   可以表示简单的操作

-   例如：加法、减法、数据移动等等


#### 汇编语言

-   将机器指令（二进制代码）映射为助记符

  ​	  如`ADD、SUB、mov`等；

-   抽象层次低，需要考虑机器细节。


#### 高级语言

-   关键字、语句容易理解；

-   有含义的数据命名和算式；

-   抽象层次较高；

  ​	  例如，算式：a+b+c/d

-   屏蔽了机器的细节；*（我不需要知道显示器端口，就可以显示）*

-  例如，这样显示计算结果：`cout<<a+b+c/d`


#### C++语言

-   是高级语言

-   支持面向对象的观点和方法。

    将客观事物看做对象;（*每个对象都有属性、行为能力等）*

    对象间通过消息传送进行沟通;

    支持分类和抽象。（*同类型对象看做一类）*

#### 面向过程的程序设计方法：

-   机器语言、汇编语言、高级语言都支持；
-   最初的目的：用于数学计算；

-   主要工作：设计求解问题的过程。

-   大型复杂的软件难以用面向过程的方式编写


#### 面向对象的程序设计方法：

-   由面向对象的高级语言支持；
-   一个系统由对象构成；

-   对象之间通过消息进行通信。

### 3.面向对象的基本概念

#### 对象

-   一般意义上的对象：现实世界中实际存在的事物。
-   面向对象方法中的对象：程序中用来描述客观事物的实体。


#### 抽象与分类

-   分类依据的原则——抽象；
-   **抽象出同一类对象的共同属性和行为形成类；**

-   **类与对象是类型与实例的关系。**（模子与铸件）


#### 封装

-   隐蔽对象的内部细节；
-   对外形成一个边界；

-   只保留有限的对外接口；

-   使用方便、安全性好。


#### 继承

-   意义在于软件复用；
-   改造、扩展已有类形成新的类。


#### 多态

-   同样的消息作用在不同对象上，可以引起不同的行为。

### 4.程序的开发过程

#### 程序

-   **源程序：**

    用源语言写的，有待翻译的程序；

-   **目标程序：（二进制代码）**

    源程序通过翻译程序加工以后生成的机器语言程序；

-   **可执行程序：（exe文件）**

    **连接目标程序以及库中的某些文件，生成的一个可执行文件；**

  **（比如：`cout`函数，它的源码我可没写，是别人写好的。把我的代码与 库里这些别人写好的且已经编译过的这些函数 连接起来。）**

    例如：Windows系统平台上的.EXE文件。


#### 三种不同类型的翻译程序

-   **汇编程序：**

    **将汇编语言源程序翻译成目标程序；（把`mov add`这些汇编语言翻译成二进制的机器代码）**

-   **编译程序：**

    **将高级语言源程序翻译成目标程序；（把`int a=0`；等这些高级语句翻译成二进制的机器代码。一次翻译好后，以后执行都不要翻译了）**

-   **解释程序：**

    **将高级语言源程序翻译成机器指令，边翻译边执行。**

  **（翻译一条执行一条，代码执行过一遍了，下一次再执行时，也要重新翻译）**

  **例如：**

  **python语言就是解释型语言。**

  **java语言是半编译半解释型语言（java源代码会编译成一种中间的二进制码，所有操作系统上的java虚拟机都会认得这种二进制码，然后每个操作系统上的不同java虚拟机再将这种二进制码解释成本地操作系统可以识别的这些二进制机器语言代码）。**

  **C++程序是直接编译为本地机器语言代码。所以Python和java可以跨平台，而c++不可以**

#### C++程序的开发过程

-   算法与数据结构设计；
-   源程序编辑；

-   编译；**（不做计算，只做语法检查，语法正确才可以编译。）**

-   连接；**（把调用的程序库里的模块跟我写的代码合在一起，形成可执行文件）**

-   测试；（大量的数据进行测试，测试是否有结果不对的数据）

-   调试。（有出现错误数据，然后进行debug）

### 5.计算机中的信息与存储单位

#### 计算机的基本功能

-   算术运算；
-   逻辑运算。


#### 计算机中信息：

-   控制信息——指挥计算机操作；（各种指令mov add等等）
-   数据信息——计算机程序加工的对象。


![image.png](https://i.loli.net/2020/01/13/z4n8sKifBjALmGR.png)

 

#### 信息的存储单位

-   位(bit，b)：数据的最小单位，表示一位二进制信息；
-   字节(byte，B)：八位二进制数字组成(1 byte = 8 bit)；

-   千字节    1 KB = 1024 B；

-   兆字节    1 MB = 1024 K；

-   吉字节    1 GB = 1024 M。

### 6.计算机的数字系统

这章主要讲各种进制之间的转换

### 7.数据的编码表示

#### 二进制数的编码表示

原码、反码、补码

| 想表示的二进制数 | +1100110 | -1100110 | 规则：正整数不管，负整数以下规则 |
| ---------------- | -------- | -------- | -------------------------------- |
| 原码             | 01100110 | 11100110 |                                  |
| 反码             | 01100110 | 10011001 | 除了第一位，其他取反             |
| 补码             | 01100110 | 10011010 | 反码末位+1                       |

```
   补码           十进制数
0111 1111           127
0111 1110           126
...                 ...
0000 0000            0

1111 1111           -1
1111 1110           -2
...                 ...
1000 0001          -127
1000 0000          -128
```

```c++
左移右移
左移（扩大）：向左进行移位操作，高位丢弃，低位补0。
8位有符号数的表示范围：[-128,127]
例如：
    0000 0110=6，左移1位，变成 0000 1100=12，扩大了一倍√
    0111 1111=127，左移1位，变成 1111 1110=-2？结果没有扩大一倍，原因是127*2=254超过8位有符号数的表示范围了，那当然会出错。×
    1111 1111=-1,左移1位，变成1111 1110=-2，扩大一倍√
    1000 0000=-128,左移1位，变成0000 0000=0，结果错误，因为-128*2=-256，也超过8位有符号数的表示范围了，那当然会出错×
    
右移（缩小）：向右进行移位操作，对无符号数，高位补0，对于有符号数，高位补符号位。
例如：
    0000 0110=6,右移1位，变成0000 0011=3，缩小一倍√
    0000 0001=1，右移1位，变成0000 0000=0，1/2=0，是对的，√
    1111 1111=-1，右移1位，变成1111 1111=-1，
    1000 0000=-128，右移1位，变成1100 0000=-64对的√
```

左移：有符号和无符号数是一样的操作。左移数字变大，会有溢出的情况，导致结果与设想差别很大。

右移：有符号和无符号操作不一样。右移数字变小，不会有溢出，因为数字越移越小，肯定不会溢出。

#### 实数的浮点表示

定点方案：定义好小数点在第多少位，现在被淘汰了。

浮点方案：

**实数 N 用浮点形式可表示为： N=M×2^E**

**E：实数N的阶码，E的位数反映浮点数所表示的数据范围；**

**M：实数N的尾数，M的位数反映数据的精度。**

比如：float，32位，E=8位，[-127,128]  M=23位。

**举例：**

![t8t8ne.png](https://s1.ax1x.com/2020/06/01/t8t8ne.png)

**float表示最大值：**

浮点类型的单精度值具有 4 个字节，包括一个符号位、一个 8 位 二进制指数和一个 23 位尾数。8位在unsigned char里是[0,255]，在这里都要减个偏置127，即[-127,128]。所以表示的最大值是-2^128~2^128，即-3.4E+38 和 3.4E+38 之间的范围。

**float精度：**

float的精度是由尾数的位数决定的。

float：2^23=8388608，一共七位，所以一共最多七位有效数字，但绝对能保证的为6位。故：**float的精度为十进制中的6~7位有效数字。也就是说是指 整数部分 和小数部分一共7位**

![tGD85F.png](https://s1.ax1x.com/2020/06/01/tGD85F.png)

![tGDIIS.png](https://s1.ax1x.com/2020/06/01/tGDIIS.png)

注意：

1. 在默认情况下以%f格式输出的情况下会输出6位小数，但并不能保证这6位小数有效，即：是否有效还要看整数位和小数位加在一起是否超过7位。
2. 另外我们要知道：**有效数字的位数与指定输出的小数位数（%.7f）是两码事**。%.mf 格式是自己设置需要输出几位小数。
   1. ![tGrNFS.png](https://s1.ax1x.com/2020/06/01/tGrNFS.png)

```c++
#include<iostream>
using namespace std;
int main()
{
	float a=9.876543E+7f;
	float b=a+1.0f;
	cout<<"a="<<a<<endl;
	cout<<"b-a="<<b-a<<endl;
	return 0;
}
//输出：
//a=9.87654e+07
//b-a=0
//9.876543E+7f是一个小数点左边有8位有效数字，加上1，就是在第8位加1。
//但是float类型只能表示数字中前6位或者前7位，因此修改第8位对这个值不会有任何影响。
```

**float和double对比**

- float
  - 4byte=32位=1bit符号位+8bit指数位+23bit尾数位
  - 精度：保证10进制中6~7位有效数字（整数部分+小数部分一共7位）
  - 表示范围：绝对值`3.4*10^-38~3.4*10^38`
- double
  - 8byte=64位=1bit符号位+11bit指数位+52bit尾数位
  - 精度：保证10进制中15~16位有效数字（整数部分+小数部分一共16位）
  - 表示范围：绝对值`1.7*10^-308~1.7*10^308`

#### 字符在计算机中的表示

-  字符在计算机中是通过编码表示的；

-  例如：

  ASCII码是一种常用的西文字符编码：用7位二进制数表示一个字符，最多可以表示2的7次方=128个字符；

- 《GB 18030-2005 信息技术 中文编码字符集》是中国国家标准。

## 第2章  c++语言概述

### 1.`cout`  `cin`

- 在C++中，将数据从一个对象到另一个对象的流动抽象为“流”。流在使用前要被建立，使用后要被删除。

- `cout`是输出流类的一个对象。`cout`用来处理标准输出，即屏幕输出。

  - “<<" 是流插入运算符。
  - 向显示器送信息的流通道默认是打开的，向这个流通道送数据的就是`cout`这个对象，送数据这个动作是通过"<<"这个插入运算符实现的。

- `cin`是输入流类的一个对象。从流中获取数据的操作称为提取操作。`cin`用来处理标准输入，即键盘输入。

  - ">>"是提取符。
  - 提取符可以连续写多个，每个后面跟一个表达式，该表达式通常是用于存放输入值的变量。例如：
  -  `int a, b;`
  -  `cin >> a >> b;`

  ![image.png](https://i.loli.net/2020/01/14/6VA9vhOaJEXCgs3.png)

  结果是：" 3.14"    因为一共setw了5个位置长度，setprecision 3，加上小数点一共3位小数。所以3、小数点、1、4 一共只有4个位置长度，前边补了个空格凑到5五个长度。

  ### 2.词法记号

-  关键字：      C++预定义的单词

-  标识符：      程序员声明的单词，它命名程序正文中的一些实体

-  文字：  在程序中直接使用符号表示的数据

-  分隔符：

   ()  {}  ,  :  ;  用于分隔各个词法记号或程序正文

-  运算符：（操作符）  用于实现各种运算的符号

-  空白符：      空格、制表符（TAB键产生的字符）、垂直制表符、换行符、回车符和注释的总称

  程序员只能定义上述中的标志符：

### 3.标识符的构成规则

-  以大写字母、小写字母或下划线(_)开始。
-  可以由以大写字母、小写字母、下划线(_)或数字0～9组成。
-  大写字母和小写字母代表不同的标识符。
-  不能是C++关键字或操作符。

### 4.数据类型

![image.png](https://i.loli.net/2020/01/13/BbKMDmuqsCxoVAg.png)

### 5.初始化

```c++
int a = 0;//int a=1.2;编译通过，只给warning

int a(0);//直接初始化

int a = {0};//列表初始化  如果int a={1.2};编译不能通过,直接给error

int a{0};
```

**其中使用大括号的初始化方式称为列表初始化，列表初始化时不允许信息的丢失(编译会报警)。例如用double值初始化int变量，就会造成数据丢失。**

```c++
vector<int>v1(10); //v1有10个元素，每个的值都是0

vector<int> v2{10}; //v2有1个元素，该元素的值时10

vector<int> v3(10,1); //v3有10个元素，每个的值都是1

vector<int> v4{10,1}; //v4有两个元素，值分别是10,1

vector<string> v5{"hi}; //列表初始化，v5有一个元素

vector<string> v6("hi"); //错误，不能使用字符串字面值构造vector对象

vector<string> v7(10); //v7有10个默认初始化的元素

vector<string> v8{10,"hi"}; //v8有10个值为”hi“的元素
```



### 6.逗号运算、关系运算、逻辑运算和条件运算

#### 逗号运算符：一个比较长的句子，中间逗号断一下

表达式1，表达式2

先求解表达式1，再求解表达式2，最终结果为表达式2的值。

(a = 3 * 5 , a * 4) 这个式子整体等于60,而a等于15。

#### 关系运算符

<    <=    >     >=     ==    !=

#### 逻辑运算符

！  &&  ||

**逻辑与**

**表达式1 && 表达式2**

- 先求解表达式1
- 若表达式1的值为false，则最终结果为false，不再求解表达式2。
- 若表达式1的结果为true，则再求解表达式2。



**逻辑或**

**表达式1 || 表达式2**

- 先求解表达式1。
- 若表达式1的值为true，则最终结果为true，不再求解表达式2。
- 若表达式1的结果为false，则求解表达式2，以表达式2的结果作为最终结果

#### 条件表达式

**表达式1？表达式2：表达式3**

- 先求解表达式1，
- 若表达式1的值为true，则求解表达式2，表达式2的值为最终结果
- 若表达式1的值为false，则求解表达式3，表达式3的值为最终结果

**表达式1是bool类型，表达式2、3的类型可以不同，条件表达式的最终类型为2 和3 中较高的类型（不管最终取得是值是表达式2还是表达式3）。**

### 7.sizeof

例如：

`sizeof(short)`

`sizeof x`

**返回类型：`unsigned int`  也就是`size_t`类型**

### 8.类型转换

- 将一个浮点数赋给整数类型时，结果值将只保留浮点数中的整数部分，小数部分将丢失。
- 将一个整数值赋给浮点类型时，小数部分记为0。如果整数所占的空间超过了浮点类型的容量，精度可能有损失。

**显示转换**

- 类型说明符(表达式)

- (类型说明符)表达式

- 类型转换操作符<类型说明符>(表达式)

-  类型转换操作符可以是：
  `const_cast、dynamic_cast、reinterpret_cast、static_cast`

  **例：`int(z)`, `(int)z`, `static_cast<int>(z)**`
  **三种完全等价**

### 9.形参实参

函数声明、函数定义时，括号里的参数叫形参。

函数调用时，括号里的参数叫实参。

在调用的时候，用实参去初始化形参，也就是这时才会为形参分配存储空间。

### 10.自定义类型

#### 类型别名：为已有类型另外命名

- `typedef  已有类型名 新类型名表`
- `using 新类型名 = 已有类型名;`

例如：

- ​	`typedef double yyd;`
- ​	`using yyd=double;`

这两句话效果一致

#### 枚举类型

**不限定作用域的枚举类型**：

`enum 枚举类型名 {变量值列表};`

例：

`enum Weekday {SUN, MON, TUE, WED, THU, FRI, SAT};`

默认情况下

`SUN=0，MON=1，TUE=2，......，SAT=6`

```c++
#include <iostream>

using namespace std;

enum GameResult {WIN, LOSE, TIE, CANCEL};

int main() {

      GameResult result;//声明result是枚举类型的变量

      enum GameResult omit = CANCEL;//声明时加不加enum没影响，omit是枚举类型的变量

      for (int count = WIN; count <= CANCEL; count++) {//枚举类型可以直接赋给整型变量。
          //枚举值可以进行关系运算。

        result = GameResult(count);//整型变量不能直接赋给枚举类型，要强制转换。

        if (result == omit)

          cout << "The game was cancelled" << endl;

        else {

          cout << "The game was played ";

          if (result == WIN)      cout << "and we won!";

          if (result == LOSE)       cout << "and we lost.";

          cout << endl;

        }

      }

      return 0;

}
```

### 11.auto类型与decltype类型

#### auto类型

auto：编译器通过初始值自动推断变量的类型

- 例如：`auto val = val1 + val2;`
  - 如果val1+val2是`int`类型，则`val`是`int`类型；
  - 如果val1+val2是double类型，则`val`是`double`类型。

#### `decltype`类型

定义一个变量与某一表达式的类型相同，但并不用该表达式初始化变量。

```c++
decltype(i) j = 2;//j 以 2 作为初始值，但是类型与i一致
```

## 第3章 函数

### 1.rand

函数原型：`int rand(void)；`

功能和返回值：求出并返回一个伪随机数

- 如果在同一个代码中，多次调用rand，产生一个随机数序列，那么它每次跟每次产生的确实是不一样的，是随机的。
- 但是如果你多次运行这个代码，会发现，每次运行跟每次运行的这个随机数序列是一模一样的。

### 2.srand

`void srand(unsigned int seed);`

参数：seed产生随机数的种子

功能：为使rand()产生一序列伪随机整数而设置起始点。使用1作为seed参数，可以重新初化rand()。

**只要每次运行时，给一个不同的随机数种子，就可以实现不同的随机数序列了**

### 3.函数的参数传递（传值、传引用）

- 在函数被调用时才会分配形参的存储单元。
- 实参可以是常量、变量、表达式
- **实参类型与形参类型不符合时，编译器会试图做隐式转换**
- 值传递，即单项传递
- 引用传递，即双向传递
- 常引用传递可以保证数据安全

```c++
void function(int& a);//引用
//调用时
int x=0;
function(x);
```

```c++
void function(int* p);//指针
//调用时
int x=0;
function(&x)//取地址符
```

- **&跟类型(例如和`int`)在一起时，是引用；**
- **&跟变量(例如和x)在一起时，是取地址**。

### 4.含有可变参数的函数（initializer_list/模板）

- **如果所有的实参类型相同**，**可以传递一个名initializer_list的标准库类型**
- **如果实参的类型不同，我们可以编写可变参数的模板**

```c++
//initializer_list
std::vector<int> vec = {1, 2, 3, 4, 5};//要求这些实参类型相同，都是int
//在c++11之前不允许这样列表初始化
//现在可以了，是因为 {1, 2, 3, 4, 5}自动变成一个initializer_list对象
//显式定义即
std::initializer_list<int> data = {1, 2, 3, 4, 5};//要求这些实参类型相同，都是int
```

### 5.内联函数（提高运行效率的机制）

就是让我们调用简单函数的时候，能够提高运行效率的一种机制

-  声明时使用关键字 inline。
-  **编译时在调用处用函数体进行替换，节省了参数传递、控制转移等开销。**
- **换句话说，编译时把子函数放到了主函数中实现，省去了转去子函数的开销。（没有了参数传递，现场保护等等操作）**
- inline只是对编译器的建议，编译器可能不接受这种建议，不理会你的内联声明
-  注意：
  -  内联函数体内不能有循环语句和switch语句；
  -  内联函数的定义必须出现在内联函数第一次被调用之前；
  -  对内联函数不能进行异常接口声明。

```c++
#include <iostream>
using namespace std;
const double PI = 3.14159265358979;
inline double calArea(double radius) {
          return PI * radius * radius;
}
int main() {

          double r = 3.0;
          double area = calArea(r);
          cout << area << endl;
          return 0;
}
```

### 6.constexpr函数（用来初始化常量的）

**常量表达式函数，用来初始化常量的。**

**（感觉没啥用，不建议使用）**

**`constexpr`函数语法规定**

- `constexpr`修饰的函数在其所有参数都是`constexpr`时，一定返回`constexpr`；
- 函数体中必须有且仅有一条return语句。

```c++
constexpr int get_size() { return 20; }//也就是说get_size这个函数，是在编译阶段就可以算出来的
constexpr int foo = get_size(); 
```

### 7.带默认参数值的函数

#### 规则

```c++
int add(int x = 5,int y = 6) {
     return x + y;
}
int main() {
     add(10,20);  //10+20
     add(10);     //10+6
     add();       //5+6
}
```

- **有默认参数的形参必须列在形参列表的最右，即默认参数值的右面不能有无默认值的参数；**

-  **调用时实参与形参的结合次序是从左向右。**

-  例：

  - ```c++
     int add(int x, int y = 5, int z = 6);//正确
     int add(int x = 1, int y = 5, int z);//错误
     int add(int x = 1, int y, int z = 6);//错误
    ```

#### 默认参数值与函数的调用位置

- 如果一个函数有原型声明，且原型声明在定义之前，或者定义在其他文件中，则默认参数值应在**函数原型声明**中给出；
- 如果只有函数的定义，或函数定义在前，则默认参数值**可以在函数定义**中给出。

![image.png](https://i.loli.net/2020/01/14/StQk5qhTVJfjBCy.png)

### 8.函数重载（函数名相同）

- **函数重载**是c++多态性的一个重要机制，它是由静态多态性实现的，也就是说这种多态性**是在编译阶段实现的多态性。**
- C++允许功能相近的函数在相同的作用域内以相同函数名声明，从而形成重载。方便使用，便于记忆。

```c++
int add(int x,int y);//形参类型不同
float add(float x,float y);

int add(int x,int y);//形参个数不同
int add(int x, int y,int z);
```

- **编译程序将根据实参和形参的类型及个数的最佳匹配来选择调用哪一个函数。**
- **各个重载函数的形参必须不同:即 类型不同 或 个数不同。**
- **形参相同，返回值不同也不行。因为返回值不能用于区分函数重载。只能通过形参区分不同函数重载**

![image.png](https://i.loli.net/2020/01/14/PoAxIclpeU5Fvyh.png)

## 第4章 类与对象 

### 1.面向对象程序的基本特点

####  抽象

- 对同一类对象的共同属性和行为进行概括，形成类。
  -  先注意问题的本质及描述，其次是实现过程或细节。
  -  数据抽象：描述某类对象的属性或状态（对象相互区别的物理量）。
  -  代码抽象：描述某类对象的共有的行为特征或具有的功能。
  -  抽象的实现：类。
-  抽象实例——钟表
  -  数据抽象：属性或状态
    - `int hour,int minute,int second`
  -  代码抽象：功能
    - `setTime(),showTime()`

```c++
class  Clock {
  public:
   void setTime(int newH, int newM, int newS);//设置时间
   void showTime();//显示时间
  private:
   int hour, minute, second;
};
```

#### 封装

- 将抽象出的数据、代码封装在一起，形成类。
  -  目的：增强安全性和简化编程，使用者不必了解具体的实现细节，而只需要通过外部接口，以特定的访问权限，来使用类的成员。
  -  实现封装：类声明中的{}

![image.png](https://i.loli.net/2020/01/14/9ZEto8l5Nj6BxHm.png)

#### 继承

在已有类的基础上，进行扩展形成新的类。详见第7章。

#### 多态

-  多态：同一名称，不同的功能实现方式。
-  目的：达到行为标识统一，减少程序中标识符的个数。
-  实现：重载函数和虚函数——见第8章

### 2.类和对象的定义

#### 定义

```c++
class 类名称
{
  public:
           //公有成员（外部接口）
  private:
           //私有成员
  protected:
           //保护型成员
};
```

#### 类内初始值（数据成员自带初始值）

- 可以为数据成员提供一个类内初始值

- **在创建对象时，类内初始值用于初始化数据成员**

- 没有初始值的成员将被默认初始化(比如`int a`)。

- 如果有构造函数对数据成员进行初始化，就回按照**构造函数指定的值初始化**。如果构造函数**没有对某些数据成员进行初始化**，将会**采用类内初始值**。

- ```c++
  class A
  {
  public:
  	A()
  	{
  		cout<<"A ctor 1 param: _i:" << _ci << endl;
  	}
  
      A(int i)
          : _ci{i}
      {
          cout << "A ctor 1 param: _i:" << _ci << endl;
      }
   
      //这个与A()构造函数不能共存
      /*A(int i=1)
          :_ci{i}
      {
          cout << "A ctor 2 param: _i:" << _ci << endl;
      }*/
   private:
       int _ci=1000;//类内初始值，如果构造函数中没有初始化这个变量，将使用这个类内初始值
  };
  ```

  

```c++
class Clock {
public:
  void setTime(int newH, int newM, int newS);
  void showTime();
private:
  int hour = 0, minute = 0, second = 0;
};
```

#### 类成员的访问控制（权限）

- 公有类型成员
  - 在关键字public后面声明，它们是类与外部的接口，任何外部函数都可以访问**公有类型数据和函数**。
-  私有类型成员
  - 在关键字private后面声明，**只允许本类中的函数访问**，而类外部的任何函数都不能访问（友元除外）。
  -  如果紧跟在类名称的后面（也就是类的最开头）声明私有成员，则关键字private可以省略。
-  保护类型成员
  -  **与private类似，其差别表现在继承与派生时对派生类的影响不同**，详见第七章。

#### 对象定义语法

-  类名 对象名；
-  例：`Clock myClock;`

#### 类成员的访问权限

-  类中成员互相访问
  -  直接使用成员名访问
-  类外访问
  -  使用“**对象名.成员名**”方式访问 **public 属性的成员**

#### 类的成员函数

-  在类中说明函数原型；
-  可以在类外给出函数体实现，并在函数名前使用类名加以限定；
-  也可以直接**在类中给出函数体**，形成**内联成员函数**；
-  **允许**声明**重载函数**和**带默认参数值的函数**。

#### 内联成员函数

-  为了提高运行时的效率，对于较简单的函数可以声明为内联形式。
-  **内联函数体**中**不要有**复杂结构（如**循环语句和switch语句**）。
-  在类中声明内联成员函数的**两种方式**：
  -  1.**将函数体放在类的声明中（无需inline关键字）。**
  -  2.**类体中只声明原型，类外实现函数，且使用inline关键字。**

#### 例子

```c++
#include<iostream>
using namespace std;
//类声明
class Clock{
public:         
     void setTime(int newH = 0, int newM = 0, int newS = 0);
     void showTime();
private:  
     int hour, minute, second;
}; 

//成员函数的实现
void Clock::setTime(int newH, int newM, int newS) {
   hour = newH;
   minute = newM;
   second = newS;
}

void Clock::showTime() {
   cout << hour << ":" << minute << ":" << second;
}

//主函数
int main() {
     Clock myClock;//定义对象
     myClock.setTime(8, 30, 30);
     myClock.showTime();
     return 0;

}
```

### 3.构造函数（可以重载）

#### 构造函数的形式

-   函数名与类名相同；
-   **不能定义返回值类型，也不能有return语句；**

-   可以有形式参数，也可以没有形式参数；

-   可以是内联函数；

-   可以重载；

-   可以带默认参数值。

#### 构造函数的调用时机

- **在对象创建时被自动调用，无需显式调用**
-  例如：
  - `Clock myClock（0,0,0）;`

#### 默认构造函数

-  **调用时可以不需要实参的构造函数（包括 没有形参 和 形参都有默认值 两种情况）**
  -  参数表为空的构造函数
  -  全部参数都有默认值的构造函数
-  下面两个都是默认构造函数，如在类中同时出现，将产生编译错误：**这两个都可以作为默认构造函数，但是默认构造函数只能存在一个**
  - `Clock();`**//没有形参的默认构造函数**
  - `Clock(int newH=0,int newM=0,int newS=0);`**//形参都有默认值的默认构造函数**

#### 隐含生成的构造函数

 **如果程序中未定义构造函数**，编译器将在需要时**自动生成一个默认构造函数**

-  参数列表为空，不为数据成员设置初始值；
-  **如果类内定义了成员的初始值，则使用内类定义的初始值**；
-  **如果没有定义类内的初始值，则以默认方式初始化**；(默认初始化比如`int a; `)
-  基本类型的数据**默认初始化**的值是不确定的。(比如`int a;`)

#### “=default”

如果程序中**已定义任何一种构造函数**，**默认情况下编译器就不再隐含生成默认构造函数，需要自己写一个无参数（无形参或者形参都有默认值）的构造函数**。如果此时依然希望编译器隐含生成默认构造函数，可以**使用“=default”**。

```c++
//例如
class Clock {
public:
   Clock() =default; //指示编译器提供默认构造函数
   Clock(int newH, int newM, int newS);     //构造函数
private:
   int hour, minute, second;
};
```

#### explicit

```c++
class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
{  
public:  
    char *_pstr;  
    int _size;  
    CxString(int size)  
    {  
        _size = size;                // string的预设大小  
        _pstr = malloc(size + 1);    // 分配string的内存  
        memset(_pstr, 0, size + 1);  
    }  
    CxString(const char *p)  
    {  
        int size = strlen(p);  
        _pstr = malloc(size + 1);    // 分配string的内存  
        strcpy(_pstr, p);            // 复制字符串  
        _size = strlen(_pstr);  
    }  
    // 析构函数这里不讨论, 省略...  
};  
  
    // 下面是调用:  
  
    CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
    CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数, 错误为: “CxString”: 没有合适的默认构造函数可用  
    CxString string4("aaaa"); // 这样是OK的  
    CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
    CxString string6 = 'c';   // 这样也是OK的, 其实调用的是CxString(int size), 且size等于'c'的ascii码  
    string1 = 2;              // 这样也是OK的, 为CxString预分配2字节的大小的内存  
    string2 = 3;              // 这样也是OK的, 为CxString预分配3字节的大小的内存  
    string3 = string1;        // 这样也是OK的, 至少编译是没问题的, 但是如果析构函数里用free释放_pstr内存指针的时候可能会报错, 完整的代码必须重载运算符"=", 并在其中处理内存释放  
```

```c++
class CxString  // 使用关键字explicit的类声明, 显示转换  
{  
public:  
    char *_pstr;  
    int _size;  
    explicit CxString(int size)  
    {  
        _size = size;  
        // 代码同上, 省略...  
    }  
    CxString(const char *p)  
    {  
        // 代码同上, 省略...  
    }  
};  
  
    // 下面是调用:  
  
    CxString string1(24);     // 这样是OK的  
    CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数  
    CxString string4("aaaa"); // 这样是OK的  
    CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
    CxString string6 = 'c';   // 这样是不行的, 其实调用的是CxString(int size), 且size等于'c'的ascii码, 但explicit关键字取消了隐式转换  
    string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
    string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
    string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载  
```



### 4.利用初始化列表对类成员进行初始化（高效的初始化方式）

- **注意：只有构造函数可以通过初始化列表对类成员进行初始化，普通的函数成员不具备这个功能。**
- 初始化列表的作用是初始化，而不是二次赋值。
- 初始化列表在构造函数执行前执行(这个可以看上面的结果，对同一个变量在初始化列表和构造函数中分别初始化，首先执行参数列表，后在**函数体内赋值**，后者**会覆盖前者（前者即初始化列表初始化的值）**。

必须采用初始化列表的三种情况：

https://blog.csdn.net/sinat_20265495/article/details/53670644

- 情况一、需要初始化的数据成员是**对象**的情况(这里包含了继承情况下，通过显示调用父类的构造函数对父类数据成员进行初始化)； 
- 情况二、需要**初始化const**修饰的类成员**或初始化引用**成员数据；
- 情况三、子类初始化父类的私有成员；

```c++
//类定义
class Clock {
public:
     Clock(); //默认构造函数
     Clock(int newH,int newM,int newS);//构造函数
     void setTime(int newH, int newM, int newS);
     void showTime();
private:
     int hour, minute, second;
}; 

//默认构造函数实现     //不能定义返回值类型，也不能有return语句
Clock::Clock(): hour(0),minute(0),second(0) {}//利用初始化列表对类成员进行初始化，效率最高

//构造函数的实现：     //不能定义返回值类型，也不能有return语句
Clock::Clock(int newH,int newM,int newS): hour(newH),minute(newM),  second(newS) {}//利用初始化列表对类成员进行初始化，效率最高

//其它函数实现同例4_1 
int main() {
  Clock c(0,0,0); //自动调用构造函数
  c.showTime();
  return 0;
}
```

### 5.委托构造函数（避免构造函数代码重复，人力浪费）

类中往往有多个构造函数，只是**参数表和初始化列表不同**，其初始化算法都是相同的，这时，为了**避免代码重复**，可以使用委托构造函数。

**也就是说一个构造函数可以委托另一个构造函数来帮它实现初始化功能**

委托构造函数使用类的其他构造函数执行初始化过程：

```c++
//例如 之前的两个构造函数
Clock(int newH, int newM, int newS) : hour(newH),minute(newM),  second(newS)  {} //构造函数
Clock::Clock(): hour(0),minute(0),second(0) { }//默认构造函数

//委托法
Clock(int newH, int newM, int newS):  hour(newH),minute(newM),  second(newS){}//跟原来一样
Clock(): Clock(0, 0, 0) {}//调用了上边那个构造函数
```

### 6.复制构造函数（不能重载，因为形参是定的）

#### 复制构造函数定义

复制构造函数是一种特殊的构造函数，其形**参为本类的对象引用**。作用是**用一个已存在的对象去初始化同类型的新对象**。

```c++
class 类名 {
public :
    类名（形参）；//构造函数
    类名（const  类名 &对象名）；//复制构造函数
    //       ...
}；
    
//复制构造函数的实现    
类名::类（ const  类名 &对象名）
{    函数体    }
```

#### 隐含的复制构造函数

-  **如果程序员没有为类声明拷贝初始化构造函数**，则**编译器自己生成一个隐含的**复制构造函数。
-  这个构造函数执行的功能是：用作为初始值的对象的每个数据成员的值，初始化将要建立的对象的对应数据成员。

**注意：**自动生成的**隐含复制构造函数** 跟 自动生成的**隐含默认构造函数**不一样。**隐含复制构造函数**会实现两个对象的数据成员一对一复制，而**隐含默认构造函数**不干事，只会采用默认方式初始化数据成员，比如`int a`,这样初始化的数据成员值是不确定的。

#### “=delete”

如果**不希望对象被复制构造**，(不仅仅是不提供隐含复制构造函数)。

**C++98**做法：将复制构造函数声明为**private**，并且**不提供函数的实现**。

**C++11**做法：用**“=delete”指示编译器不生成默认（隐含）复制构造函数**。

**换句话说：自己写的复制构造函数一定要是`publlic**`

```c++
class Point {  //Point 类的定义
public:
  Point(int xx=0, int yy=0) { x = xx; y = yy; }  //构造函数，内联
  Point(const Point& p) =delete; //指示编译器不生成默认（隐含）复制构造函数
private:
  int x, y; //私有数据
};
```

#### 复制构造函数被调用的三种情况

-  定义一个对象时，以**本类另一个对象作为初始值**，发生复制构造；
-  如果**函数的形参是类的对象**，调用函数时，将使用**实参对象初始化形参对象**，**发生复制构造**；
-  如果**函数的返回值是类的对象**，函数执行完成返回主调函数时，将使用**return语句**中的对象初始化一个临时无名对象，**传递给主调函数，此时发生复制构造**。
  -  这种情况也可以通过移动构造避免不必要的复制（第6章介绍）

**一般情况下，默认（隐含）复制构造函数就挺好用的，但是如果类的成员中有指针的时候，一般就得自己写了。**

```c++
int main()
{
	Point a;//调用默认构造函数
	Point b;//调用默认构造函数
	Point c=a;//调用复制构造函数  等同于Point c(a);
	c = b;//什么都不调用，赋值不是构造！
	return 0;
}
```

### 7.析构函数

- 完成对象被删除前的一些清理工作。
- 在对象的生存期结束的时刻系统自动调用它，然后再释放此对象所属的空间。
- 如果程序中未声明析构函数，编译器将**自动产生一个默认的析构函数**，其**函数体为空(什么都不做)**。
- 构造函数和析构函数举例

```c++
class Point {     
public:
  Point(int xx,int yy);//构造函数
  ~Point();//析构函数
  //...其他函数原型
private:
  int x, y;
};

Point::Point(int xx,int yy){x=xx;y=yy;}//构造函数实现
Point::~Point(){}//析构函数实现
```

### 8.类的组合（类的数据成员是另一个类对象）

#### 组合的概念

类中的成员是另一个类的对象。

#### 类组合的构造函数设计

原则：不仅要负责**对本类中的基本类型成员数据初始化**，也要**对对象成员初始化**。

**就是说，要给大量的形参，即给本类中的数据成员初始化；又要给本类中的调用的其他类对象成员初始化。举例：computer 类构造函数形参中，要包含自己类的数据成员功率（unsigned int）的形参，又要包含自己类的对象成员 RAM类对象 的形参，用于初始化RAM类对象**

声明形式：

```c++
//构造函数
类名::类名(对象成员所需的形参，本类成员形参)
:对象1(参数)，对象2(参数)，......//利用初始化列表对类成员进行初始化
{
//函数体其他语句
}
```

#### 构造组合类对象时的初始化次序

- 首先对构造函数初始化列表中列出的成员（包括基本类型成员和对象成员）进行初始化，**初始化次序**是**成员在类体中定义的次序(而不是初始化列表里的顺序)**。
  - 成员对象构造函数调用顺序：按对象成员的声明顺序，先声明者先构造。
  - 初始化列表中未出现的成员对象，调用用默认构造函数（即无形参的）初始化
- 处理完初始化列表之后，再执行构造函数的函数体。

#### 举例

```c++
#include <iostream>
#include <cmath>
using namespace std;
class Point { //Point类定义
public:
	Point(int xx = 0, int yy = 0) {x = xx;y = yy;}//默认构造函数，内联实现
	Point(Point& p);//复制构造函数
	int getX() { return x; }//函数成员，内联实现
	int getY() { return y; }
private:
	int x, y;
};

Point::Point(Point& p) { //复制构造函数的实现
	x = p.x;
	y = p.y;
	cout << "Calling the copy constructor of Point" << endl;
}

//类的组合
class Line { //Line类的定义
public: //外部接口
	Line(Point xp1, Point xp2);//构造函数
	Line(Line& l);//复制构造函数
	double getLen() { return len; }//函数成员，内联实现

private: //私有数据成员
	Point p1, p2; //Point类的对象p1,p2
	double len;
};

//组合类的构造函数
Line::Line(Point xp1, Point xp2) : p2(xp2), p1(xp1) {
	cout << "Calling constructor of Line" << endl;
	double x = static_cast<double>(p1.getX()) - static_cast<double>(p2.getX());
	double y = static_cast<double>(p1.getY()) - static_cast<double>(p2.getY());
	len = sqrt(x * x + y * y);
}

Line::Line(Line& l) : p1(l.p1), p2(l.p2) {//组合类的复制构造函数
	cout << "Calling the copy constructor of Line" << endl;
	len = l.len;
}

//主函数
int main() {
	Point myp1(1, 1), myp2(4, 5); //建立Point类的对象
	Line line(myp1, myp2); //建立Line类的对象
	Line line2(line); //利用复制构造函数建立一个新对象
	cout << "The length of the line is: ";
	cout << line.getLen() << endl;
	cout << "The length of the line2 is: ";
	cout << line2.getLen() << endl;
	return 0;
}
```

主函数详解：

```c++
int main() {
	Point myp1(1, 1), myp2(4, 5); 
    //第一次调用Point类构造函数，构造myp1,再一次调用Point类构造函数构造myp2	
    
    //————————————————————复合类的构造函数——————————————
    Line line(myp1, myp2); //
    //形实结合从后往前传，所以
    //1. 调用Point类复制构造函数，将实参myp2复制给line函数（Line类的构造函数）的形参xp2
    //2. 调用Point类复制构造函数，将实参myp1复制给line函数（Line类的构造函数）的形参xp1
    //
    //Line::Line(Point xp1, Point xp2) :  p2(xp2),p1(xp1) {//....} 先执行列表初始化，再执行函数体。函数是列表初始化方式，但是初始化的次序是由Point xp1, Point xp2在Line类中对象成员的声明顺序，先声明者先构造。所以尽管顺序是 p2(xp2),p1(xp1)，但是在line类中p1比p2先声明，依然是先构造p1。
    //3. 调用Point类复制构造函数，将形参Point xp1复制给Line类的数据成员Point p1
    //4. 调用Point类复制构造函数，将形参Point xp2复制给Line类的数据成员Point p2
	//
    //5.列表初始化执行完了，执行Line::Line(Point xp1, Point xp2) :  p2(xp2),p1(xp1) {//....}函数体部分
    
    //————————————————————复合类的复制构造函数——————————————
    Line line2(line); //利用复制构造函数建立一个新对象
    //调用Line类的复制构造函数。
    //Line::Line(Line& l) : p1(l.p1), p2(l.p2) {//组合类的复制构造函数}
    //由于Point p1比Point p2在类中先声明，所以先初始化（构造）p1
    //1.  p1(l.p1) 调用Point类复制构造函数，构造Line类数据成员p1
    //2.  p2(l.p2) 调用Point类复制构造函数，构造Line类数据成员p2
    //3. 执行完列表初始化，执行函数体部分
    
	cout << "The length of the line is: ";
	cout << line.getLen() << endl;
	cout << "The length of the line2 is: ";
	cout << line2.getLen() << endl;
	return 0;
```

### 9.前向引用声明(解决两个类互相引用)

- 类应该先声明，后使用
- 如果需要在某个类的声明之前，引用该类，则应进行前向引用声明。
- 前向引用声明只为程序引入一个标识符，但具体声明在其他地方。
- **主要用于解决的问题是：两个类互相引用**

```c++
class B;  //前向引用声明

class A {
public:
  void f(B b);
};

class B {
public:
  void g(A a);
};
```

前向引用声明注意事项

-  使用前向引用声明虽然可以解决一些问题，但它并不是万能的。
-  在**提供一个完整的类声明之前**，**不能声明该类的对象**，也不能在内联成员函数中使用该类的对象。
-  当使用前向引用声明时，只能使用被声明的符号，而**不能涉及类的任何细节**。

```c++
class Fred; //前向引用声明

class Barney {
   Fred x; //错误：类Fred的声明尚不完善
};

class Fred {
   Barney y;
};
```

### 10.UML（类似流程图）

![image.png](https://i.loli.net/2020/01/15/F8hp6iPtrBExnWC.png)



![image.png](https://i.loli.net/2020/01/15/Lg8EbDGceBt7RYo.png)

![image.png](https://i.loli.net/2020/01/15/JjoMyPxGLwQfUsb.png)

![image.png](https://i.loli.net/2020/01/15/vpsKBEVjNQFHGgi.png)



![image.png](https://i.loli.net/2020/01/15/W6h8AaDplxobsru.png)

![image.png](https://i.loli.net/2020/01/15/3xclN1bpBJrfSoE.png)

![image.png](https://i.loli.net/2020/01/15/WcmDHvbzy9nZ5pa.png)

### 11.结构体（只有数据成员，没有函数成员的类）

- 结构体是一种特殊形态的类
- 与类的唯一区别：类的缺省访问权限是private，结构体的缺省访问权限是public
- 结构体存在的主要原因：与C语言保持兼容
- 什么时候用结构体而不用类
  - 定义主要用来保存数据、而没有什么操作的类型
- 人们习惯将结构体的数据成员设为公有，因此这时用结构体更方便

```c++
struct 结构体名称 {
	 公有成员
protected:
    保护型成员
private:
     私有成员
};
```

#### 结构体的初始化

如果一个结构体的全部数据成员都是公共成员，并且没有用户定义的构造函数，没有基类和虚函数（基类和虚函数将在后面的章节中介绍），这个结构体的变量可以用下面的语法形式赋初值。
`类型名 变量名 = { 成员数据1初值, 成员数据2初值, …… };`

```c++
#include 
#include 
#include 
using namespace std;

struct Student {	//学生信息结构体
	int num;		//学号
	string name;	//姓名，字符串对象，将在第6章详细介绍
	char sex;		//性别
	int age;		//年龄
};

int main() {
	Student stu = { 97001, "Lin Lin", 'F', 19 };
	cout << "Num:  " << stu.num << endl;
	cout << "Name: " << stu.name << endl;
	cout << "Sex:  " << stu.sex << endl;
	cout << "Age:  " << stu.age << endl;
	return 0;
}

运行结果：
Num:  97001
Name: Lin Lin
Sex:  F
Age:  19
```

### 12.联合体（几个变量共用存储空间，没啥用）

**联合体的目的是几个变量共用存储空间（类中的几个数据成员共用存储空间）**

#### 声明形式

```c++
union 联合体名称 {
    公有成员
protected:
    保护型成员
private:
    私有成员
};
```

#### 特点

- **成员共用同一组内存单元**
- **任何两个成员不会同时有效**

#### 联合体的内存分配

```c++
union Mark {	//表示成绩的联合体
	char grade;	//等级制的成绩
	bool pass;	//只记是否通过课程的成绩
	int percent;	//百分制的成绩
};
//整个union只占4个字节（最多字节数的成员的空间）
```

#### 例子

```c++
#include<iostream>
#include<string>
using namespace std;
class ExamInfo {
private:
	string name;	//课程名称
	enum { GRADE, PASS, PERCENTAGE } mode;//计分方式
	union {
		char grade;	//等级制的成绩
		bool pass;	//只记是否通过课程的成绩
		int percent;	//百分制的成绩
	};
public:
	//三种构造函数，分别用等级、是否通过和百分初始化
	ExamInfo(string name, char grade)
		: name(name), mode(GRADE), grade(grade) { }
	ExamInfo(string name, bool pass)
		: name(name), mode(PASS), pass(pass) { }
	ExamInfo(string name, int percent)
		: name(name), mode(PERCENTAGE), percent(percent) { }
	void show();
}

void ExamInfo::show() {
	cout << name << ": ";
	switch (mode) {
	  case GRADE: cout << grade;  break;
	  case PASS: cout << (pass ? "PASS" : "FAIL"); break;
	  case PERCENTAGE: cout << percent; break;
	}
	cout << endl;
}

int main() {
	ExamInfo course1("English", 'B');
	ExamInfo course2("Calculus", true);
	ExamInfo course3("C++ Programming", 85);
	course1.show();
	course2.show();
	course3.show();
	return 0;
}
/*运行结果：
English: B
Calculus: PASS
C++ Programming: 85*/
```

### 13.枚举类（强类型的枚举,或者叫有限定作用域的枚举）

#### c语言继承的枚举

之前从c语言继承而来的枚举类型，有个最严重的问题就是，枚举类型可以自动隐含转换为整数类型，即类型定义不是很严格。

`enum 枚举类型名 {变量值列表};`

例：

`enum Weekday {SUN, MON, TUE, WED, THU, FRI, SAT};`

#### 枚举类定义

- 语法形式

  - `enum class 枚举类型名: 底层类型 {枚举值列表};`

- 例：

  - ```c++
     enum class Type { General, Light, Medium, Heavy};//默认底层类型是int类型
     enum class Type: char { General, Light, Medium, Heavy};
     enum class Category { General=1, Pistol, MachineGun, Cannon};
    ```

#### 枚举类相比之前简单枚举类型的优势

- 强作用域，其作用域限制在枚举类中。

  - 例：使用Type的枚举值General：
  - `Type::General  //不可以简单的只写General，可以避免不同枚举类型之间枚举值重名的问题`

-  转换限制，枚举类对象**不可以**与整型**隐式地互相转换。**

-  可以指定底层类型

    例：

  - `enum class Type: char { General, Light, Medium, Heavy};`

```c++
#include<iostream>
using namespace std;

enum class Side{ Right, Left };//默认底层类型是int类型
enum class Thing{ Wrong, Right };  //这里也有Right，但是不会跟上边冲突
int main()
{
Side s = Side::Right;
Thing w = Thing::Wrong;//两个变量底层默认都是int，但是无法隐式转换成整形int
    
cout << (s == w) << endl;  //编译错误，无法直接比较不同枚举类
return 0;
}
```

### 14.const成员函数（不修改调用对象的成员函数）

```c++
void show() const;//类内声明部分

//类外实现部分
void stock:show() const;//const成员函数，意味着该函数show不会修改调用对象。
```

### 15.类的组合中，列表初始化调用复制构造函数。非列表初始化调用默认构造函数

```c++
//全屏观看
class COMPUTER
{
public:
    //非列表初始化
	COMPUTER(CPU a, RAM b, CDROM c, int d) { cpu = a; ram = b; cdrom = c; s = d; }
    
    //列表初始化
    //COMPUTER(CPU a, RAM b, CDROM c, int d) :cpu(a), ram(b), cdrom(c), s(d) {}
	~COMPUTER() {}
private:
	CPU cpu;
	RAM ram;
	CDROM cdrom;
	int s = 0;
};

int main()
{
	CPU x(P6,300,2.8);//构造了一个CPU x
    RAM y(DDR3,1600,8);//构造了一个RAM y
    CD_ROM z(SATA,2,built_in);//构造了一个CD_ROM z
    
   //COMPUTER(CPU a, RAM b, CDROM c, int d) { cpu = a; ram = b; cdrom = c; s = d}时
    COMPUTER my_computer(x,y,z,128,10);
    //传参从右往左
    //拷贝构造 形参c，所以z复制构造了一个CD_ROM c
    //拷贝构造 形参b，所以y复制构造了一个RAM b
    //拷贝构造 形参a，所以x复制构造了一个CPU a
                                          
                                       //左右分界
//假如COMPUTER 构造函数没有使用列表初始化时     ↓↓        //假如采用列表初始化
//即COMPUTER(CPU a, RAM b, CDROM c, int d) ↓↓  //即COMPUTER(CPU a, RAM b, CDROM c, int d) 
//{ cpu = a; ram = b; cdrom = c; s = d}时   ↓↓    :cpu(a), ram(b), cdrom(c), s(d) {}
    
    //COMPUTER 里的数据（对象）成员将采用默认构造函数，然后采用赋值运算符。↓↓数据（对象）成员将采用复制构造
    
    //构造了一个数据成员 CPU cpu，然后将形参a的值赋值运算给cpu ↓↓ CPU a复制构造了一个数据成员CPU cpu
    //构造了一个数据成员RAM ram，然后将形参b的值赋值运算给ram ↓↓ RAM b复制构造了一个数据成员RAM ram
//构造了一个数据成员CD_ROM cd_rom，然后将形参c的值赋值运算给cd_rom ↓↓  CD_ROM c复制构造了cd_rom
    //构造完毕computer
    //析构形式参数a,所以析构了一个CPU a
    //析构形式参数b，所以析构了一个RAM b
    //析构形式参数c，所以析构了一个CDROM c
   
    return 0;
    //析构 COMPUTER my_computer
    //析构 my_computer中的数据成员CD_ROM
    //析构 my_computer中的数据成员RAM
    //析构 my_computer中的数据成员CPU
    
    //析构最开头定义的 CD_ROM z
    //析构最开头定义的 RAM y
    //析构最开头定义的 CPU x
}
```

## 第5章 数据的共享与保护

### 1.标识符的作用域与可见性

#### 标识符的作用域与可见性

- 作用域是一个标识符(函数名字，变量名字)在程序正文中有效的区域。
- 作用域分类：

1. 函数原型作用域
2. 局部作用域(块作用域)
3. 类作用域
4. 文件作用域
5. 命名空间作用域（详见第10章）

#### 函数原型作用域

**函数原型中的参数，其作用域始于"("，结束于")"。也就是函数声明时的参数表的括号之内**

函数原型作用域举例：

```c++
double area(double radius);//radius的作用域仅在于此（函数声明），不能用于程序正文其他地方
//或者写成
double area(double);
```

函数原型声明时，编译器不在乎名字是什么，只在乎类型，原因是这个的作用域就这么小，就只在声明时的括号之内，在这之内并不适用名字，离开这个函数原型声明，它的作用域就结束了。所以写不写名字都可以。

#### 局部作用域

- 函数的形参、在块中声明的标识符；

- 其作用域自声明处起，限于块中。

- 举例

  ![image.png](https://i.loli.net/2020/01/19/tmkBaiduT3HGYMD.png)

#### 类作用域

- **类的成员具有类作用域，其范围包括类体和非内联成员函数的函数体**（也就是类声明外的类成员函数实现部分）。（即类中定义的标识符在这些范围内可以访问的，可以起作用的）
- 如果在**类作用域以外访问类的成员**（类中定义的标识符），
  - 静态成员：通过类名、该类的对象名**（对象名.成员名  还要受public许可才行**）、对象引用  访问
  - 非静态成员：通过类名、该类的对象名、对象引用、对象指针 访问

#### 文件作用域

- 不在前述各个作用域中出现的声明，就具有文件作用域，
- 这样声明的标识符其作用域开始于声明点，结束于文件尾。

即你定义这个标识符所在的c++文件，文件作用域也称为静态作用域。

#### 可见性

这个标识符即使在作用域范围内，还要考虑它是不是可见的。只有可见的才可以访问。

- 可见性是从对标识符的引用的角度来谈的概念
- 可见性表示从内层作用域向外层作用域“看”时能看见什么。
- 如果标识在某处可见，就可以在该处引用此标识符。
- **如果某个标识符在外层中声明，且在内层中没有同一标识符的声明，则该标识符在内层可见。**
- **对于两个嵌套的作用域，如果在内层作用域内声明了与外层作用域中同名的标识符，则外层作用域的标识符在内层不可见。（也就是内层同名的标识符，将外层的同名标识符给屏蔽了，虽然外层的同名标识符依然在作用域之内，但是不可见了）**

![image.png](https://i.loli.net/2020/01/19/5fhknFWQUm1KGxI.png)

例子：

```c++
#include<iostream>
using namespace std;
 
int i;	//全局变量，文件作用域
int main() { 
     i = 5; //为全局变量i赋值
     {
         int i; //局部变量，局部作用域(局部变量i 将外层的同名变量i屏蔽了，外层i不可见)
         i = 7;
         cout << "i = " << i << endl;//输出7
      }
      cout << “i = ” << i << endl;//输出5
      return 0;
}
```

### 2.对象的生存期

#### 静态生存期（整个程序只分配一次内存，只初始化一次，数值每次保留）

- 这种生存期与程序的运行期相同。
- 两种具有静态生存期的情况：
  - 1.在文件作用域中声明的对象**（全局变量）**具有这种生存期。
  - 2.在函数内部声明静态生存期对象，要冠以关键字static 。（**虽然它是局部变量或者局部对象，虽然它的作用域在函数体这个局部范围之内，但是它在整个程序运行期间，它都是生存着的，内存不会被释放，下次有机会再回这个作用域时，还会继续使用这个结束时的静态值。**）

#### 动态生存期（每次调用时都会重新分配内存，每次都初始化，数值不保留）

- 块作用域中声明的，没有用static修饰的对象是动态生存期的对象（习惯称局部生存期对象）。

- 开始于程序执行到声明点时，结束于命名该标识符的作用域结束处。（作用域结束后，内存会被释放，如果又一次调用这个函数时，会重新分配内存空间，初始化等，所以上次结束时的数据不会被保留了）

例子：

```c++
#include<iostream>
using namespace std;

int i = 1; // i 为全局变量，具有静态生存期。（它不用写static）

void other() {
static int a = 2;//只在第一次进入函数时执行初始化，第二次再调用这个函数时就不会再初始化了
static int b;
// a,b为静态局部变量，具有全局寿命，局部可见。
//只第一次进入函数时被初始化。
int c = 10; // C为局部变量，具有动态生存期，
//每次进入函数时都初始化。
a += 2; i += 32; c += 5;
cout<<"---OTHER---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
b = a;
}


int main() {
static int a;//静态局部变量，有全局寿命，局部可见。静态变量默认初始化为0，其他的默认初始化未知。
int b = -10; // b, c为局部变量，具有动态生存期。
int c = 0;
cout << "---MAIN---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
c += 8; other();
cout<<"---MAIN---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
i += 10; other();
return 0;
}

/*运行结果：
---MAIN---
i: 1 a: 0 b: -10 c: 0
---OTHER---
i: 33 a: 4 b: 0 c: 15
---MAIN---
i: 33 a: 0 b: -10 c: 8
---OTHER---
i: 75 a: 6 b: 4 c: 15*/
```

### 3.类的静态成员

#### (1)静态数据成员（整个类共用这一个变量）

-   用关键字static声明

-   为**该类的所有对象共享，静态数据成员具有静态生存期**。（不会在每个对象中存储，整个类只存储一份）

-   **必须在类外定义和初始化**，用(::)来指明所属的类。

  

  ```c++
  #include <iostream>
  using namespace std;
  class Point {     //Point类定义
  public:       //外部接口
         Point(int x = 0, int y = 0) : x(x), y(y) { //构造函数
             //在构造函数中对count累加，所有对象共同维护同一个count
             count++;
         }
  
         Point(Point &p) {    //复制构造函数
             x = p.x;
             y = p.y;
             count++;
         }
  
         ~Point() {  count--; }
         int getX() { return x; }
         int getY() { return y; }
  
        void showCount() {           //输出静态数据成员
             cout << "  Object count = " << count << endl;
         }
  
  private:      //私有数据成员
         int x, y;
         static int count;       //静态数据成员声明，用于记录点的个数
  
  };
  
  int Point::count = 0;//静态数据成员定义和初始化，使用类名限定
  
  int main() {       //主函数
         Point a(4, 5);     //定义对象a，其构造函数回使count增1
         cout << "Point A: " << a.getX() << ", " << a.getY();
         a.showCount(); //输出对象个数
         Point b(a); //定义对象b，其构造函数回使count增1
         cout << "Point B: " << b.getX() << ", " << b.getY();
         b.showCount();       //输出对象个数
         return 0;
  
}
  ```

  

#### (2)静态函数成员（主要用于处理类的静态数据成员）

- 类外代码可以使用**类名和作用域操作符来调用静态成员函数**。
- **静态成员函数主要用于处理该类的静态数据成员，可以直接通过类名而不通过对象调用静态成员函数**。
- 如果访问非静态成员，要通过对象来访问。
- **我的理解：主要是想在未构建对象时，直接调用类名::静态函数，来查看某些静态数据成员（比如cout出来）。因为非静态函数其实也可以处理静态数据成员，没什么差别**

```c++
#include <iostream>
using namespace std;
class Point {    
public:      
       Point(int x = 0, int y = 0) : x(x), y(y) {  count++; }//构造函数

       Point(Point &p) {    //复制构造函数
           x = p.x;
           y = p.y;
           count++;
       }

       ~Point() {  count--; }
       int getX() { return x; }
       int getY() { return y; }
       static void showCount() {   
      cout << "  Object count = " << count << endl;
       }
private:     
       int x, y;
       static int count;       //静态数据成员声明，用于记录点的个数
};
int Point::count = 0;//静态数据成员定义和初始化，使用类名限定

int main() {      
       Point a(4, 5);     //定义对象a，其构造函数回使count增1
       cout << "Point A: " << a.getX() << ", " << a.getY();
       Point::showCount();       //输出对象个数
       Point b(a); //定义对象b，其构造函数回使count增1
       cout << "Point B: " << b.getX() << ", " << b.getY();
       Point::showCount();       //输出对象个数
       return 0;
}
```

静态成员函数可以直接引用该类的**静态数据成员和静态成员函数**，**但不能直接引用　非静态数据成员　和　非静态成员函数**，否则编译报错。如果要引用，**必须通过参数传递的方式得到对象名，然后再通过对象名引用**

```c++
#include<iostream>
using namespace std;
class Myclass
{
private:
int m; //　非静态数据成员
static int n; //　静态数据成员
public:
Myclass(); //　构造函数
static int getn(Myclass a); // 静态成员函数
};
Myclass::Myclass()
{
m = 10;
}
int Myclass::getn(Myclass a)
{
cout << a.m << endl; // 通过类间接使用 非静态数据成员
return n; // 直接使用 静态数据成员
}
int Myclass::n = 100; // 静态数据成员初始化
void main()
{
Myclass app1;
cout << app1.getn(app1) << endl; // 利用对象引用静态函数成员
cout << Myclass::getn(app1) << endl; // 利用类名引用静态函数成员
}
```



### 4.类的友元

#### 类的友元

- 友元是C++提供的一种破坏数据封装和数据隐藏的机制。
- 通过将一个模块声明为另一个模块的友元，一个模块能够引用到另一个模块中本是**被隐藏的信息(private\protected里的数据成员或者函数成员)**。
- 可以使用**友元函数和友元类**。
- 为了确保数据的完整性，及数据封装与隐藏的原则，建议尽量不使用或少使用友元。

#### 友元函数（它不是成员函数，而是类外的函数）

- 友元函数是在类声明中由关键字friend修饰说明的非成员函数，在它的函数体中能够通过对象名访问 private 和 protected成员
- 作用：增加灵活性，使程序员可以在封装和快速性方面做合理选择。
- 访问对象中的成员**必须通过对象名**。

```c++
#include <iostream>
#include <cmath>
using namespace std;

class Point { //Point类声明
public: //外部接口
Point(int x=0, int y=0) : x(x), y(y) { }
int getX() { return x; }
int getY() { return y; }
friend float dist(Point &a, Point &b);//友元函数声明，它不是成员函数，它是类外的函数
private: //私有数据成员
int x, y;
};

float dist( Point& a, Point& b) {//它不是Point类的成员函数，所以写函数时，不需要加上Point::。
    //友元函数的实现，传入的参数必须有对象名。
double x = a.x - b.x;//它可以读对象的private成员，非友元函数是访问不了对象里的private成员的
double y = a.y - b.y;
return static_cast<float>(sqrt(x * x + y * y));
}

int main() {
Point p1(1, 1), p2(4, 5);
cout <<"The distance is: ";
cout << dist(p1, p2) << endl;
return 0;
}
```

#### 友元类

- 若一个类为另一个类的友元，则**此类的所有成员（主要指函数成员）都能访问对方类的私有成员**。
- 声明语法：将友元类名在另一个类中使用friend修饰说明。

![image.png](https://i.loli.net/2020/01/23/jmFuMyQ1lLIrGbE.png)

#### 类的友元关系是单向的

如果声明B类是A类的友元，B类的成员函数就可以访问A类的私有和保护数据，但A类的成员函数却不能访问B类的私有、保护数据。

### 5.共享数据的保护

#### 共享数据的保护

-   对于既需要共享、又需要防止改变的数据应该声明为**常类型**（用const进行修饰）。
-   对于不改变对象状态的成员函数应该声明为**常函数**。

#### 常类型

- **常对象**：用const修饰的对象，必须在定义时进行初始化,不能被更新。

  -    const 类名 对象名

  - ```c++
    class A
    {
      public:
        A(int i,int j) {x=i; y=j;}
                         ...
    
      private:
        int x,y;
    };
    
    
    A const a(3,4); //a是常对象，不能被更新
    ```

    

- **常成员**

  -    用const进行修饰的类成员：**常数据成员和常函数成员**

  - **常成员函数（不打算修改对象的函数，一般都加上const）**

    - 使用const关键字说明的函数。

    - **常成员函数不更新对象的数据成员**。

    - 常成员函数说明格式：

      类型说明符  函数名（参数表）const;

      这里，const是函数类型的一个组成部分，因此在实现部分也要带const关键字。

    -  const关键字可以被用于参与对重载函数的区分

    - 通过**常对象只能调用它的常成员函数（const对象，只能调用const成员函数）**

    - ```c++
      //常成员函数举例
      #include<iostream>
      using namespace std;
      
      class R {
      public:
        R(int r1, int r2) : r1(r1), r2(r2) { }
        void print();
        void print() const;//const可用于区分重载函数
      private:
        int r1, r2;
      };
      
      void R::print() {
        cout << r1 << ":" << r2 << endl;
      }
      
      void R::print() const {
        cout << r1 << ";" << r2 << endl;
      }
      
      int main() {
        R a(5,4);
        a.print(); //调用void print()
        const R b(20,52); 
        b.print(); //调用void print() const
        return 0;
      }
      ```

    

  - 常数据成员

    - 使用const说明的数据成员。

    - **常数据成员不可以在构造函数函数体中赋值，只能通过列表初始化**，并且之后不可以改变这个值了。没有const限定的数据成员可以既可以在构造函数函数体赋中值，又可以列表初始化赋值

    - 

    - ```c++
      // 常数据成员举例
      #include <iostream>
      using namespace std;
      
      class A {
      public:
             A(int i);
             void print();
      private:
             const int a;//常数据成员,属于每个对象
             static const int b;  //静态常数据成员，属于整个类共享
      };
      const int A::b=10;//static成员，类体中声明，类体外定义和初始化。由于有const限定，初始化后再也不允许改变了
      
      A::A(int i) : a(i) { }//常成员a不可以在构造函数函数体中赋值，只能通过列表初始化，并且之后不可以改变这个值了。没有const限定的数据成员可以既可以在构造函数函数体中赋值，又可以列表初始化赋值
      
      void A::print() {
        cout << a << ":" << b <<endl;
      }
      
      int main() {
      //建立对象a和b，并以100和0作为初值，分别调用构造函数，
      //通过构造函数的初始化列表给对象的常数据成员赋初值
        A a1(100), a2(0);
          
        a1.print();
        a2.print();
        return 0;
      }
      //运行结果
      //100:10
      //0:10
      ```

      

    

-  **常引用**：被引用的对象不能被更新。

  - 如果在声明引用时用**const**修饰，被声明的引用就是常引用。

  - 常引用所引用的对象不能被更新。

  -  如果用常引用做形参，便**不会意外地发生对实参的更改**。常引用的声明形式如下：

     const  类型说明符  &引用名;

  - ```c++
    #include <iostream>
    #include <cmath>
    using namespace std;
    
    class Point { //Point类定义
    public:          //外部接口
           Point(int x = 0, int y = 0): x(x), y(y) { }
           int getX() { return x; }
           int getY() { return y; }
           friend float dist(const Point &p1,const Point &p2);//将对象p1 p2传进去后一定不会被修改，若修改，编译器会报错
    
    private:         //私有数据成员
           int x, y;
    };
    
    float dist(const Point &p1, const Point &p2) {
           double x = p1.x - p2.x; 
           double y = p1.y - p2.y;
           return static_cast<float>(sqrt(x*x+y*y));
    }
    
    int main() {  //主函数
           const Point myp1(1, 1), myp2(4, 5);    
           cout << "The distance is: ";
           cout << dist(myp1, myp2) << endl;
           return 0;
    }
    ```

       

-  **常数组**：数组元素不能被更新(详见第6章)。

  - ​    *类型说明符  const  数组名[大小]...*

-  **常指针**：指向常量的指针(详见第6章)。

**关于const引用**

- 第一个例子中，我们说，fun(int i)和fun(const int i)是一样的，是因为函数调用中存在实参和形参的结合。比如我们用的实参是int a，那么这两个函数都不会改变a的值，这两个函数对于a来说是没有任何区别的，所以不能通过编译，会提示重定义。
  - **fun(int i)和fun(const int i)不允许同时存在重载**，因为调用时无法区分应该调用哪个函数。但是两者还是不一样的，一个i是变量，一个i是常量
- 好了，那 fun(char *a)和fun(const char *a)是一样的吗？答案是：不一样。因为char *a 中a指向的是一个字符串变量，而const char *a指向的是一个字符串常量，所以当参数为字符串常量时，调用第二个函数，而当函数是字符串变量时，调用第一个函数。
  -  **fun(char *a)和fun(const char *a)允许重载**
  - 实参是字符串变量，那么会调用 fun(char *a)。实参是字符串常量，会调用fun(const char *a)。char * a说明a是个char指针，且是变量。const char * a说明，a指向的是const char，不允许通过指针修改字符串，也就是指向了字符串常量。
- 但是char *a和char * const a，这两个都是指向字符串变量，不同的是char *a是指针变量 而char *const a是指针常量，这就和int i和const int i的关系一样了，所以也会提示重定义。
  - 所以**fun(char* a)和fun(char* const a)不允许同时存在重载**，因为调用时无法区分。但是两者还是不一样的，char* a说明a是个char指针，且是变量。char * const a说明，a 是个char指针，且是常量，不允许修改指向。
- 最后说一下，对于引用，比如**int &i 和const int & i 也是可以重载的**，原因是第一个i引用的是一个变量，而第二个i引用的是一个常量，两者是不一样的，**类似于上面的指向变量的指针的指向常量的指针。重载调用时调用哪个，就看实参是变量还是常量了**

![tv1ub6.png](https://s1.ax1x.com/2020/06/13/tv1ub6.png)

```c++
#include<iostream>  
using namespace std;  
   
void fun(const int &i)  
{  
    cout << "fun(const int &) called  "<<endl;  
}  
void fun(int &i)  
{  
    cout << "fun(int &) called "<<endl ;  
}  
int main()  
{  
    const int i = 10;//  
    fun(i);//实参是const是常量，那么调用fun(const int &i)  
    return 0;  
}
```




### 6.多文件结构和预编译命令

#### C++程序的一般组织结构

-  一个工程可以划分为多个源文件：
  -   类声明文件（.h文件）
  -   类实现文件（.cpp文件）
  -   类的使用文件（main()所在的.cpp文件）
-  利用工程来组合各个文件。

```c++
//文件1，类的定义，Point.h
class Point { //类的定义
public:          //外部接口
       Point(int x = 0, int y = 0) : x(x), y(y) { count++; }
       Point(const Point &p);
       ~Point() { count--; }
       int getX() const { return x; }
       int getY() const { return y; }
       static void showCount();          //静态函数成员
private:         //私有数据成员
       int x, y;
       static int count; //静态数据成员
};
```

```c++
//文件2，类的实现，Point.cpp
#include "Point.h"//双引号引起来的会首先在当前工作目录下找这个文件，如果找不到，才会到系统约定的目录下找
#include <iostream>//系统自带的头文件不带“.h"后缀，用尖括号引起来，会到安装时的默认目录下寻找
using namespace std;
int Point::count = 0;            //使用类名初始化静态数据成员
Point::Point(const Point &p) : x(p.x), y(p.y) {
       count++;
}
void Point::showCount() {
       cout << "  Object count = " << count << endl;
}
```

```c++
//文件3，主函数，5_10.cpp
#include "Point.h"
#include <iostream>
using namespace std;
int main() {
       Point a(4, 5);      //定义对象a，其构造函数使count增1
       cout <<"Point A: "<<a.getX()<<", "<<a.getY();
       Point::showCount();      //输出对象个数
       Point b(a);         //定义对象b，其构造函数回使count增1
       cout <<"Point B: "<<b.getX()<<", "<<b.getY();
       Point::showCount();      //输出对象个数
       return 0;
}
```

![image.png](https://i.loli.net/2020/01/26/NFaPCuxBtnkSDhs.png)

#### 外部变量

-  如果**一个变量**除了在定义它的源文件中可以使用外，还**能被其它文件使用**，那么就称**这个变量是外部变量**。
-  **文件作用域中定义的变量，默认情况下都是外部变量**，但在其它文件中如果需要使用这一变量，**需要用extern关键字加以声明（意思是这是别的文件中定义的变量）**。

#### 外部函数

-  在所有类之外声明的函数（也就是非成员函数），都是具有文件作用域的。
-  这样的函数都可以在不同的编译单元中被调用，只要在调用之前进行引用性声明（即声明函数原型）即可。也可以在声明函数原型或定义函数时**用extern修饰，其效果与不加修饰的默认状态是一样的（外部变量不行，声明外部变量一定要extern关键字）**。

#### 将变量和函数限制在编译单元内

- 有时候并不希望当前文件中定义的这些标识符，能拿到其他文件中使用，所以可以使用匿名命名空间

- 使用匿名的命名空间：在匿名命名空间中定义的变量和函数，都不会暴露给其它的编译单元。

- ```C++
  namespace {     //匿名的命名空间
       int n;
       void f() 
       { n++;}
     }
  ```

-  这里被“namespace { …… }”括起的区域都属于匿名的命名空间。

#### 标准c++库

标准C++类库是一个极为灵活并可扩展的可重用软件模块的集合。标准C++类与组件在逻辑上分为6种类型：

-   输入/输出类
-   容器类与抽象数据类型
-   存储管理类
-   算法
-   错误处理
-   运行环境支持

#### 编译预处理

-  #include 包含指令
  -   将一个源文件嵌入到当前源文件中该点处。
  -   #include<文件名> 
    -  按标准方式搜索，文件位于C++系统目录的include子目录下
  -   #include"文件名"
    -  首先在当前目录中搜索，若没有，再按标准方式搜索。

-  #define 宏定义指令
  -   **定义符号常量**，很多情况下**已被const定义语句取代**。
  -   **定义带参数宏**，**已被内联函数取代**。

-  #undef
  -   删除由#define定义的宏，使之不再起作用。

- 条件编译指令——#if 和 #endif

```c++
#if 常量表达式
 //当“ 常量表达式”非零时编译
   程序正文 
#endif
......
```

- 条件编译指令——#else

```c++
 #if  常量表达式
   //当“ 常量表达式”非零时编译
    程序正文1
#else
 //当“ 常量表达式”为零时编译
    程序正文2
#endif
```

- 条件编译指令——#elif

```c++
#if 常量表达式1
  程序正文1 //当“ 常量表达式1”非零时编译
#elif 常量表达式2
  程序正文2 //当“ 常量表达式2”非零时编译
#else
  程序正文3 //其他情况下编译
#endif
```

- 条件编译指令——**#ifdef**

```c++
#ifdef 标识符
  程序段1
#else
  程序段2
#endif
```

 如果“标识符”经#defined定义过，且未经undef删除，则编译程序段1； 否则编译程序段2。



- 条件编译指令——**#ifndef** 标识符

```c++
#ifnef 标识符
程序段1
#else
  程序段2
#endif
```

 如果“标识符”没有被定义过，则编译程序段1,同时呢把这个标识符定义一下； 否则编译程序段2。

```C++
#ifndef HEADER_FILE
#define HEADER_FILE

the entire header file file

#endif
```

如果这个程序被多次包含，当第二次包含的时候，它就会发现标识符已经被定义了，那么重复的函数段呢就不会被编译，这样就可以避免多次include同一个头文件的时候，造成某些类、函数、常量等重复定义的问题。

## 第6章 数组、指针与字符串

### 1.数组的存储与初始化

二维数组**按行存放**，a[0]   a[1]  a[2] 同时又是每一行的首地址

![image.png](https://i.loli.net/2020/01/27/3zxGUBD4kdlsrbt.png)

- 将所有初值写在一个{}内，按顺序初始化
  - 例如：`static int a[3][4]={1,2,3,4,5,6,7,8,9,10,11,12}`
- 分行列出二维数组元素的初值
  - 例如：`static int a[3][4]={{1,2,3,4},{5,6,7,8},{9,10,11,12}};`
- 可以只对部分元素初始化
  - 例如：`static int a[3][4]={{1},{0,6},{0,0,11}};`
- 列出全部初始值时，第1维下标个数可以省略
  - 例如：`static int a[][4]={1,2,3,4,5,6,7,8,9,10,11,12};`
  - 或：`static int a[][4]={{1,2,3,4},{5,6,7,8},{9,10,11,12}};`
- 注意：
  - 如果不作任何初始化，局部作用域的非静态数组中会存在垃圾数据，**static数组中的数据默认初始化为0**；
  - 如果**只对部分元素初始化，剩下的未显式初始化的元素，将自动被初始化为零；**

### 2.数组作为函数的参数

- 数组元素作实参，与单个变量一样。

-  数组名作参数，形、实参数都应是数组名（实质上是地址，关于地址详见6.2），类型要一样，传送的是数组首地址。对形参数组的改变会直接影响到实参数组。

```c++
#include <iostream>
using namespace std;
void rowSum(int a[][4], int nRow) {
	for (int i = 0; i < nRow; i++) {
		for (int j = 1; j < 4; j++)
			a[i][0] += a[i][j];
	}
}

int main() {   //主函数
	 //定义并初始化数组
	int table[3][4] = { {1, 2, 3, 4}, {2, 3, 4, 5}, {3, 4, 5, 6} };
	//输出数组元素
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 4; j++)
			cout << table[i][j] << "   ";
		cout << endl;
	}
	rowSum(table, 3);     //调用子函数，计算各行和
 //输出计算结果
	for (int i = 0; i < 3; i++)
		cout << "Sum of row " << i << " is " << table[i][0] << endl;
	return 0;
}
```

### 3.对象数组

#### 对象数组的定义与访问

- 定义对象数组
  - 类名 数组名[元素个数]；

- 访问对象数组元素
  - 通过下标访问
  - 数组名[下标].成员名

#### 对象数组初始化

- 数组中每一个元素对象被创建时，系统都会调用**类构造函数初始化该对象**。
- 通过初始化列表赋值。
  - 例：Point a[2]={Point(1,2),Point(3,4)};

- 如果没有为数组元素指定显式初始值，数组元素便使用默认值初始化（调用默认构造函数）。

#### 数组元素所属类的构造函数

- 元素所属的类不声明构造函数，则采用默认构造函数。
- 各元素对象的初值要求为相同的值时，可以声明具有默认形参值的构造函数。
- 各元素对象的初值要求为不同的值时，需要声明带形参的构造函数。
- 当数组中每一个对象被删除时，系统都要调用一次析构函数。

```c++
//Point.h
#ifndef _POINT_H
#define _POINT_H
class Point { //类的定义
public: //外部接口
 Point();
Point(int x, int y);
~Point();
void move(int newX,int newY);
int getX() const { return x; }
int getY() const { return y; }
static void showCount(); //静态函数成员
private: //私有数据成员
int x, y;
};
#endif //_POINT_H
```

```c++
//Point.cpp
#include <iostream>
#include "Point.h"
using namespace std;
Point::Point() : x(0), y(0) {
cout << "Default Constructor called." << endl;
}
Point::Point(int x, int y) : x(x), y(y) {
cout << "Constructor called." << endl;
}
Point::~Point() {
cout << "Destructor called." << endl;
}
void Point::move(int newX,int newY) {
cout << "Moving the point to (" << newX << ", " << newY << ")" << endl;
x = newX;
y = newY;
}
```

```c++
#include "Point.h"
#include <iostream>
using namespace std;
int main() {
cout << "Entering main..." << endl;
Point a[2];
for(int i = 0; i < 2; i++)
 a[i].move(i + 10, i + 20);
cout << "Exiting main..." << endl;
return 0;

}
```

### 4.基于范围的for循环

```c++
//原版
int main()
{
    int array[3] = {1,2,3};
    int *p;
    for(p = array; p < array + sizeof(array) / sizeof(int); ++p)
    {
        *p += 2;
        std::cout << *p << std::endl;
    }
    return 0;
}

//基于范围的for循环
int main()
{
    int array[3] = {1,2,3};
    for(int & e : array) //加了&符号就可以修改里边的值了
    {
        e += 2;
        std::cout<<e<<std::endl;
    }
   return 0;
}
```

### 5.指针的概念、定义和指针运算

#### 内存空间的访问方式

- **通过变量名访问（在这个标识符的作用域范围内，就可以用变量名来访问内存空间了）**
- **通过地址访问**

#### 指针的概念（地址类型的变量）

- 指针：内存地址，用于间接访问内存单元
- 指针变量：用于存放地址的变量

“*”运算符：寻址。以ptr的内容为地址，找到这个地址的内容

“&”运算符：取地址。取变量i的地址。

```c++
#include<iostream>
using namespace std;
int main()
{
	static int i;
	cout << i << endl;
	static int* ptr = &i;//取i的地址，作为指针变量ptr的内容
	*ptr = 3;//以Ptr的内容为地址，寻找这个地址的内容
	cout << i << endl;
	return 0;
}
```

### 6.指针的初始化和赋值

#### 指针变量的初始化

- 语法形式

  存储类型 数据类型 *指针名＝初始地址；

- 例：

`int* ptr_a = &a;`

- 注意事项
  - 用变量地址（比如&a）作为初值时，该变量必须在指针初始化之前已声明过，且变量类型应与指针类型一致。
  - 可以用一个已有合法值的指针去初始化另一个指针变量。
  - 不要用一个内部非静态变量去初始化 static 指针。

#### 指针变量的赋值运算

- 语法形式

  - 指针名=地址

- 注意：

  - “地址”中存放的数据类型与指针类型必须相符
  - 向指针变量赋的值必须是地址常量或变量，不能是普通整数，

- 例如：

  - 通过地址运算“&”求得已定义的变量和对象的起始地址
  - 动态内存分配成功时返回的地址

- 例外：**整数0可以赋给指针，表示空指针**。

- 允许定义或声明指向 void 类型的指针。该指针可以被赋予任何类型对象的地址。

  - 例： **void* general;**

  - ```c++
    //void指针。注意，只可以定义void指针，不可以定义void变量
    #include <iostream>
    using namespace std;
    
    int main() {
    void* pv; //对，可以声明void类型的指针
    //pv是确定的，它就是一个指针，只是没有说明它所指的对象的类型，也就是它所指向的对象是不确定的，没有定义。像这样的指针就只能存储地址，但是不能通过这样的指针去访问它所指向的对象，因为不知道它指向什么。  
    
    int i = 5;
    pv = &i; //void类型指针指向整型变量
    int *pint = static_cast<int *>(pv); //void指针转换为int指针
    cout << "*pint = " << *pint << endl;
    return 0;
    }
    ```

#### 指针空值nullptr

- 以往用0或者NULL去表达空指针的问题：
  - C/C++的NULL宏是个被有很多潜在BUG的宏。因为有的库把其定义成整数0，有的定义成 (void*)0。在C的时代还好。但是在C++的时代，这就会引发很多问题。
- C++11使用**nullptr关键字**，是表达更准确，类型安全的空指针

#### 指向常量的指针（const int*）

- const指针，即这个指针指向的对象是一个常量

- **不能通过指向常量的指针改变所指对象的值**，**但指针本身可以改变，可以指向另外的对象**。

- ```c++
  int a;
  const int* p1 = &a; //p1是指向常量的指针
  int b;
  p1 = &b; //正确，p1本身的值可以改变
  *p1 = 1; //编译时出错，不能通过p1改变所指的对象
  ```

#### 指针类型的常量(int* const)

- 若声明**指针常量，则指针本身的值不能被改变。**
- 例

```c++
int a;
int * const p2 = &a;
p2 = &b; //错误，p2是指针常量，值不能改变
```

### 7.指针的算术运算、关系运算

#### 指针的算术运算

- 指针p加上或减去n
  - 其意义是指针当前指向位置的前方或后方第n个数据的起始位置。（加减了n*sizeof(类型) byte)
- 指针的++、--运算
  - 意义是指向下一个或前一个完整数据的起始。
- 运算的结果值取决于指针指向的数据类型，总是指向一个完整数据的起始位置。
- **当指针指向连续存储的同类型数据时，指针与整数的加减运和自增自减算才有意义。**

![image.png](https://i.loli.net/2020/01/29/l5AiRHnX349SWOt.png)

#### 指针类型的关系运算（相等或者不等）

- 指向相同类型数据的指针之间可以进行各种关系运算。

- 指向不同数据类型的指针，以及指针与一般整数变量之间的关系运算是无意义的。

- 指针可以和零之间进行等于或不等于的关系运算。

  例如：p==0或p!=0 

### 8.指针与数组

#### 定义指向数组元素的指针

- 定义与赋值

- 例：int a[10],*pa;

- pa=&a[0];或 pa=a；

- 等效的形式

  - ```c++
    *pa就是a[0]，*(pa+1)就是a[1]，... ，*(pa+i)就是a[i].
    a[i], *(pa+i), *(a+i), pa[i]都是等效的。
    ```

- 注意：

  - 不能写 a++，因为a是数组首地址、是常量。

- ```c++
  //使用数组名和下标访问数组元素
  #include 
  using namespace std;
  int main() {
  	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
  	for (int i = 0; i < 10; i++)
  		cout << a[i] << "  ";
  	cout << endl;
  	return 0;
  }
  
  //使用数组名和指针运算访问数组元素
  #include 
  using namespace std;
  int main() {
  	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
  	for (int i = 0; i < 10; i++)
  		cout << *(a+i) << "  ";
  	cout << endl;
  	return 0;
  }
  
  //使用指针变量访问数组元素
  #include 
  using namespace std;
  int main() {
  	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
  	for (int *p = a; p < (a + 10); p++)
  		cout << *p << "  ";
  	cout << endl;
  	return 0;
  }
  ```

#### 指针数组

即数组的元素是指针型。

#### 指针数组与二维数组

- 二维数组的每一行都是固定长度的。比如`int array[2][3]`,则是两行三列。
- 但是`int* array[2]`，就只是代表里边存了两个指针，每个指针如果索引一个一位数组的话，那他们长度可以不一致。
- 二维数组所有元素是物理（内存）相邻的，但是指针数组可不是。

```c++
//例：
Point* pa[2];
//由pa[0]和pa[1]两个指针组成
```

```c++
#include 
using namespace std;
int main() {
	int line1[] = { 1, 0, 0 };	//矩阵的第一行
	int line2[] = { 0, 1, 0 };	//矩阵的第二行

	
	//定义整型指针数组并初始化
	int* pLine[] = { line1, line2 };//PLine是一个数组，它里边的元素都是int* 类型
    
    //int a[2][3]={1,0,0,0,1,0};
    //int (*pLine)[3]=a; //与上边效果一样的
    
	cout << "Matrix test:" << endl;
  //输出矩阵
	for (int i = 0; i < 2; i++) {
	  for (int j = 0; j < 3; j++)
	    cout << pLine[i][j] << " ";
    cout << endl;
	}
	return 0;
}
输出结果为：
Matrix test:
1,0,0
0,1,0
```

### 9.指针与函数

#### 以指针作为函数参数

为什么需要指针作为参数？

- 需要数据双向传递时（**引用也可以达到此效果**）

- 需要传递一组数据，只传首地址运行效率比较高

- 实参是数组名时，形参可以是指针

```C++
//读入三个浮点数，将整数部分和小数部分分别输出

#include <iostream>
using namespace std;
void splitFloat(float x, int *intPart, float *fracPart) {
   *intPart = static_cast<int>(x); //取x的整数部分
   *fracPart = x - *intPart; //取x的小数部分
}

int main() {
  cout << "Enter 3 float point numbers:" << endl;
  for(int i = 0; i < 3; i++) {
    float x, f;
    int n;
    cin >> x;
    splitFloat(x, &n, &f);	//变量地址作为实参
    cout << "Integer Part = " << n << " Fraction Part = " << f << endl;
  }
  return 0;
}
```

```c++
//指向常量的指针做形参
//

#include <iostream>
using namespace std;
const int N = 6;
void print(const int *p, int n);
int main() {
  int array[N];
  for (int i = 0; i < N; i++)
        cin>>array[i];
  print(array, N);
  return 0;
}
void print(const int *p, int n) {
  cout << "{ " << *p;
  for (int i = 1; i < n; i++)
        cout << ", " << *(p+i);
  cout << " }" << endl;
}
```

**& 在引用与指针中的区别：**

```c++
void function(int& a);//引用
//调用时
int x=0;
function(x);
```

```c++
void function(int* p);//指针
//调用时
int x=0;
function(&x)//取地址符
```

- **&跟类型(例如和`int`)在一起时，是引用；**
- **&跟变量(例如和x)在一起时，是取地址**。

#### 指针类型的函数

函数的返回值是指针。

注意：

- **不要将非静态局部地址，用作函数的返回值**。（因为这个局部变量离开函数之后它就失效了，那个地址就是无效地址了）

  - **错误的例子**：在子函数中定义**局部变量**后**将其地址返回给主函数，就是非法地址**

  - ```c++
    int* function();//函数声明
    int main(){
        int* ptr= function();
        *prt=5; //危险的访问！
        return 0;
    }
    int* function(){//函数实现
        int local=0; //非静态局部变量作用域和寿命都仅限于本函数体内
        return &local;
    }//函数运行结束时，变量local被释
    ```

    

- 返回的指针要确保在主调函数中是有效、合法的地址
  - 正确的例子：主函数中定义的数组，在子函数中对该数组元素进行某种操作后，返回其中一个元素的地址，这就是合法有效的地址

  - ```c++
    #include
    using namespace std;
    int main(){
        int array[10]; //主函数中定义的数组
        int* search(int* a, int num);
        for(int i=0; i<10; i++)
          cin>>array[i];
        int* zeroptr= search(array, 10);  //将主函数中数组的首地址传给子函数
        return 0;
    }
    int* search(int* a, int num){ //指针a指向主函数中定义的数组
        for(int i=0; i<num; i++)
          if(a[i]==0)
            return &a[i]; //返回的地址指向的元素是在主函数中定义的
    }//函数运行结束时，a[i]的地址仍有
    ```

    

  - 正确的例子：在子函数中通过动态内存分配new操作取得的内存地址返回给主函数是合法有效的，但是内存分配和释放不在同一级别，要注意不能忘记释放，避免内存泄漏

  - ```c++
    #include
    using namespace std;
    int main(){
        int* newintvar();//函数声明
        int* intptr= newintvar();
        *intptr=5; //访问的是合法有效的地址
        delete intptr; //如果忘记在这里释放，会造成内存泄漏
        return 0;
    }
    int* newintvar (){ 
        int* p=new int();
        return p; //返回的地址指向的是动态分配的空间
    }//函数运行结束时，p中的地址仍有效
    ```

#### 指向函数的指针

函数返回类型 （*函数指针名）（形参列表）；

含义：函数指针指向的是函数程序代码存储区的起始位置。

**函数指针的典型用途——实现函数回调**

- 通过函数指针调用的函数
  - 例如：将函数的指针作为参数传递给一个函数，使得在处理相似事件的时候可以灵活的使用不同的方法。
- 调用者不关心谁是被调用者
  - 需知道存在一个具有特定原型和限制条件的被调用函数

```c++
//函数回调
#include<iostream>
using namespace std;

int compute(int a,int b,int(*func)(int,int))
{	return func(a, b);}

int max(int a,int b)//求最大值
{	return ((a > b) ? a : b);}

int min(int a, int b) //求最小值
{return ((a < b) ? a : b);}

int sum(int a,int b)//求和
{return a + b;}

int main()
{
	int a, b, res;
	cout << "输入a:"; cin >> a;
	cout << "输入b:"; cin >> b;

	res = compute(a, b, &max);//不加&也行
	cout << "Max of" << a << "and" << b << "is:" << res << endl;
	
	res = compute(a, b, &min);
	cout << "Min of" << a << "and" << b << "is:" << res << endl;
	
	res = compute(a, b, &sum);
	cout << "Sum of" << a << "and" << b << "is:" << res << endl;
}
```

### 10.对象指针

#### 对象指针的定义形式

类名*  对象指针名；

```c++
例:
Point a(5,10);
Piont* ptr;
ptr=&a;
```

#### 通过指针访问对象成员

对象指针名->成员名

例：ptr->getx() 相当于 (*ptr).getx();

```c++
#include <iostream>
using namespace std;

class Point {
public:
Point(int x = 0, int y = 0) : x(x), y(y) { }
int getX() const { return x; }
int getY() const { return y; }
private:
int x, y;
};

int main() {
Point a(4, 5);
Point *p1 = &a; //定义对象指针，用a的地址初始化
cout << p1->getX() << endl;//用指针访问对象成员
cout << a.getX() << endl; //用对象名访问对象成员
return 0;
}
```

#### this指针

- 指向当前对象自己
- **隐含于类的每一个非静态成员函数中**。
- 指出成员函数所操作的对象。
  - 当通过一个对象调用成员函数时，系统先将该对象的地址赋给this指针，然后调用成员函数，成员函数对对象的数据成员进行操作时，就隐含使用了this指针。
- 例如：Point类的getX函数中的语句：
  - return x;
  - 相当于：return this->x;

```C++
//曾经出现过的错误例子
class Fred; //前向引用声明
class Barney {
Fred x; //错误：类Fred的声明尚不完善
};
class Fred {
Barney y;
};

//正确的程序
class Fred; //前向引用声明
class Barney {
Fred *x;
};

class Fred {
Barney y;
};
```

### 11.动态内存分配

#### 动态申请内存操作符 new

- new 类型名T（初始化参数列表）
- 功能：在程序执行期间，申请用于存放T类型对象的内存空间，并依初值列表赋以初值。
- 结果值：成功：T类型的指针，指向新分配的内存；失败：抛出异常。

#### 释放内存操作符delete

- delete 指针p
- 功能：释放指针p所指向的内存。p必须是new操作的返回值。

```c++
#include <iostream>
using namespace std;

class Point {
public:
Point() : x(0), y(0) {
cout<<"Default Constructor called."<<endl;
}

Point(int x, int y) : x(x), y(y) {
cout<< "Constructor called."<<endl;
}

~Point() { cout<<"Destructor called."<<endl; }
int getX() const { return x; }
int getY() const { return y; }
void move(int newX, int newY) {
x = newX;
y = newY;
}

private:
int x, y;
};

int main() {
cout << "Step one: " << endl;
Point* ptr1 = new Point; //调用默认构造函数
delete ptr1; //删除对象，自动调用析构函数。删除的是ptr1的内容所代表位置的内存，而不是删除掉了这个指针。
cout << "Step two: " << endl;
ptr1 = new Point(1,2);
delete ptr1;
return 0;
}
运行结果：

Step One:
Default Constructor called.
Destructor called.
Step Two:
Constructor called.
Destructor called.
```

### 12.申请和释放动态数组

#### 分配和释放动态数组

- 分配：new 类型名T [数组长度]
  - 数组长度可以是任何整数类型表达式。
- 释放：delete[] 数组名p
  - 释放指针p所指向的数组。
  - p必须是用new分配得到的数组首地址

```c++
//动态创建对象数组举例
#include<iostream>
using namespace std;

class Point{
}

int main()
{
    Point* ptr=new Point[2];
    ptr[0].move(5,10);
    ptr[1].move(15,20);
    cout<<"Deleting..."<<endl;
    delete[] ptr;
    return 0;
}
运行结果：
Default Constructor called.
Default Constructor called.
Deleting
Destructor called.
Destructor called.
```

#### 动态创建多维数组

`new 类型名T[第1维长度][第2维长度]`

- 如果内存申请成功，new运算返回一个指向新分配内存首地址的指针
- 例如：
  - `char (*fp)[3];`
  - `fp=new char[2][3];`

注意： `char(*fp)[3]  和 char* fp[3]`的不同。 不带括号是说它有3个元素，带括号是说它要按每3个3个去切分。

fp就是二维数组的首地址。

![image.png](https://i.loli.net/2020/01/29/Mpicql86keLaOTB.png)

```c++
//指针数组
int a1[] = { 1, 0, 0 };	//矩阵的第一行
int a2[] = { 0, 1, 0 };	//矩阵的第二行
int* fp[2] = { a1, a2 };//PLine是一个数组，它里边的元素都是int* 类型
    
//也是指针数组
int a[2][3]={1,0,0,0,1,0};
int (*fp)[3]=a; //与上边效果一样的

差别就在：
int *fp[2]//它是说fp是一个2个元素的数组，每个元素都是int*类型
int (*fp)[3]//它是说fp是一个2维数组（因为只有一个[]），按每3个元素每3个元素的去切割
int (*cp)[9][8]//它是说cp是一个3维数组（因为有2个[]），按[9][8]的样式去切割 
```

```c++
//例子
#include <iostream>
using namespace std;
int main() {
int(*cp)[9][8] = new int[7][9][8];
for (int i = 0; i < 7; i++)
	for (int j = 0; j < 9; j++)
		for (int k = 0; k < 8; k++)
			* (*(*(cp + i) + j) + k) = (i * 100 + j * 10 + k);

for (int i = 0; i < 7; i++) {
	for (int j = 0; j < 9; j++) {
		for (int k = 0; k < 8; k++)
			cout << cp[i][j][k] << " ";
		cout << endl;
}
	cout << endl;
}
delete[] cp;
return 0;
}
```

#### 例子:动态数组类

```c++
#include <iostream>
#include <cassert>
using namespace std;
class Point {
public:
	Point() : x(0), y(0) {
		cout << "Default Constructor called." << endl;
	}

	Point(int x, int y) : x(x), y(y) {
		cout << "Constructor called." << endl;
	}

	~Point() { cout << "Destructor called." << endl; }
	int getX() const { return x; }
	int getY() const { return y; }
	void move(int newX, int newY) {
		x = newX;
		y = newY;
	}

private:
	int x, y;
};

class ArrayOfPoints { //动态数组类
public:
	ArrayOfPoints(int size) : size(size) {
		points = new Point[size];
	}
	~ArrayOfPoints() {
		cout << "Deleting..." << endl;
		delete[] points;
	}

	Point& element(int index) {
		assert(index >= 0 && index < size);
		return points[index];
	}//返回引用

private:
	Point* points; //指向动态数组首地址
	int size; //数组大小
};

int main() {
	int count;
	cout << "Please enter the count of points: ";
	cin >> count;
	ArrayOfPoints points(count); //创建数组对象
	points.element(0).move(5, 0); //访问数组元素的成员
	points.element(1).move(15, 20); //访问数组元素的成员
	return 0;
}

运行结果：
Please enter the number of points:2
Default Constructor called.
Default Constructor called.
Deleting...
Destructor called.
Destructor called.
```

element函数返回“引用”可以用来操作封装数组对象内部的数组元素。如果返回“值”则只是返回了一个“副本”，通过“副本”是无法操作原来数组中的元素的。返回引用

### 13.智能指针

- new delete这样动态分配与回收，就是显示管理内存。显式管理内存在是能上有优势，但容易出错。
- C++11提供智能指针的数据类型，对垃圾回收技术提供了一些支持，实现一定程度的内存管理。

#### C++11的智能指针

- **unique_ptr** ：不允许多个指针共享资源，可以用标准库中的move函数转移指针（这个指针所指向的空间，只能由它所指向。它里边存放的地址不许复制到别的指针去。但是可以用move函数把它所指向的对象转移给别的指针，也就是把它里边存的地址转移到别的指针中去，一旦转移到别的指针中去，原来的这个unique_ptr指针就失效了，不能再用了）
- **shared_ptr** ：多个指针共享资源（多个shared_ptr类型的指针可以指向同一个内存单元）
- weak_ptr ：可复制shared_ptr，但其构造或者释放对资源不产生影响

### 14.vector对象

#### 为什么需要vector？

- 封装任何类型的动态数组，自动创建和删除。
- 数组下标越界检查。
- 例6-18中封装的ArrayOfPoints也提供了类似功能，但只适用于一种类型的数组。

#### vector对象的定义

- vector<元素类型> 数组对象名(数组长度);
- 例：`vector<int> arr(5)`
  建立大小为5的int数组

#### vector对象的使用

- 对数组元素的引用
  - 与普通数组具有相同形式：

vector对象名 [ 下标表达式 ]

- vector数组对象名不表示数组首地址
  - 获得数组长度
  - 用size函数
    - 数组对象名.size()

#### 例子

```c++
#include <iostream>
#include <vector>
using namespace std;
//计算数组arr中元素的平均值

double average(const vector<double> &arr)
{
    double sum = 0;
    for (unsigned i = 0; i<arr.size(); i++)
    sum += arr[i];
    return sum / arr.size();
}

int main() {
    unsigned n;
    cout << "n = ";
    cin >> n;

    vector<double> arr(n); //创建数组对象
    cout << "Please input " << n << " real numbers:" << endl;

    for (unsigned i = 0; i < n; i++)
        cin >> arr[i];

    cout << "Average = " << average(arr) << endl;
    return 0;

}
```

#### 基于范围的for循环配合auto举例

```c++
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> v = {1,2,3};
    for(auto i = v.begin(); i != v.end(); ++i)
   		std::cout << *i << std::endl;
    for(auto e : v)//基于范围的for循环
    	std::cout << e << std::endl;
    return 0;
}
```

### 15.深层复制与浅层复制

#### 浅层复制与深层复制

- 浅层复制

  - 实现对象间数据元素的一一对应复制。

- 深层复制

  - 当被复制的对象数据成员是指针类型时，不是复制该指针成员本身，而是将指针所指对象进行复制。

  主要注意：假如类的数据成员中有指针，而指针所指向的空间，是在构造对象的时候通过动态分配内存得到的地址。这时候只能用深层复制了。

#### 例子

```c++
//浅层复制
#include <iostream>

#include <cassert>

using namespace std;

class Point {};
class ArrayOfPoints {};

int main() {
    int count;
    cout << "Please enter the count of points: ";
    cin >> count;
    ArrayOfPoints pointsArray1(count); //创建对象数组
    pointsArray1.element(0).move(5,10);
    pointsArray1.element(1).move(15,20);
    ArrayOfPoints pointsArray2(pointsArray1); //创建副本
    cout << "Copy of pointsArray1:" << endl;
    cout << "Point_0 of array2: " << pointsArray2.element(0).getX() << ", "
    << pointsArray2.element(0).getY() << endl;
    cout << "Point_1 of array2: " << pointsArray2.element(1).getX() << ", "
    << pointsArray2.element(1).getY() << endl;
    pointsArray1.element(0).move(25, 30);
    pointsArray1.element(1).move(35, 40);
    cout<<"After the moving of pointsArray1:"<<endl;
    cout << "Point_0 of array2: " << pointsArray2.element(0).getX() << ", "
    << pointsArray2.element(0).getY() << endl;
    cout << "Point_1 of array2: " << pointsArray2.element(1).getX() << ", "
    << pointsArray2.element(1).getY() << endl;
	return 0;
}
运行结果如下：
Please enter the number of points:2
Default Constructor called.
Default Constructor called.
Copy of pointsArray1:
Point_0 of array2: 5, 10
Point_1 of array2: 15, 20
After the moving of pointsArray1:
Point_0 of array2: 25, 30
Point_1 of array2: 35, 40
Deleting...
Destructor called.
Destructor called.
Deleting...
接下来程序出现运行错误。
   因为两个对象里的指针变量是指向同一个地方的，第一个对象析构时，已经delete过一次了。第二个对象析构时，又打算delete一次，所以出现了error
```

```c++
//深层复制
#include <iostream>
#include <cassert>
using namespace std;
class Point { //类的声明同例6-16
};
class ArrayOfPoints {
public:
ArrayOfPoints(const ArrayOfPoints& pointsArray);
//其他成员同例6-18
};
ArrayOfPoints::ArrayOfPoints(const ArrayOfPoints& v) {//自己实现的复制构造函数
size = v.size;
points = new Point[size];
for (int i = 0; i < size; i++)
points[i] = v.points[i];
}

int main() {

//同例6-20

}

程序的运行结果如下：
Please enter the number of points:2
Default Constructor called.
Default Constructor called.
Default Constructor called.
Default Constructor called.
Copy of pointsArray1:
Point_0 of array2: 5, 10
Point_1 of array2: 15, 20
After the moving of pointsArray1:
Point_0 of array2: 5, 10
Point_1 of array2: 15, 20
Deleting...
Destructor called.
Destructor called.
Deleting...
Destructor called.
Destructor called.
```

### 16.移动构造

- C++11标准中提供了一种新的构造方法——移动构造。

- 本质：将即将要消亡的对象，它的一切转移过来，无需复制构造，效率高一些。

- C++11之前，如果要将源对象的状态转移到目标对象只能通过复制。在某些情况下，我们没有必要复制对象——只需要移动它们。

- C++11引入移动语义：

  - 源对象资源的控制权全部交给目标对象

- 移动构造函数

- 什么时候该触发移动构造？

  - 有可被利用的临时对象

- 移动构造函数:

  class_name ( class_name && )

  注意：

  &&是右值引用；

  函数返回的临时变量是右值。（即将消亡的值就是右值）

  ```c++
  //不使用移动构造，使用复制构造
  #include<iostream>
  using namespace std;
  class IntNum {
  public:
  IntNum(int x = 0) : xptr(new int(x)){ //构造函数
  cout << "Calling constructor..." << endl;}
  
  IntNum(const IntNum & n) : xptr(new int(*n.xptr)){//复制构造函数
      cout << "Calling copy constructor..." << endl;
  };
  
  ~IntNum(){ //析构函数
      delete xptr;
      cout << "Destructing..." << endl;
  }
  
  int getInt() { return *xptr;} //成员函数
      
  private:
  int *xptr;//数据成员（是在构造函数中new出来的内存地址）
  };
  
  //返回值为IntNum类对象
  
  IntNum getNum() {
  IntNum a;
  return a;
  }
  
  int main() {
  cout<<getNum().getInt()<<endl;//getNum()函数，进入后，IntNum类的默认构造函数构造对象a，然后复制构造给临时对象（因为这个return回去的没有名字，只是临时对象），然后对象a被析构。然后输出0。
  //上边那句话执行完之后，getNum()那个临时对象被析构。
  return 0;
  }
  
  运行结果：
  Calling constructor...//getNum()函数中构造对象a
  Calling copy constructor...//复制构造给return对象（这里是临时对象，因为主函数中的对象是临时对象）
  Destructing...//对象a析构
  0//主函数的cout语句
  Destructing...//主函数中的临时对象析构
  ```

  ```C++
  //定义了移动构造之后
  #include<iostream>
  using namespace std;
  class IntNum {
  public:
      IntNum(int x = 0) : xptr(new int(x)){ //构造函数
      cout << "Calling constructor..." << endl;}
  
      IntNum(const IntNum & n) : xptr(new int(*n.xptr)){//复制构造函数
          cout << "Calling copy constructor..." << endl;
      };
  
      IntNum(IntNum && n): xptr( n.xptr){ //移动构造函数
       n.xptr = nullptr;//这是很有必要，因为之后要析构，同一地址被delete两次会出error，但是如果是空指针，delete将不发生操作
      cout << "Calling move constructor..." << endl;
      }
  
      ~IntNum(){ //析构函数
          delete xptr;
          cout << "Destructing..." << endl;
      }
  
      int getInt() { return *xptr;} //成员函数
      
  private:
      int *xptr;//数据成员（是在构造函数中new出来的内存地址）
  };
  
  //返回值为IntNum类对象
  
  IntNum getNum() {
  IntNum a;
  return a;
  }
  
  int main() {
  cout<<getNum().getInt()<<endl;
  return 0;
  }
  
  //输出：
  Calling constructor...//构造对象a
  Calling move constructor...//采用移动构造给return对象
  Destructing...//对象a析构。（虽然a的东西被移动构造走了，还是要析构的。这种的优势是效率高，相比复制构造）
  0
  Destructing...//主函数临时对象析构
  ```
  
```c++
 //在g++下没有任何优化时是这样，在msvc下可能还不一样
//主函数是不是临时对象不重要。
  int main()
  {
      IntNum y=getNum();
  	cout << y.getInt() << endl;
  	return 0;
      
  }
  //输出：
  Calling constructor...//getNum()函数中构造对象a
  Calling move constructor...//调用移动构造，构造这个return临时对象
  Destructing...//对象a析构
  Calling move constructor...//调用移动构造，从临时对象构造对象y
  Destructing...//临时对象析构
  0
  Destructing...//对象y析构
```



### 17.c风格字符串

#### 字符串常量

- 例："program"
- 各字符连续、顺序存放，每个字符占一个字节，以‘\0’结尾，相当于一个隐含创建的字符常量数组
- “program”出现在表达式中，表示这一char数组的首地址
- 首地址可以赋给char常量指针：
  - const char *STRING1 = "program";

#### 用字符数组存储字符串（C风格字符串）

- 例如

  char str[8] = { 'p', 'r', 'o', 'g', 'r', 'a', 'm', '\0' };

  char str[8] = "program";

  char str[] = "program";

#### 用字符数组表示字符串的缺点

- 执行连接、拷贝、比较等操作，都需要显式调用库函数，很麻烦
- 当字符串长度很不确定时，需要用new动态创建字符数组，最后要用delete释放，很繁琐
- 字符串实际长度大于为它分配的空间时，会产生数组下标越界的错误

### 18.string类

#### string类

- 使用字符串类string表示字符串
- string实际上是对字符数组操作的封装

#### string类常用的构造函数

- string(); //默认构造函数，建立一个长度为0的串
  - 例：string s1;

- string(const char *s); //用指针s所指向的字符串常量初始化string对象
  - 例：string s2 = “abc”;

- string(const string& rhs); //复制构造函数
  - 例：string s3 = s2;

#### string类常用操作

- s + t 将串s和t连接成一个新串
- s = t 用t更新s
- s == t 判断s与t是否相等
- s != t 判断s与t是否不等
- s < t 判断s是否小于t（按字典顺序比较）
- s <= t 判断s是否小于或等于t （按字典顺序比较）
- s > t 判断s是否大于t （按字典顺序比较）
- s >= t 判断s是否大于或等于t （按字典顺序比较）
- s[i] 访问串中下标为i的字符
- 例：
  - string s1 = "abc", s2 = "def";
  - string s3 = s1 + s2; //结果是"abcdef"
  - bool s4 = (s1 < s2); //结果是true
  - char s5 = s2[1]; //结果是'e'

```c++
#include <string>
#include <iostream>
using namespace std;
//根据value的值输出true或false
//title为提示文字
inline void test(const char *title, bool value)
{
cout << title << " returns "
<< (value ? "true" : "false") << endl;
}

int main() {
string s1 = "DEF";
cout << "s1 is " << s1 << endl;
string s2;
cout << "Please enter s2: ";
cin >> s2;
cout << "length of s2: " << s2.length() << endl;
//比较运算符的测试
test("s1 <= \"ABC\"", s1 <= "ABC");
test("\"DEF\" <= s1", "DEF" <= s1);
//连接运算符的测试
s2 += s1;
cout << "s2 = s2 + s1: " << s2 << endl;
cout << "length of s2: " << s2.length() << endl;
return 0;
}
```

#### 如何输入整行字符串？

- 用cin的>>操作符输入字符串，会以空格作为分隔符，空格后的内容会在下一回输入时被读取
- 当我们希望输入一整行字符串，其中包含空格时，用cin就不行了，这时该采用getline

#### 输入整行字符串

- getline可以输入整行字符串（要包string头文件），例如：
  - ```c++
    getline(cin, s2);//第一个cin代表从键盘读入。没有第三个参数，则默认以换行(回车)作为结束符
    ```

    

- 输入字符串时，可以使用其它分隔符作为字符串结束的标志（例如逗号、分号），将分隔符作为getline的第3个参数即可，例如：
  - getline(cin, s2, ',');

  ```c++
  include <iostream>
  #include <string>
  using namespace std;
  
  int main() {
  for (int i = 0; i < 2; i++){
      string city, state;
      getline(cin, city, ',');
      getline(cin, state);
      cout << "City:" << city << “ State:" << state << endl;
  }
  return 0;
  }
  
  运行结果：
  Beijing,China//这是输入
  City: Beijing State: China//显示
  San Francisco,the United States//输入
  City: San Francisco State: the United States//显示
  ```

## 第7章 继承与派生

### 1.继承的基本概念和语法

#### 继承与派生概述

- 继承与派生其实是一回事，只是同一过程从不同的角度看

- - 保持已有类的特性而构造新类的过程称为**继承**
  - 在已有类的基础上新增自己的特性而产生新类的过程称为**派生**。

- 被继承的已有类称为**基类**（或父类）

- 派生出的新类称为**派生类**（或子类）

- 直接参与派生出某类的基类称为**直接基类**

- 基类的基类甚至更高层的基类称为**间接基类**

#### 继承与派生的目的

- 继承的目的：实现设计与代码的重用。
- 派生的目的：当新的问题出现，原有程序无法解决（或不能完全解决）时，需要对原有程序进行改造。

#### 单继承时派生类的定义

单继承就是派生类只从一个直接基类继承

- 语法

```c++
class 派生类名：继承方式 基类名

{

成员声明；

}
```



- 例

```c++
class Derived: public Base

{

public:

Derived ();

~Derived ();

};
```



#### 多继承时派生类的定义

- 语法

```c++
class 派生类名：继承方式1 基类名1，继承方式2 基类名2，...

{

成员声明；

}
```

注意：每一个“继承方式”，只用于限制对紧随其后之基类的继承。

- 例

```c++
class Derived: public Base1, private Base2

{
public:
Derived ();
~Derived ();
};
```

#### 派生类的构成

- 吸收基类成员（继承）
  - 默认情况下，**派生类除了基类的构造函数和析构函数不被继承**，其它所有成员都被继承了
  - 不过C++11规定可以用using语句继承基类构造函数
- 改造基类成员
  - 如果派生类声明了一个和某基类成员同名的新成员，派生的新成员就隐藏或覆盖了外层同名成员。（重新写的就覆盖了继承了的）
- 添加新的成员（派生）
  - 派生类增加新成员使派生类在功能上有所发展

### 2.继承方式

#### 继承方式简介

**不同继承方式的影响主要体现在：**

- 派生类**成员**对基类成员的访问权限
  - 也就是派生类内部成员（主要指函数成员），怎么来访问基类继承过来的成员，能访问哪些，不能访问哪些
- 通过派生类**对象**对基类成员的访问权限
  - 也就是构造出的派生类对象，怎么去调用从基类继承过来的成员函数，去访问基类继承的数据成员。

**三种继承方式**

- 公有继承：基类成员权限不变
  - 在派生类内部：可以访问基类的public和protected成员，不能访问基类Private成员；可以访问派生类新增的所有类型成员。
  - 派生类构造的对象：可以访问基类public成员；可以访问派生类的新增public成员
- 私有继承：基类成员全部变为private
  - 在派生类内部：可以访问基类的public和protected成员，不能访问基类private成员；可以访问派生类新增的所有类型成员。
  - 派生类构造的对象：基类所有成员都不可以访问；可以访问派生类的新增public成员
- 保护继承：基类public和protected成员都变成protected
  - 在在派生类内部：可以访问基类的public和protected成员，不能访问基类private成员；可以访问派生类新增的所有类型成员。
  - 派生类构造的对象：基类所有成员都不可以访问；可以访问派生类的新增public成员

**区别**：不管是哪种继承方式，派生类中**新增成员**可以访问基类的公有成员和保护成员，无法访问私有成员。而**继承方式影响的是**派生类继承成员访问属性，也就是影响**基类继承下来的成员对外的权限（也就是派生类构造出来的对象对成员的访问权限）**。

**protected的作用**：在一个无继承的类中，它对外的作用相当于private。但是一旦当做基类，无论什么继承方式，它对于派生类内部都是可见的，相当于是public(当然无论什么继承方式，对外还是不可见的，也就是派生类构造的对象不可见)

**继承以后：**

- 公有继承：基类成员是public的还是public，protected还是protected，private还是private。这是对外而言，并且影响再次继承
- 私有继承：基类成员不管public还是protected还是private，全部变为private。
- 保护继承：基类成员public和protected成员全部变成

**总结**：

- 对于派生类内部成员：不论是什么继承方式，内部都是使用基类的public和protected。（再加上派生类自己的所有新增成员）
- 对于派生类构造的对象：只有基类的public成员且是public继承，外部对象才可以使用（再加上派生类新增的public）

#### 共有继承（public）：这里的public是继承方式，跟成员权限是两码事

- 继承的访问控制

  - **基类的public和protected成员：访问属性在派生类中保持不变；**
    - 也就是保留基类的公有接口
  - 基类的private成员：不可直接访问。

- 访问权限

  - **派生类中的成员函数：**可以**直接访问基类中的public和protected成员**，但不能直接访问基类的private成员；
    - 这与之前的无继承类有区别，之前的不管是private public protected，凡是自己的不是继承的，在成员函数中都可以直接访问。
  - **通过派生类构造的对象：只能访问public成员。**基类的protected或者派生类的protected也都是无法直接访问的。（public继承方式的作用）

- 公有继承举例

- ```c++
  Point.h
  #ifndef _POINT_H
  #define _POINT_H
  class Point {   
      //基类Point类的定义
      public:     
      //公有函数成员
          void initPoint(float x = 0, float y = 0){ 
              this->x = x; 
              this->y = y;
          }
          void move(float offX, float offY){ 
              x += offX; 
              y += offY;
          }
          float getX() const { return x; }
          float getY() const { return y; }
      private:        
      //私有数据成员
          float x, y;
  };
  #endif //_POINT_H
  ```

  ```c++
  Rectangle.h
  #ifndef _RECTANGLE_H
  #define _RECTANGLE_H
  #include "Point.h"
  class Rectangle: public Point { 
  //派生类定义部分
  public: 
  //新增公有函数成员
      void initRectangle(float x, float y, float w, float h) {
          //基类的x,y是private的，继承类的内部也无法直接访问，可以通过基类提供的公有接口来初始化，比如这个iniPoint这个public成员函数
          initPoint(x, y);              //调用基类公有成员函数
          
          this->w = w;
          this->h = h;
      }
      float getH() const { return h; }
      float getW() const { return w; }
  private:    
  //新增私有数据成员
      float w, h;
  };
  #endif //_RECTANGLE_H
  ```

  ```c++
  main.cpp
  #include <iostream>
  #include <cmath>
  using namespace std;
  #include “Rectangle.h”
  int main() {
      Rectangle rect; //定义Rectangle类的对象
      //设置矩形的数据
      rect.initRectangle(2, 3, 20, 10);   //这是派生类的公有函数
      rect.move(3,2); //移动矩形位置。这是基类的公有函数，派生类对象可以直接调用
      cout << "The data of rect(x,y,w,h): " << endl;
      //输出矩形的特征参数
      cout << rect.getX() <<", "
          << rect.getY() << ", "
          << rect.getW() << ", "
          << rect.getH() << endl;
      return 0;
  }
  ```

  #### 私有继承（private)

  - 继承的访问控制

  - - 基类的**public**和**protected**成员：都以**private**身份出现在派生类中；
      - 基类的所有公有接口都屏蔽了
    - 基类的private成员：**不可直接访问**。

  - 访问权限

  - - **派生类中的成员函数**：可以直接访问基类中的public和protected成员，但不能直接访问基类的private成员；
    - **通过派生类构造的对象**：不能直接访问从基类继承的任何成员。（private继承方式的作用）

  #### 保护继承（protected）

  - 继承的访问控制

  - - 基类的**public**和**protected**成员：都以**protected**身份出现在派生类中；
    - 基类的**private**成员：**不可直接访问**。

  - 访问权限

  - - 派生类中的成员函数：可以直接访问基类中的public和protected成员，但不能直接访问基类的private成员；
    - 通过派生类的对象：不能直接访问从基类继承的任何成员。

  - protected 成员的特点与作用

  - - 对建立其所在类对象的模块来说，它与 private 成员的性质相同。
    - 对于其派生类内部来说，它与 public 成员的性质相同。
    - 既实现了数据隐藏，又方便继承，实现代码重用。
    - 如果派生类有多个基类，也就是多继承时，可以用不同的方式继承每个基类。

例：保护成员的作用（就是继承下去以后，继承类内部成员函数可用，外部对象不可用）

```c++
//保护成员，在类外跟Private一样，不能直接访问。
//即对应“对建立其所在类对象的模块来说，它与 private 成员的性质相同。”
class A {
protected:
    int x;
};

int main() {
    A a;
    a.x = 5;//错误
}

//如果继承到B类（不论什么继承方式），那么B类的成员函数中直接访问基类的保护成员x，就是可以的。但是类外依然不可以，即用对象来访问。
//即对应“对于其派生类来说，它与 public 成员的性质相同。”
class A {
protected:
    int x;
};
class B: public A{
public:
    void function();
};
void B:function() {
    x = 5;   //正确
}
```

**多继承举例**

```c++
class A {
public:
    void setA(int);
    void showA() const;
private:
    int a;
};

class B {
public:
    void setB(int);
    void showB() const;
private:
    int b;
};

class C : public A, private B { //C类以public方式继承了A类，以private方式继承了B类
public:
    void setC(int, int, int);
    void showC() const;
private:
    int c;
};

void  A::setA(int x) {
    a=x; 
}
void B::setB(int x) {
    b=x; 
}
void C::setC(int x, int y, int z) {
    //派生类成员直接访问基类的公有成员和保护成员
    setA(x); 
    setB(y); 
    c = z;
}

//其他函数实现略

int main() {
    C obj;
    obj.setA(5);//基类的公有成员，公有继承
    obj.showA();//基类的公有成员，公有继承
    obj.setC(6,7,9);//派生类的共有成员
    obj.showC();//派生类的公有成员
// obj.setB(6);  错误，因为是基类的公有成员，但是是私有继承
// obj.showB(); 错误
    return 0;
}
```

### 3.基类与派生类类型转换

#### 类型转换

- **公有继承的派生类对象可以被当作基类的对象使用**，反之则不可。
  - **派生类的对象可以隐含转换为基类对象；**
  - **派生类的对象可以初始化基类的引用；**
  - **派生类的指针可以隐含转换为基类的指针**。
- 通过**基类对象名、指针只能使用从基类继承的成员**。派生类新增的成员，就不能使用

类型转换规则举例

注意：不要重新定义继承而来的非虚函数(比如底下的display函数)。底下的例子就是教训。

```c++
#include <iostream>
using namespace std;
class Base1 { //基类Base1定义
public:
    void display() const {
        cout << "Base1::display()" << endl;
    }
};
class Base2: public Base1 { //公有派生类Base2定义
public:
    void display() const {
        cout << "Base2::display()" << endl;
    }
};
class Derived: public Base2 { //公有派生类Derived定义
public:
    void display() const {
        cout << "Derived::display()" << endl;
    }
};
void fun(Base1 *ptr) {  //参数为指向基类对象的指针
    ptr->display();     //"对象指针->成员名"
}
int main() {    //主函数
    Base1 base1;    //声明Base1类对象
    Base2 base2;    //声明Base2类对象
    Derived derived;    //声明Derived类对象

    fun(&base1);    //用Base1对象的指针调用fun函数
    fun(&base2);    //用Base2对象的指针调用fun函数
    fun(&derived); //用Derived对象的指针调用fun函数
    
	//派生类base2 derived的指针可以隐含转换为基类的指针，所以形参Base1 *ptr可以接受这3种调用方式。但是fun函数里的display函数都是base1的display函数，而不是相应的base2 derived各自的display函数，与我们的期望不一致。无法做到运行时绑定。所以不要重新定义继承而来的非虚函数
    return 0;
}
```

### 4.派生类的构造和析构

#### 派生类的构造函数

- 默认情况
  - 基类的构造函数不被继承;
  - 派生类需要定义自己的构造函数。
- C++11规定
  - 可用using语句继承基类构造函数。
  
  - 例如：
  
    - ```c++
      #include <iostream>
      using namespace std;
      
      class Base
      {
      public:
      	Base(int va) :m_value(va), m_c('0') {}
      	Base(char c) :m_c(c), m_value(0) {}
      protected:
      	int m_value;
      	char m_c;
      };
      
      class Derived :public Base
      {
      public:
      	//使用继承构造函数，就不用写派生类Derived的构造函数了
      	using Base::Base;
      
      	//假设派生类只是添加了一个普通的函数
      	void display()
      	{
      		cout << m_value << " " << m_c <<x<< endl;
      		//dosomething		
      	}
      private:
      	int x=0;
      };
      int main()
      {
      	Derived yyd(1);//实际是用的Base(int va) :m_value(va), m_c('0') {}构造函数。派生类新增的成员Int x由类内默认值初始化
      	yyd.display();
      	return 0;
      }
      ```
  
      
  
  - 但是只能初始化从基类继承的成员。也就是说派生类新增的成员就不能够通过构造函数初始化了。适用于派生类新增成员很少的情况
    
    - 派生类新增成员可以通过类内初始值进行初始化。
    
  - 语法形式：
    
    - using B::B;

**建议**

- 如果派生类有自己新增的成员，且需要通过构造函数初始化，则派生类要自定义构造函数。自定义的构造函数要初始化继承来的数据成员和自己新增的数据成员。

**若不继承基类的构造函数**

- 派生类新增成员：派生类定义构造函数初始化；
- 继承来的成员：自动调用基类构造函数进行初始化；
- 派生类的构造函数需要给基类的构造函数传递参数。

**单继承**

派生类只有一个直接基类的情况，是单继承。单继承时，派生类的构造函数只需要给一个直接基类构造函数传递参数。

**单继承时构造函数的定义语法**

```c++
派生类名::派生类名(基类所需的形参，派生类新增成员所需的形参):
基类名(参数表), 本类成员初始化列表
{
	//其他初始化；
}；
```

**单继承时的构造函数举例**

```c++
#include<iostream>
using namespace std;
class B {
public:
    B();
    B(int i);
    ~B();
    void print() const;
private:
    int b;
};

B::B() {
    b=0;
    cout << "B's default constructor called." << endl;
}
B::B(int i) {
    b=i;
    cout << "B's constructor called." << endl;
}
B::~B() {
    cout << "B's destructor called." << endl;
}
void B::print() const {
    cout << b << endl;
}

class C: public B {
public:
    C();
    C(int i, int j);
    ~C();
    void print() const;
private:
    int c;
};
C::C() {
    c = 0;
    cout << "C's default constructor called." << endl;
}
C::C(int i,int j): B(i), c(j){//一个是基类B构造函数，一个是给int c初始化
    cout << "C's constructor called." << endl;
}

C::~C() {
    cout << "C's destructor called." << endl;
}
void C::print() const {
    B::print();
    cout << c << endl;
}

int main() {
    C obj(5, 6);
    obj.print();
    return 0;
}
```

**多继承**

多继承时，有多个直接基类，如果不继承基类的构造函数，派生类构造函数需要给所有基类构造函数传递参数。我们来看一下语法规定

**多继承时构造函数的定义语法**

```c++
派生类名::派生类名(参数表) : 
基类名1(基类1初始化参数表), 
基类名2(基类2初始化参数表), 
...
基类名n(基类n初始化参数表), 
本类成员初始化列表
{
        //其他初始化；
}；
```

**派生类与基类的构造函数**

- 当基类有默认构造函数时
  - 派生类构造函数可以不向基类构造函数传递参数。
  - 构造派生类的对象时，基类的默认构造函数将被调用。
- 如需执行基类中带参数的构造函数
  - 派生类构造函数应为基类构造函数提供参数。

**多继承且有对象成员时派生的构造函数定义语法**

```c++
派生类名::派生类名(形参表):
基类名1(参数), 基类名2(参数), ..., 基类名n(参数), 
本类成员（含对象成员）初始化列表
{
        //其他初始化
}；
```

**构造函数的执行顺序**

1. 调用基类构造函数。
   - 顺序按照它们被继承时声明的顺序（从左向右）。
2. 对初始化列表中的成员进行初始化。
   - 顺序按照它们在类中定义的顺序。
   - 对象成员初始化时自动调用其所属类的构造函数。由初始化列表提供参数。
3. 执行派生类的构造函数体中的内容。

#### 派生类构造函数举例

```c++
#include <iostream>
using namespace std;
class Base1 {//基类Base1，构造函数有参数
public:
    Base1(int i) 
  { cout << "Constructing Base1 " << i << endl; }
};
class Base2 {//基类Base2，构造函数有参数
public:
    Base2(int j) 
  { cout << "Constructing Base2 " << j << endl; }
};
class Base3 {//基类Base3，构造函数无参数
public:
    Base3() 
  { cout << "Constructing Base3 *" << endl; }
};

class Derived: public Base2, public Base1, public Base3 {
public: 
    Derived(int a, int b, int c, int d): Base1(a), member2(d), member1(c), Base2(b)
  //Base1(a) Base2(b)是用于初始化基类里的数据成员，member1(c) member2(d)是用于初始化成员变量（刚好成员是对象）
  //此处的次序与构造函数的执行次序无关        
    { }
 //初始化次序应该是
 //Base2(b),Base1(a)，Base3()因为按照被继承时声明的顺序（从左向右），虽然初始化列表没有显式调用基类base3的初始化，但是还是要默认构造的
  //然后是member1(c)  member2(d)  member3()，按照类里的声明顺序
private:
    Base1 member1;
    Base2 member2;
    Base3 member3;
};

int main() {
    Derived obj(1, 2, 3, 4);
    return 0;
}

```

#### 派生类的复制构造函数

**派生类未定义复制构造函数的情况**

- 编译器会在需要时生成一个隐含的复制构造函数；
- 先调用基类的复制构造函数；
- 再为派生类新增的成员执行复制。

**派生类定义了复制构造函数的情况**

- 一般都要为基类的复制构造函数传递参数。
- 复制构造函数只能接受一个参数，既用来初始化派生类定义的成员，也将被传递给基类的复制构造函数。
- **基类的复制构造函数形参类型是基类对象的引用，实参可以是派生类对象的引用**
- 例如: C::C(const C &c1): B(c1) {…}
  - 也就是拿派生类对象直接传给基类的复制构造函数，因为派生类也可以当基类用

#### 派生类的析构函数

- 析构函数不被继承，派生类如果需要，要自行声明析构函数。
- 声明方法与无继承关系时类的析构函数相同。
- **不需要显式地调用基类的析构函数，系统会自动隐式调用。**
- **先执行派生类析构函数的函数体，再调用基类的析构函数。**

例7-5 派生类对象析构举例

```c++
#include <iostream>
using namespace std;
class Base1 {   
public:
    Base1(int i) 
   { cout << "Constructing Base1 " << i << endl; }
    ~Base1() { cout << "Destructing Base1" << endl; }
};
class Base2 {
public:
    Base2(int j) 
   { cout << "Constructing Base2 " << j << endl; }
    ~Base2() { cout << "Destructing Base2" << endl; }
};
class Base3 {
public:
    Base3() { cout << "Constructing Base3 *" << endl; }
    ~Base3() { cout << "Destructing Base3" << endl; }
};

class Derived: public Base2, public Base1, public Base3 {
public: 
    Derived(int a, int b, int c, int d): Base1(a), member2(d), member1(c), Base2(b)
  { }   
private:    
    Base1 member1;
    Base2 member2;
    Base3 member3;
};

int main() {
    Derived obj(1, 2, 3, 4);
    return 0;
}
观察Derived对象obj的构造和析构
//初始化基类，继承它们的成员。Base2(b),Base1(a)，Base3()因为按照被继承时声明的顺序（从左向右）
Constructing Base2 2     
Constructing Basel 1
Constructing Base3 *

//构造派生类数据成员member1(c)  member2(d)  member3()，按照类里的声明顺序    
Constructing Basel 3 
Constructing Base2 4
Constructing Base3 *

//析构派生类的数据成员，跟构造时的顺序反过来
Destructing Base3
Destructing Base2
Destructing Base1

//析构从基类中继承的数据成员，跟构造时顺序反过来  
Destructing Base3
Destructing Base1
Destructing Base2

```



### 5.派生类成员的标识与访问

#### 访问从基类继承的成员

**作用域限定**

当派生类与基类中有相同成员时:

- 若未特别限定,则通**过派生类对象使用的是派生类中的同名成员，基类同名成员被隐藏。**
- 如要通过派生类对象访问基类中被隐藏的同名成员,应使用基类名和作用域操作符(:) 来限定。

**继承的同名成员被隐藏的例子**

```c++
#include <iostream>
using namespace std;
class Base1 {   
public:
    int var;
    void fun() { cout << "Member of Base1" << endl; }
};
class Base2 {   
public:
    int var;
    void fun() { cout << "Member of Base2" << endl; }
};
class Derived: public Base1, public Base2 {
public:
    int var;    
    void fun() { cout << "Member of Derived" << endl; }
};

int main() {
    Derived d;
    Derived *p = &d;

  //访问Derived类成员
    d.var = 1;
    d.fun();//Derived里的    

    //访问Base1基类成员
    d.Base1::var = 2;
    d.Base1::fun(); 

    //访问Base2基类成员
    p->Base2::var = 3;
    p->Base2::fun();    

    return 0;
}
```

**二义性问题**

- 如果从不同基类继承了同名成员,但是在派生类中没有定义同名成员，“派性类对象名或引名.成员名”、“派生类指针->成员名"访问成员存在二义性问题
  - 也就是多个基类中有同名成员，而派生类并没有同名成员来隐藏
- 解决方式:用类名限定

**二义性问题举例**



```c++
class A {
public:
    void  f();//A里的f函数跟B里的f函数重名了
};
class B {
public:
    void f();
    void g()
};
class C: public A, piblic B {
public:
    void g();
    void h();
};

如果定义：C  c1;
则 c1.f() 具有二义性
而 c1.g() 无二义性（同名隐藏）
```

- 解决方案一：用类名来限定
  - c1.A::f() 或者 c1.B::f()
- 解决方案二：同名隐藏
  - 在C中声明一个同名成员函数f()，f()在根据需要调用A::f()或 B::f()

**多继承时的二义性和冗余问题**

```c++
#include <iostream>
using namespace std;
class Base0 {   //定义基类Base0
public:
    int var0;
    void fun0() { cout << "Member of Base0" << endl; }
};

class Base1: public Base0 { //定义派生类Base1 
public: //新增外部接口
    int var1;
};
class Base2: public Base0 { //定义派生类Base2 
public: //新增外部接口
    int var2;
};

例7-7  多继承时的二义性和冗余问题
class Derived: public Base1, public Base2 {
public: 
    int var;
    void fun() 
  { cout << "Member of Derived" << endl; }
};

int main() {    //程序主函数
    Derived d;
    d.Base1::var0 = 2;
    d.Base1::fun0();
    d.Base2::var0 = 3;
    d.Base2::fun0();
    return 0;
}
```

![tfIijJ.png](https://s1.ax1x.com/2020/06/08/tfIijJ.png)

两个var0就是冗余

#### 虚基类

当多个类从同一个基类派生出来，后来又汇聚在一起共同派生出更下级派生类的时候，就出现了冗余和二义性。

- 需要解决的问题

- - 当派生类从多个基类派生，而这些基类又共同基类，则在访问此共同基类中的成员时，将产生冗余，并有可能因冗余带来不一致性

- 虚基类声明

- - 以virtual说明基类继承方式
  - 例：class B1:virtual public B

- 作用

- - **主要用来解决多继承时可能发生的对同一基类继承多次而产生的二义性问题**
  - 为最远的派生类（derived）提供唯一的基类(base0)成员，而**不重复产生多次复制**

- 注意：

- - 在第一级继承时就要将共同基类设计为虚基类。
  - 比如底下的base1 base2都虚继承base0

![tfIfv4.png](https://s1.ax1x.com/2020/06/08/tfIfv4.png)

```c++
#include <iostream>
using namespace std;
class Base0 {//虚基类
public:
    int var0;
    void fun0() { cout << "Member of Base0" << endl; }
};
class Base1: virtual public Base0 {//虚继承
public: 
    int var1;
};
class Base2: virtual public Base0 {//虚继承
public: 
    int var2;
};


class Derived: public Base1, public Base2 {
//定义派生类Derived 
public: 
    int var;
    void fun() {
        cout << "Member of Derived" << endl;
    }
};

int main() {    
    Derived d;
    d.var0 = 2; //直接访问虚基类的数据成员
    d.fun0();     //直接访问虚基类的函数成员
    return 0;
}
```

#### 虚基类及其派生类构造函数

- 建立对象时所指定的类称为**最远派生类（最底下这个，一层层继承完的这个）**。
- **虚基类（虚基类就是最顶上那个基类，鼻祖）的成员是由最远派生类的构造函数**通过调用虚基类的构造函数进行**初始化的**。
- 在整个继承结构中，直接或间接继承虚基类的所有派生类，都必须在构造函数的成员初始化表中为虚基类的构造函数列出参数。如果未列出，则表示调用该虚基类的默认构造函数。
- 在建立对象时，**只有最远派生类的构造函数调用虚基类的构造函数，其他类对虚基类构造函数的调用被忽略。**

有虚基类时的构造函数举例

```c++
#include <iostream>
using namespace std;

class Base0 {   
public:
    Base0(int var) : var0(var) { }
    int var0;
    void fun0() { cout << "Member of Base0" << endl; }
};
class Base1: virtual public Base0 {
public: 
    Base1(int var) : Base0(var) { }//虽然这时base1也要负责base0的构造，等到真正构造derived对象时，这些都被忽略了。以derived里的构造base0为准
    int var1;
};
class Base2: virtual public Base0 { 
public:
    Base2(int var) : Base0(var) { }
    int var2;
};

class Derived: public Base1, public Base2 {
public:
    Derived(int var) : Base0(var), Base1(var), Base2(var)//现在还要负责最顶上的base0的初始化。以前都是只负责最近的base1和base2就行
   { }
    int var;
    void fun() 
   { cout << "Member of Derived" << endl; }
};

int main() {    //程序主函数
    Derived d(1);
    d.var0 = 2; //直接访问虚基类的数据成员
    d.fun0();   //直接访问虚基类的函数成员
    return 0;
}

```

## 第8章多态性

### 1.运算符重载（可以类内也可以类外，可以友元或者不友元）

#### 1.1运算符重载的规则

- 思考：用“+”、“-”能够实现复数的加减运算吗？
- 实现复数加减运算的方法 ——重载“+”、“-”运算符
- 运算符重载是对已有的运算符赋予多重含义，使同一个运算符作用于不同类型的数据时导致不同的行为。
- C++ 几乎可以重载全部的运算符，而且只能够重载C++中已经有的。
- **不能重载的运算符：“.”、“.*”、“::”、“?:”**
  - 重载之后运算符的优先级和结合性都不会改变。
- 运算符重载是针对新类型数据的实际需要，对原有运算符进行适当的改造。例如：
  - 使复数类的对象可以用“+”运算符实现加法；
  - 是时钟类对象可以用“++”运算符实现时间增加1秒。
- 重载为类的非静态成员函数；（放在类体里）
- 重载为非成员函数。（放在类体外）
  - 如果有的运算符对于这个类来说不能作为成员函数来重载，那么就把它写在类外作为独立的函数

#### 1.2双目运算符重载为类内成员函数

**重载为类成员的运算符函数定义形式**

```c++
函数类型  operator 运算符（形参）//operator和运算符中间必须有一个空格
    {
           ......
    }
    参数个数=原操作数个数-1   （后置++、--除外）
```

**双目运算符重载规则**

- 如果要重载 运算符B 为类成员函数，使之能够实现表达式 **oprd1 B oprd2**，其中 **左操作数oprd1 为A 类对象**，则 **运算符B 应被重载为 A 类的成员函数**，**形参类型应该是 右操作室oprd2 所属的类型**。
- 经重载后，表达式 oprd1 B oprd2 相当于 oprd1.operator B(oprd2)

**例8-1复数类加减法运算重载为成员函数**

要求:

- 将+、-运算重载为复数类的成员函数。

规则:

- 实部和虚部分别相加减。

操作数:

- 两个操作数都是复数类的对象。

源代码：

```c++
#include <iostream>
using namespace std;
class Complex {
public:
    Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { }
    //运算符+重载成员函数
  Complex operator + (const Complex &c2) const;
    //运算符-重载成员函数
  Complex operator - (const Complex &c2) const;
    void display() const;   //输出复数
private:
    double real;    //复数实部
    double imag;    //复数虚部
};
例8-1复数类加减法运算重载为成员函数
Complex Complex::operator+(const Complex &c2) const{
  //创建一个临时无名对象作为返回值 
  return Complex(real+c2.real, imag+c2.imag); 
}

Complex Complex::operator-(const Complex &c2) const{
 //创建一个临时无名对象作为返回值
    return Complex(real-c2.real, imag-c2.imag); 
}

void Complex::display() const {
    cout<<"("<<real<<", "<<imag<<")"<<endl;
}
例8-1复数类加减法运算重载为成员函数
int main() {
    Complex c1(5, 4), c2(2, 10), c3;
    cout << "c1 = "; c1.display();
    cout << "c2 = "; c2.display();
    c3 = c1 - c2;   //使用重载运算符完成复数减法
    cout << "c3 = c1 - c2 = "; c3.display();
    c3 = c1 + c2;   //使用重载运算符完成复数加法
    cout << "c3 = c1 + c2 = "; c3.display();
    return 0;
}
```

**赋值运算符重载的惯用格式是**

```c++
A A::operator=(A &a)
{
    //……
    return *this;
}
```

- 也即，返回值是这个类的对象，且是“=”左边对象本身（赋值后）。

- 函数中不返回值（返回void）理论上也可以完成赋值，缺点在于不能连续赋值。
- 函数中不返回*this而返回临时对象理论上也可以完成赋值，缺点在于内存和效率的浪费。

#### 1.3单目运算符重载为类内成员函数

**前置单目运算符重载规则**

- 如果要重载 单目运算符U 为类成员函数，使之能够实现表达式 U oprd，其中 oprd 为A类对象，则 U 应被重载为 A 类的成员函数，无形参（因为单目，只有一个操作数，也就是调用这个的类对象）。
- 经重载后，表达式 U oprd 相当于 oprd.operator U()

**后置单目运算符++和--重载规则（除了++ --，其他单目运算符也不分前置后置，一律都按第一条规则来写就行。只有++ --比较特殊，要区分前置后置）**

- 如果要重载 ++或--为类成员函数，使之能够实现表达式 oprd++ 或 oprd-- ，其中 oprd 为A类对象，则 ++或-- 应被重载为 A 类的成员函数，且具有一个 int 类型形参。
  - 举例：前置++ 和后置++，都要重载为成员函数，但是呢他们的参数表又一样，这怎么区分两个差别呢？规定前置++ 前置--，不需要形参；后置++，后置--，需要具备一个int类型形参，但是函数体中不应该使用这个形参，它只是用于区分两个重载函数。
- 经重载后，表达式 oprd++ 相当于 oprd.operator ++(0)

**例8-2重载前置++和后置++为时钟类成员函数**

- **前置单目运算符，重载函数没有形参**
- **后置++运算符，重载函数需要有一个int形参**
- 操作数是时钟类的对象。
- 实现时间增加1秒钟。

```c++
#include <iostream>
using namespace std;
class Clock {//时钟类定义
public: 
    Clock(int hour = 0, int minute = 0, int second = 0);
    void showTime() const;
  //前置单目运算符重载
    Clock& operator ++ ();//前置没有形参
  //后置单目运算符重载
    Clock operator ++ (int);//++后置要带一个形参，用于区分两个运算符。调用时无需传参。直接 Obj++就行  
private:
    int hour, minute, second;
};

Clock::Clock(int hour, int minute, int second) {    
    if (0 <= hour && hour < 24 && 0 <= minute && minute < 60
        && 0 <= second && second < 60) {
        this->hour = hour;
        this->minute = minute;
        this->second = second;
    } else
        cout << "Time error!" << endl;
}
void Clock::showTime() const {  //显示时间
    cout << hour << ":" << minute << ":" << second << endl;
}

例8-2重载前置++和后置++为时钟类成员函数
Clock & Clock::operator ++ () { //前置，返回引用
    second++;
    if (second >= 60) {
        second -= 60;  minute++;
        if (minute >= 60) {
          minute -= 60; hour = (hour + 1) % 24;
        }
    }
    return *this;
}

Clock Clock::operator ++ (int) {//后置，返回副本
    //注意形参表中的整型参数
    Clock old = *this;
    ++(*this);  //调用前置“++”运算符。也可以重新写，但是为了简单就调用写好的前置++
    return old;
}
例8-2重载前置++和后置++为时钟类成员函数
int main() {
    Clock myClock(23, 59, 59);
    cout << "First time output: ";
    myClock.showTime();
    cout << "Show myClock++:    ";
    (myClock++).showTime();
    cout << "Show ++myClock:    ";
    (++myClock).showTime();
    return 0;
}
运行结果:
First time output: 23:59:59
Show myClock+ +:23:59:59
Show + +myClock:0:0:1

```



#### 1.4运算符重载为类外的非成员函数

有些运算符不能重载为成员函数，例如二元运算符的左操作数不是对象，或者是不能由我们重载运算符的对象。（比如实数+复数，实数在左边；或者说某个类是类库里的，我们没法添加运算符重载函数。这两种情况就得把运算符重载到类外，作非成员函数了）

**运算符重载为非成员函数的规则**

- 函数的形参代表依自左至右次序排列的各操作数。
- 重载为非成员函数时
  - 参数个数=原操作数个数（后置++、--除外）
  - **至少应该有一个自定义类型的参数。（不能两个参数都是基本数据类型，比如两个int，我重新定义了+，这是绝对不允许的）**
- 后置单目运算符 ++和--的重载函数，形参列表中要增加一个int，但不必写形参名。
- **如果在运算符的重载函数中需要操作某类对象的私有成员，可以将此函数声明为该类的友元。**（如果没声明成友元，也可以通过Pubic接口来获取私有成员）

**运算符重载为非成员函数的规则**

- 双目运算符 B重载后，

  表达式oprd1 B oprd2

  等同于operator B(oprd1,oprd2 )

- 前置单目运算符 B重载后，

  表达式 B oprd

  等同于operator B(oprd )

- 后置单目运算符 ++和--重载后，

  表达式 oprd B

  等同于operator B(oprd,0 )

**例8-3 重载Complex的加减法和“<<”运算符为非成员函数**

- 将+、-（双目）重载为非成员函数，并将其声明为复数类的友元，两个操作数都是复数类的常引用。 

- 将<<（双目）重载为非成员函数，并将其声明为复数类的友元，它的左操作数是std::ostream引用，右操作数为复数类的常引用，返回std::ostream引用，用以支持下面形式的输出：

  - ```c++
    cout << a << b;
    ```

  - 该输出调用的是：

  - 

  ```c++
  operator << (operator << (cout, a), b);
  ```

```c++
//8_3.cpp
    #include <iostream>
    using namespace std;

    class Complex {
    public:
        Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { } 
        //如果real,imag是public成员，可以直接访问，那么无需声明为友元，直接在类外实现就行
        friend Complex operator+(const Complex &c1, const Complex &c2);
        friend Complex operator-(const Complex &c1, const Complex &c2);
        friend ostream & operator<<(ostream &out, const Complex &c);
    private:    
        double real;  //复数实部
        double imag;  //复数虚部
    };

    Complex operator+(const Complex &c1, const Complex &c2){
        return Complex(c1.real+c2.real, c1.imag+c2.imag); 
    }
    Complex operator-(const Complex &c1, const Complex &c2){
        return Complex(c1.real-c2.real, c1.imag-c2.imag); 
    }

    ostream & operator<<(ostream &out, const Complex &c){
        out << "(" << c.real << ", " << c.imag << ")";
        return out;
    }

    int main() {    
        Complex c1(5, 4), c2(2, 10), c3;    
        cout << "c1 = " << c1 << endl;
        cout << "c2 = " << c2 << endl;
        c3 = c1 - c2;   //使用重载运算符完成复数减法
        cout << "c3 = c1 - c2 = " << c3 << endl;
        c3 = c1 + c2;   //使用重载运算符完成复数加法
        cout << "c3 = c1 + c2 = " << c3 << endl;
        return 0;
    }
```

### 2.虚函数（告诉编译器，编译时不要锁死该函数的函数体到底是哪个）

实现动态绑定的函数

- 最常用的情形就是，指针是基类指针，但是指向的是派生类对象。比如Base *ptr=new Derived();
- 那么这种情况下，实际上利用指针调用各种函数时，如果是非虚函数，那么一定调用的是基类的。如果是虚函数，那么将会调用派生类的。
- 虚函数在基类中有定义，派生类再重定义一下，就可以实现运行时多态。（都要通过指针实现）

#### 2.1虚函数

回顾第7章的例子

**类型转换规则举例**

```c++
#include <iostream>
using namespace std;
class Base1 { //基类Base1定义
public:
    void display() const {
        cout << "Base1::display()" << endl;
    }
};
class Base2: public Base1 { //公有派生类Base2定义
public:
    void display() const {
        cout << "Base2::display()" << endl;
    }
};
class Derived: public Base2 { //公有派生类Derived定义
public:
    void display() const {
        cout << "Derived::display()" << endl;
    }
};

void fun(Base1 *ptr) {  //参数为指向基类对象的指针
    ptr->display();     //"对象指针->成员名"
    //display是非虚函数，编译时就确定了，函数体是Base1类的display函数体
}
int main() {    //主函数
    Base1 base1;    //声明Base1类对象
    Base2 base2;    //声明Base2类对象
    Derived derived;    //声明Derived类对象

    fun(&base1);    //用Base1对象的指针调用fun函数
    fun(&base2);    //用Base2对象的指针调用fun函数
    fun(&derived);     //用Derived对象的指针调用fun函数

    return 0;
}
运行结果：
    Base1::display()
    Base1::display()
    Base1::display()
    调用的都是基类的display函数，而没有实现我们想要的传什么类型调用什么类型的display
    所以，这是个非常严重的错误。
    注意：一定不要重新定义继承而来的非虚函数，否则就会出现预期结果与实际结果不一致，但又不报错的问题
```

**通过虚函数可以实现运行时的多态**

- virtual的意思就是告诉编译器，凡是你遇到这样原型的函数的调用，你都不要在编译的时候马上做决定，决定它该去调用哪个函数的函数体，先把这个工作滞后。换言之，**它指示编译器在编译阶段不要做静态绑定，要为运行阶段做动态绑定做好准备。**
- **所有的虚函数都必须以非内联的形式在类外实现函数体**，也就是函数实现不能写在类体里。因为virtual要求编译器不要在编译时处理，运行阶段再去决定对哪个display的调用。而内联函数一定是在编译阶段处理的，把它嵌入到程序代码中去。所以两者显然矛盾。

```c++
#include <iostream>
using namespace std;

class Base1 {
public:
    virtual void display() const;  //虚函数
};
void Base1::display() const {
    cout << "Base1::display()" << endl;
}

class Base2:public Base1 { 
public:
     virtual void display() const;
};
void Base2::display() const {
    cout << "Base2::display()" << endl;
}
class Derived: public Base2 {
public:
     virtual void display() const; 
};
void Derived::display() const {
    cout << "Derived::display()" << endl;
}

void fun(Base1 *ptr) { 
    ptr->display(); //所有的display都应该写成虚函数，在编译时无法确定是哪个函数体。等到运行时，再看，传进来的指针到底是指向哪个类（派生类或者继生类）的，就调用哪类的display函数
}

int main() {    
    Base1 base1;
    Base2 base2;
    Derived derived;    
    fun(&base1);
    fun(&base2);
    fun(&derived);
    return 0;
}
运行结果:
Basel::display()
Base2:display()
Derivel::display()
```

```c++
#include <iostream>
using namespace std;

class Base1 {
public:
    void display() const;  //虚函数
};
void Base1::display() const {
    cout << "Base1::display()" << endl;
}

class Base2:public Base1 {
public:
    virtual void display() const;
};
void Base2::display() const {
    cout << "Base2::display()" << endl;
}
class Derived : public Base2 {
public:
    virtual void display() const;
};
void Derived::display() const {
    cout << "Derived::display()" << endl;
}

void fun(Base1* ptr) {
    ptr->display(); //所有的display都应该写成虚函数，在编译时无法确定是哪个函数体。等到运行时，再看，传进来的指针到底是指向哪个类（派生类或者继生类）的，就调用哪类的display函数
}

int main() {
    Base1* test = new Base1();
    test->display();
    test = new Base2();
    test->display();
    test = new Derived();
    test->display();
    Base2* test2 = new Base2();
    test2->display();
    test2 = new Derived();
    test2->display();
    return 0;
}
//base里没声明成virtual，继承派生是virtual都没用
//Base1::display()
//Base1::display()
//Base1::display()
//Base2::display()
//Derived::display()
```

**初识虚函数**

- 用virtual关键字说明的函数
- 虚函数是实现运行时多态性基础
- **C++中的虚函数是动态绑定的函数**
- **虚函数必须是非静态的成员函数**，**虚函数经过派生**之后，就可以**实现运行过程中的多态**。
  - 非静态成员函数，即说明虚函数应该属于具体对象的，而不是属于整个类共享的。
  - 它需要在运行时用指针定位到它实际上指定的对象（**比如虽然是基类指针，但是实际上指向的是派生类对象，那就调用派生类的那一套东西**）是谁，然后决定调用哪个函数体，所以虚函数当然得是属于对象的，而不是整个类共享的
  - 虚函数在基类中有定义，派生类再重定义一下，就可以实现运行时多态。（非虚函数不可以，非虚函数在编译时就定死了，无法运行多态，而是定态）
- 一般成员函数可以是虚函数
- **构造函数不能是虚函数**
- **析构函数可以是虚函数**

**一般虚函数成员**

- 虚函数的声明

  virtual 函数类型 函数名（形参表）;

- **虚函数声明**只能出现在**类定义中的函数原型声明**中，而**不能在成员函数实现的时候**。

- **在派生类中可以对基类中的成员函数进行覆盖**。

  - 非虚函数在派生类其实也可以覆盖，但是很容易造成混淆，**千万不要重新定义继承而来的非虚函数**

- **虚函数一般不声明为内联函数**，因为对虚函数的调用需要动态绑定，而对内联函数的处理是静态的。

**virtual关键字**

- 派生类可以不显式地用virtual声明虚函数，这时系统就会用以下规则来判断派生类的一个函数成员是不是虚函数：
  - 该函数是否与基类的虚函数有相同的名称、参数个数及对应参数类型；
  - 该函数是否与基类的虚函数有相同的返回值或者满足类型兼容规则的指针、引用型的返回值；
- 如果从名称、参数及返回值三个方面检查之后，派生类的函数满足上述条件，就会自动确定为虚函数。这时，派生类的虚函数便覆盖了基类的虚函数。
- **派生类中的虚函数还会隐藏基类中同名函数的所有其它重载形式。**
- 一般习惯于在派生类的函数中也使用virtual关键字，以增加程序的可读性。
- 对于派生类中，你写了virtual的一定是虚函数，你没写virtual，编译器也会检查一下，它在基类中是不是virtual。如果跟基类函数的函数声明一模一样，基类中它还是virtual，那么自动的派生类中这个函数也就变成virtual了，实现运行时动态绑定。

#### 2.2 虚析构函数

为什么需要虚析构函数？

- 可能通过基类指针删除派生类对象； 
- 如果你打算允许其他人（其他程序代码）通过基类指针调用派生类对象的析构函数（通过delete这样做是正常的），就需要让基类的析构函数成为虚函数，否则执行delete的结果是不确定的。

**一个不使用虚析构函数的例子**

```c++
#include <iostream>
using namespace std; 
class Base {
public:
    ~Base(); //不是虚函数
};
Base::~Base() {
    cout<< "Base destructor" << endl;
 }
class Derived: public Base{
public:
    Derived();
    ~Derived(); //不是虚函数
private:
    int *p;
};
Derived::Derived(){//派生类构造函数实现
    p=new int(0);
}
Derived::~Derived(){//派生类析构函数实现
    cout<<"Derived desructor"<<endl;
    delete p;
}


int main(){
    Base *b=new Derived();
    delete b;//执行了基类的析构函数，但是实际上，派生类里边还有新增的数据，内存没有回收，就这样泄露了
    return 0;
}
运行结果：Base destructor
```

```c++
#include <iostream>
using namespace std;
class Base {
public:
    virtual ~Base();
};
Base::~Base() {
    cout<< "Base destructor" << endl;
 }
class Derived: public Base{
public:
    Derived();
    virtual ~Derived();
private:
    int *p;
};
Derived::Derived(){//派生类构造函数实现
    p=new int(0);
}
Derived::~Derived(){//派生类析构函数实现
    cout<<"Derived desructor"<<endl;
    delete p;
}
int main(){
    Base *b=new Derived();//当析构函数有了virtual以后，虽然b是基类指针，但是实际上指向的是派生类对象，所以当delete b调用析构函数的时候，将会动态的绑定到派生类析构函数上，调用派生类里的析构函数
    delete b;
    //假如用虚析构函数的话，那么效果就和
    //Derived *b=new Derived();delete b一样了
    //都是先执行派生类的析构函数，再执行基类的析构函数
    return 0;
}
运行结果：
Derived destructor
Base destructor
```

#### 2.3 虚表与动态绑定

主要是讲动态绑定的原理，实际上的运行规则，只看前边几节就行。

为什么运行时可以动态绑定呢？运行时没有编译环境了，只有操作系统，谁帮我们确定该执行哪个函数体呢？其实还是编译器为我们预先做好了准备，在编译时埋好了伏笔，这就是虚表。

- 虚表

  - **每个多态类有一个虚表**（virtual table）
  - **虚表中有当前类的各个虚函数的入口地址**
  - **每个对象有一个指向当前类的虚表的指针（虚指针vptr）**
  - 在编译时，编译器做好虚表，等待运行时查找

- 动态绑定的实现

  - 构造函数中为对象的虚指针赋值
  - 通过多态类型的指针或引用调用成员函数时，通过虚指针找到虚表，进而找到所调用的虚函数的入口地址
  - 通过该入口地址调用虚函数。

  ![NSJHxO.png](https://s1.ax1x.com/2020/06/14/NSJHxO.png)

```c++
class A{
public:
	virtual void fun();
};

sizeof(A)=8 byte;//因为对象A中含有一个指向类A的虚表的指针，在64位机器上，指针占8个字节。
```

### 3.抽象类（规范接口，只能做基类，且无法创建实例）

- 虚基类（里有虚继承的概念）：是为了解决多个类从同一个基类派生出来，后来又汇聚在一起共同派生出更下级派生类的时候，数据冗余和二义的问题
  - 虚基类是指最顶上最原始那个基类，往下继承时要虚继承
- 抽象类（里有纯虚函数的概念）：为了保持基类与派生类的统一接口，派生类和基类的区别通过重写继承而来的虚函数来实现
  - 抽象类也是最原始那个基类，里边**必须包含至少一个纯虚函数**

#### 3.1纯虚函数

- 纯虚函数是一个在基类中声明的虚函数，它在该基类中没有定义具体的操作内容，要求各派生类根据实际需要定义自己的版本，纯虚函数的声明格式为：

  virtual 函数类型 函数名(参数表) = 0;

- **带有纯虚函数的类称为抽象类**

- 为了规定整个类家族的统一的行为和对外接口，就需要在比较高层次的基类中，定义这种纯虚函数。=0表示没有函数体，不是返回值为0。

#### 3.2抽象类

带有至少一个纯虚函数的类称为抽象类:（可以有其他的成员函数，也可以有函数实现，但是一旦有一个=0这种形式的纯虚函数，那它就是抽象类）

class 类名 { virtual 类型 函数名(参数表)=0; //其他成员…… }

#### 3.3抽象类作用

- 抽象类为抽象和设计的目的而声明
  - 规定了对外的接口的统一形式，这样的话就可以将基类对象和各级不同派生类对象，都按照统一的方式进行处理，因为它们都有统一的接口。
  - 比如通过基类指针，接受不同派生类对象的地址，然后调用在基类中定义过的函数名，虽然在基类中没有实现，但是由于虚函数的动态绑定机制，可以调用派生类中定义的相应函数。
  
- 将有关的数据和行为组织在一个继承层次结构中，保证派生类具有要求的行为。

- 对于暂时无法实现的函数，可以声明为纯虚函数，留给派生类去实现。

  - **注意，纯虚函数  基类可以实现，也可以不实现。一般都是留给派生类去实现，但是基类也可以自己实现**

  - ```c++
    class A
    {
    public:
    
        virtual void pureVirtualFunc() = 0;
    };
    void A::pureVirtualFunc()
    {
        cout << "I'am pureVirtualFunc" << endl;
    }
    class B : public A
    {
    public:
        void pureVirtualFunc()
        {
            A::pureVirtualFunc();
            cout << "I belong to B!";
        }
    };
    int main(void)
    {
        B b;
        b.pureVirtualFunc();
        return 0;
    }
    //运行结果：
    //I’am pureVirtualFunc
    //I belong to B!
    ```

    

  - **纯虚析构函数，必须实现**

#### 3.4注意

- **抽象类只能作为基类来使用。**
- **不能定义抽象类的对象。**

#### 3.5抽象类举例

```c++
//8_6.cpp
#include <iostream>
using namespace std;

class Base1 {//抽象类
public:
    virtual void display() const = 0;   //纯虚函数
};

class Base2: public Base1 { 
public:
    virtual void display() const; //覆盖基类的虚函数
};
void Base2::display() const {
    cout << "Base2::display()" << endl;
}

class Derived: public Base2 { 
public:
     virtual void display() const; //覆盖基类的虚函数
};
void Derived::display() const {
    cout << "Derived::display()" << endl;
} 
void fun(Base1 *ptr) { 
    ptr->display(); 
}
int main() {
    //不可以用Base1类来实例化，因为他是抽象类
    //即不能够 Base1 yyd;  也不能Base1* yyd=new Base1();
    Base2 base2;
    Derived derived;
    fun(&base2);//Base2::display()    
    fun(&derived);  //Derived::display()
    Base1* ptr1 = new Derived();
    Base1* ptr2 = new Base2();
    Base2* ptr3 = new Derived();
    fun(ptr1); // Derived::display()
    fun(ptr2); //Base2::display()
    fun(ptr3);// Derived::display()
    return 0;
}
```

### 4.override与final（override用于避免出错，final是不让继承或者覆盖）

- **隐藏**：不加virtual关键字，同名（同签名）函数在派生类中重定义，那么派生类函数隐藏了基类的函数
  - 也就是**重定义了继承而来的非虚函数**，非常不推荐这种做法
- **覆盖**：加上virtual关键字，同名（同签名）函数在派生类中重定义，那么派生类函数覆盖了基类的函数
  - 也就是**重定义了继承而来的虚函数**，支持这样做，这样实现了运行时的绑定，也就是多态。

#### 4.1override

- **多态行为的基础：基类声明虚函数，继承(派生)类声明一个函数覆盖该虚函数**
- 覆盖要求： 函数签名（signatture）完全一致
- 函数签名包括：函数名 参数列表 const

下列程序就仅仅因为疏忽漏写了const，导致多态行为没有如期进行

```c++
#include<iostream>
using namespace std;

class Base {
public:
	virtual void f1(int) const;
	virtual ~Base() {};
};
void Base::f1(int) const {
	cout << "Base f1" << endl;
	return;
}

class Derived :public Base {
public:
	void f1(int);//缺少了const，跟基类的f1函数签名不一样，无法自动识别为虚函数
	~Derived() {};
};
void Derived::f1(int) {
	cout << "Derived f1" << endl;
	return;
}

int main() {
	Base* b;
	b = new Base();
	b->f1(1);
	b = new Derived();
	b->f1(1);
	return 0;
}
//运行结果
Base f1
Base f1
```

```c++
#include<iostream>
using namespace std;

class Base {
public:
	virtual void f1(int) const;
	virtual ~Base() {};
};
void Base::f1(int) const {
	cout << "Base f1" << endl;
	return;
}

class Derived :public Base {
public:
	void f1(int) const;//虽然没加virtual关键字，但是跟基类f1签名一模一样，自动识别为虚函数
	~Derived() {};
};
void Derived::f1(int) const{
	cout << "Derived f1" << endl;
	return;
}

int main() {
	Base* b;
	b = new Base();
	b->f1(1);
	b = new Derived();
	b->f1(1);
	return 0;
}
//Base f1
//Derived f1
```

```c++
//情况1
Base::f1(int) const     Derived::f1(int) const
输出：
base::f1 const
Derived::f1 const
//情况2
Base::f1(int)   Derived::f1(int) 
输出：
base::f1
Derived::f1
//情况3
Base::f1(int) const       Derived::f1(int) 
输出：
base::f1 const
Derived::f1 const
//情况4
Base::f1(int)   Derived::f1(int)const
输出：
base::f1
base::f1
    
总结：
只要签名不一致，那么都是没有这个动态绑定（多态）的效果的。  
```



#### 4.2显式函数覆盖override

- C++11 引入显式函数覆盖，在编译期而非运行期捕获此类错误。 

- 在虚函数显式重载中运用，编译器会检查基类是否存在一虚拟函数，与派生类中带有声明override的虚拟函数，有相同的函数签名（signature）；若不存在，则会回报错误。
- 用override说明的函数，就必须在基类中找到同样原型的虚函数。如果在派生类中写了一个函数，用override说明了，但是编译器在基类中找不到相同原型的虚函数，那么编译器就会报错。
  - 也就是说，我在派生类本来意图写一个函数去覆盖基类的虚函数。加上override后，可以保证了我不犯低级错误。（比如，上边的在派生类中意图覆盖f1，但是忘记加const，导致没有成功覆盖。如果我加了override，编译器就可以帮助我发现问题）

#### 4.3final:不希望本类被继承或者本函数被覆盖

C++11提供的final，用来避免类被继承，或是基类的函数被改写 

```c++
例： struct Base1 final { };//Base1不能派生出新类
struct Derived1 : Base1 { }; // 编译错误：Base1为final，不允许被继承


struct Base2 { virtual void f() final; };
struct Derived2 : Base2 { void f(); // 编译错误：Base2::f 为final，不允许被覆盖 };
```

## 第9章 模板与群体数据

### 1.函数模板（定义一个函数，里边用了模板）

#### 函数模板

- 思考：如果重载的函数，其解决问题的逻辑是一致的、函数体语句相同，只是处理的数据类型不同，那么写多个相同的函数体，是重复劳动，而且还可能因为代码的冗余造成不一致性。

- 解决：使用模板

- **函数重载**给代码的使用者提供了方便，但是代码的编写者，还是要写很多个类似的代码。所以一般不用函数重载，应该使用函数模板。

- 例子：

- ```c++
  #include<iostream>
  using namespace std;
  
  template<typename T>
  T abs(T x){
      return x<0?-x:x;
  }
  int main(){
      int n=-5;
      double d=-5.5;
      cout<<abs(n)<<endl;
      cout<<abs(d)<<endl;
      return 0;
  }
  ```

#### 函数模板定义语法

语法形式：

template <模板参数表>

#### 函数定义

模板参数表的内容

- 类型参数：class（或typename） 标识符
- 常量参数：类型说明符 标识符
- 模板参数：template <参数表> class标识符

#### 例子

```c++
#include <iostream>
using namespace std;

template <class T>  //定义函数模板
void outputArray(const T *array, int count) {//也可以不加那个const。不加的话，比如array是指向Int的指针，那么就可以修改指针指向内容。如果array是指向const int，那么就不可以修改。
 //当T前边加了const以后，那么就一定不可以修改。
    for (int i = 0; i < count; i++)
        cout << array[i] << " "; //如果数组元素是类的对象，需要该对象所属类重载了流插入运算符“<<”
    cout << endl;
}

int main() {     
    const int A_COUNT = 8, B_COUNT = 8, C_COUNT = 20;
    int a [A_COUNT] = { 1, 2, 3, 4, 5, 6, 7, 8 };
    double b[B_COUNT] = { 1.1, 2.2, 3.3, 4.4, 5.5, 6.6, 7.7, 8.8 };
    char c[C_COUNT] = "Welcome!";

    cout << " a array contains:" << endl;
    outputArray(a, A_COUNT);    
    cout << " b array contains:" << endl;
    outputArray(b, B_COUNT);    
    cout << " c array contains:" << endl;
    outputArray(c, C_COUNT);    
    return 0;
}
//
运行结果如下：
a array contains:
1 2 3 4 5 6 7 8
b array contains:
1.1 2.2 3.3 4.4 5.5 6.6 7.7 8.8 
c array contains:
W e l c o m e!
```

#### 注意

- 一个函数模板并非自动可以处理所有类型的数据
- 只有能够进行函数模板中运算的类型，可以作为类型实参
- 自定义的类，需要重载模板中的运算符，才能作为类型实参(比如例子中的class T，假如调用的时候不是自带的数组类型，而是自己写的一个类，里边没有[i]运算或者没有重载cout<<，这都是会报错的)

### 2.类模板（定义一个类，里边用了模板）

#### 类模板的作用

使用类模板使用户可以为类声明一种模式，使得类中的某些数据成员、某些成员函数的参数、某些成员函数的返回值，能取任意类型（包括基本类型的和用户自定义类型）。

注意：

**类模板，是我要写一个类，里边用了模板参数（这个参数可以是一个class)。而不是写了个模板可以定义很多的类（或者可以理解为可以定义非常相似的许多的类，不同的T，即对应不同的类）。**

#### 类模板的声明

- 类模板 
  - template <模板参数表> 
  - class 类名 
  - {类成员声明};
- 如果需要在类模板以外定义其成员函数，则要采用以下的形式：
  -  template <模板参数表> 
  - 类型名 类名<模板参数标识符列表>::函数名（参数表）
- 模板类的成员函数实现应该放在头文件中
  - 因为模板类本质上不是类，只有实例化之后才会编译生成.o文件

#### 例子

```c++
#include <iostream>
#include <cstdlib>
using namespace std;
struct Student {
  int id;       //学号
  float gpa;    //平均分
}; 


template <class T>
class Store {//类模板：实现对任意类型数据进行存取
private:
    T item; // item用于存放任意类型的数据
    bool haveValue;  // haveValue标记item是否已被存入内容
public:
    Store();//默认构造函数
    T &getElem();   //提取数据函数
    void putElem(const T &x);  //存入数据函数
};

template <class T>  
Store<T>::Store(): haveValue(false) { } //store类默认构造函数的实现

template <class T>
T &Store<T>::getElem() {//store类成员函数的实现
    //如试图提取未初始化的数据，则终止程序
    if (!haveValue) {   
        cout << "No item present!" << endl;
        exit(1);    //使程序完全退出，返回到操作系统。
    }
    return item;        // 返回item中存放的数据 
}

template <class T>
void Store<T>::putElem(const T &x) {//store类成员函数的实现
    // 将haveValue 置为true，表示item中已存入数值   
    haveValue = true;   
    item = x;           // 将x值存入item
}

int main() {
    Store<int> s1, s2;  
    s1.putElem(3);  
    s2.putElem(-7);
    cout << s1.getElem() << "  " << s2.getElem() << endl;

    Store<Student> s3;
    Student g = { 1000, 23 };
    s3.putElem(g); 
    cout << "The student id is " << s3.getElem().id << endl;

    Store<double> d;
    cout << "Retrieving object D... ";
    cout << d.getElem() << endl;
   //d未初始化，执行函数D.getElement()时导致程序终止
    return 0;
}
```

### 3.线性群体

- 群体是指由多个数据元素组成的集合体。群体可以分为两个大类：**线性群体**和**非线性群体**。
- 线性群体中的元素按位置排列有序，可以区分为第一个元素、第二个元素等。
- 非线性群体不用位置顺序来标识元素。
- 线性群体中的元素次序与其逻辑位置关系是对应的。在线性群体中，又可按照访问元素的不同方法分为**直接访问、顺序访问和索引访问**。
- 在本章我们只介绍直接访问和顺序访问。

### 4.数组类模板

- 静态数组是具有固定元素个数的群体，其中的元素可以通过下标直接访问。

- - 缺点：大小在编译时就已经确定，在运行时无法修改。

- 动态数组由一系列位置连续的，任意数量相同类型的元素组成。

- - 优点：其元素个数可在程序运行时改变。

- vector就是用类模板实现的动态数组。

```c++
#ifndef ARRAY_H
#define ARRAY_H
#include <cassert>

template <class T>  //数组类模板定义
class Array {
private:
    T* list;        //用于存放动态分配的数组内存首地址
    int size;       //数组大小（元素个数）
public:
    Array(int sz = 50);     //构造函数，默认构造50个元素的数组
    Array(const Array<T> &a);   //复制构造函数
    ~Array();           //析构函数
    Array<T> & operator = (const Array<T> &rhs);    //重载"=“
    T & operator [] (int i); //重载"[]”
    const T & operator [] (int i) const;     //重载"[]”常函数
    operator T * ();        //重载到T*类型的转换。就是拿数组名做首地址使用
    operator const T * () const;
    int getSize() const;        //取数组的大小
    void resize(int sz);        //修改数组的大小
};

template <class T> Array<T>::Array(int sz) {//构造函数
    assert(sz >= 0);//sz为数组大小（元素个数），应当非负
    size = sz;  // 将元素个数赋值给变量size
    list = new T [size];    //动态分配size个T类型的元素空间
}

template <class T> Array<T>::~Array() { //析构函数
    delete [] list;
}

template <class T> 
Array<T>::Array(const Array<T> &a) {    //复制构造函数
    size = a.size;     //从对象x取得数组大小，并赋值给当前对象的成员
    list = new T[size]; // 动态分配n个T类型的元素空间
    for (int i = 0; i < size; i++)     //从对象X复制数组元素到本对象 
        list[i] = a.list[i];
}

//续上，重载"="运算符，将对象rhs赋值给本对象。实现对象之间的整体赋值
template <class T>
Array<T> &Array<T>::operator = (const Array<T>& rhs) {
    if (&rhs != this) {
//如果本对象中数组大小与rhs不同，则删除数组原有内存，然后重新分配
        if (size != rhs.size) {
            delete [] list; //删除数组原有内存
            size = rhs.size;    //设置本对象的数组大小
            list = new T[size];  //重新分配size个元素的内存
        }
        //从对象X复制数组元素到本对象  
        for (int i = 0; i < size; i++)
            list[i] = rhs.list[i];
    }
    return *this;   //返回当前对象的引用
}
//重载下标运算符，实现与普通数组一样通过下标访问元素，具有越界检查功能
template <class T>
T &Array<T>::operator[] (int n) {
    assert(n >= 0 && n < size);  //检查下标是否越界
    return list[n];       //返回下标为n的数组元素
}
template <class T>
const T &Array<T>::operator[] (int n) const {
    assert(n >= 0 && n < size);  //检查下标是否越界
    return list[n];       //返回下标为n的数组元素
//重载指针转换运算符，将Array类的对象名转换为T类型的指针
template <class T>
Array<T>::operator T * () {
    return list;    //返回当前对象中私有数组的首地址
}

//取当前数组的大小
template <class T>
int Array<T>::getSize() const {
    return size;
}

// 将数组大小修改为sz
template <class T>
void Array<T>::resize(int sz) {
    assert(sz >= 0);    //检查sz是否非负
    if (sz == size) //如果指定的大小与原有大小一样，什么也不做
        return;
    T* newList = new T [sz];    //申请新的数组内存
    int n = (sz < size) ? sz : size;//将sz与size中较小的一个赋值给n
    //将原有数组中前n个元素复制到新数组中
    for (int i = 0; i < n; i++)
        newList[i] = list[i];
    delete[] list;      //删除原数组
    list = newList; // 使list指向新数组
    size = sz;  //更新size
}
#endif  //ARRAY_H
```

#### 为什么有的函数返回引用

- 如果一个函数的返回值是一个对象的值，就是右值，不能成为左值。
  - 也就是返回右值，只能使用，不能改变对象的状态
- 如果返回值为引用。由于引用是对象的别名，通过引用可以改变对象的值，因此是左值。
  - 返回引用，那么返回的就是左值

#### 指针转换运算符的作用

operator T * ();        //重载到T*类型的转换。就是拿数组名做首地址使用
    operator const T * () const;

```c++
#include <iostream>
using namespace std;

void read(int *p, int n) {
 for (int i = 0; i < n; i++)
    cin >> p[i];
}
int main() {
 int a[10];
 read(a, 10);
 return 0;
}

#include "Array.h"
#include <iostream>
using namespace std;

void read(int *p, int n) {
 for (int i = 0; i < n; i++)
   cin >> p[i];
}
int main() {
Array<int> a(10);
 read(a, 10);
 return 0;
}

```

### 5.链表

**结点类模板**

```c++
//Node.h
#ifndef NODE_H
#define NODE_H
//类模板的定义
template <class T>
class Node {
private:
    Node<T> *next;  //指向后继结点的指针
public:
    T data; //数据域
    Node (const T &data, Node<T> *next = 0);    //构造函数
    void insertAfter(Node<T> *p);   //在本结点之后插入一个同类结点p 
    Node<T> *deleteAfter(); //删除本结点的后继结点，并返回其地址
    Node<T> *nextNode();            //获取后继结点的地址
    const Node<T> *nextNode() const;     //获取后继结点的地址
};

//类的实现部分
//构造函数，初始化数据和指针成员
template <class T>
Node<T>::Node(const T& data, Node<T> *next = 0 ) : data(data), next(next) { }
//返回后继结点的指针
template <class T>
Node<T> *Node<T>::nextNode() {
    return next;
}
//返回后继结点的指针
template <class T>
const Node<T> *Node<T>::nextNode() const {
    return next;
} 
//在当前结点之后插入一个结点p 
template <class T>
void Node<T>::insertAfter(Node<T> *p) {
    p->next = next; //p结点指针域指向当前结点的后继结点
    next = p;    //当前结点的指针域指向p 
}
//删除当前结点的后继结点，并返回其地址
template <class T> Node<T> *Node<T>::deleteAfter() {
    Node<T> *tempPtr = next;//将欲删除的结点地址存储到tempPtr中
    if (next == 0)  //如果当前结点没有后继结点，则返回空指针
        return 0;
    next = tempPtr->next;//使当前结点的指针域指向tempPtr的后继结点
    return tempPtr;         //返回被删除的结点的地址
}
#endif //NODE_H
```

**链表类模板**

```c++
//LinkedList.h
#ifndef LINKEDLIST_H
#define LINKEDLIST_H
#include "Node.h"

template <class T>
class LinkedList {
private:
    //数据成员：
    Node<T> *front, *rear;  //表头和表尾指针
    Node<T> *prevPtr, *currPtr;   //记录表当前遍历位置的指针，由插入和删除操作更新
    int size;   //表中的元素个数
    int position;   //当前元素在表中的位置序号。由函数reset使用

    //函数成员：
    //生成新结点，数据域为item，指针域为ptrNext
    Node<T> *newNode(const T &item,Node<T> *ptrNext=NULL);

    //释放结点
    void freeNode(Node<T> *p);

    //将链表L 拷贝到当前表（假设当前表为空）。
    //被拷贝构造函数、operator = 调用
    void copy(const LinkedList<T>& L);

public:
    LinkedList();   //构造函数
    LinkedList(const LinkedList<T> &L);  //拷贝构造函数
    ~LinkedList();  //析构函数
    LinkedList<T> & operator = (const LinkedList<T> &L); //重载赋值运算符

    int getSize() const;    //返回链表中元素个数
    bool isEmpty() const;   //链表是否为空

    void reset(int pos = 0);//初始化游标的位置
    void next();    //使游标移动到下一个结点
    bool endOfList() const; //游标是否到了链尾
    int currentPosition() const;    //返回游标当前的位置

    void insertFront(const T &item);    //在表头插入结点
    void insertRear(const T &item);     //在表尾添加结点
    void insertAt(const T &item);       //在当前结点之前插入结点
    void insertAfter(const T &item);    //在当前结点之后插入结点

    T deleteFront();    //删除头结点
    void deleteCurrent();   //删除当前结点

    T& data();              //返回对当前结点成员数据的引用
    const T& data() const;   //返回对当前结点成员数据的常引用

    //清空链表：释放所有结点的内存空间。被析构函数、operator= 调用
    void clear();
};

template <class T> //生成新结点
Node<T> *LinkedList<T>::newNode(const T& item, Node<T>* ptrNext)
{
    Node<T> *p;
    p = new Node<T>(item, ptrNext);
    if (p == NULL)
    {
        cout << "Memory allocation failure!\n";
        exit(1);
    }
    return p;
}

template <class T>
void LinkedList<T>::freeNode(Node<T> *p) //释放结点
{
    delete p;
}

template <class T>
void LinkedList<T>::copy(const LinkedList<T>& L) //链表复制函数
{
    Node<T> *p = L.front;   //P用来遍历L 
    int pos;
    while (p != NULL)   //将L中的每一个元素插入到当前链表最后
    {
        insertRear(p->data);
        p = p->nextNode();
    }
    if (position == -1) //如果链表空,返回
        return;
    //在新链表中重新设置prevPtr和currPtr
    prevPtr = NULL;
    currPtr = front;
    for (pos = 0; pos != position; pos++)
    {
        prevPtr = currPtr;
        currPtr = currPtr->nextNode();
    }
}

template <class T>  //构造一个新链表，将有关指针设置为空，size为0，position为-1
LinkedList<T>::LinkedList() : front(NULL), rear(NULL),
prevPtr(NULL), currPtr(NULL), size(0), position(-1)
{}

template <class T>
LinkedList<T>::LinkedList(const LinkedList<T>& L)  //拷贝构造函数
{
    front = rear = NULL;
    prevPtr = currPtr = NULL;
    size = 0;
    position = -1;
    copy(L);
}

template <class T>
LinkedList<T>::~LinkedList()    //析构函数
{
    clear();
}

template <class T>
LinkedList<T>& LinkedList<T>::operator=(const LinkedList<T>& L)//重载"="
{
    if (this == &L) // 不能将链表赋值给它自身
        return *this;
    clear();
    copy(L);
    return *this;
}

template <class T>
int LinkedList<T>::getSize() const  //返回链表大小的函数
{
    return size;
}

template <class T>
bool LinkedList<T>::isEmpty() const //判断链表为空否
{
    return size == 0;
}

template <class T>
void LinkedList<T>::reset(int pos)  //将链表当前位置设置为pos 
{
    int startPos;
    if (front == NULL)  // 如果链表为空，返回
        return;
    if (pos < 0 || pos > size - 1)  // 如果指定位置不合法，中止程序
    {
        std::cerr << "Reset: Invalid list position: " << pos << endl;
        return;
    }
    // 设置与遍历链表有关的成员
    if (pos == 0)   // 如果pos为0，将指针重新设置到表头
    {
        prevPtr = NULL;
        currPtr = front;
        position = 0;
    }
    else    // 重新设置 currPtr, prevPtr, 和 position 
    {
        currPtr = front->nextNode();
        prevPtr = front;
        startPos = 1;
        for (position = startPos; position != pos; position++)
        {
            prevPtr = currPtr;
            currPtr = currPtr->nextNode();
        }
    }
}

template <class T>
void LinkedList<T>::next()  //将prevPtr和currPtr向前移动一个结点
{
    if (currPtr != NULL)
    {
        prevPtr = currPtr;
        currPtr = currPtr->nextNode();
        position++;
    }
}

template <class T>
bool LinkedList<T>::endOfList() const   // 判断是否已达表尾
{
    return currPtr == NULL;
}

template <class T>
int LinkedList<T>::currentPosition() const  // 返回当前结点的位置
{
    return position;
}

template <class T>
void LinkedList<T>::insertFront(const T& item)   // 将item插入在表头
{
    if (front != NULL)  // 如果链表不空则调用Reset 
        reset();
    insertAt(item); // 在表头插入
}

template <class T>
void LinkedList<T>::insertRear(const T& item)   // 在表尾插入结点
{
    Node<T> *nNode;
    prevPtr = rear;
    nNode = newNode(item);  // 创建新结点
    if (rear == NULL)   // 如果表空则插入在表头
        front = rear = nNode;
    else
    {
        rear->insertAfter(nNode);
        rear = nNode;
    }
    currPtr = rear;
    position = size;
    size++;
}

template <class T>
void LinkedList<T>::insertAt(const T& item) // 将item插入在链表当前位置
{
    Node<T> *nNode;
    if (prevPtr == NULL)    // 插入在链表头，包括将结点插入到空表中
    {
        nNode = newNode(item, front);
        front = nNode;
    }
    else    // 插入到链表之中. 将结点置于prevPtr之后
    {
        nNode = newNode(item);
        prevPtr->insertAfter(nNode);
    }
    if (prevPtr == rear)    //正在向空表中插入，或者是插入到非空表的表尾
    {
        rear = nNode;   //更新rear 
        position = size;    //更新position 
    }
    currPtr = nNode;    //更新currPtr
    size++; //使size增值
}

template <class T>
void LinkedList<T>::insertAfter(const T& item)  // 将item 插入到链表当前位置之后
{
    Node<T> *p;
    p = newNode(item);
    if (front == NULL)   // 向空表中插入
    {
        front = currPtr = rear = p;
        position = 0;
    }
    else    // 插入到最后一个结点之后
    {
        if (currPtr == NULL)
            currPtr = prevPtr;
        currPtr->insertAfter(p);
        if (currPtr == rear)
        {
            rear = p;
            position = size;
        }
        else
            position++;
        prevPtr = currPtr;
        currPtr = p;
    }
    size++;              // 使链表长度增值
}

template <class T>
T LinkedList<T>::deleteFront()  // 删除表头结点
{
    T item;
    reset();
    if (front == NULL)
    {
        cerr << "Invalid deletion!" << endl;
        exit(1);
    }
    item = currPtr->data;
    deleteCurrent();
    return item;
}

template <class T>
void LinkedList<T>::deleteCurrent() // 删除链表当前位置的结点
{
    Node<T> *p;
    if (currPtr == NULL)    // 如果表空或达到表尾则出错
    {
        cerr << "Invalid deletion!" << endl;
        exit(1);
    }
    if (prevPtr == NULL)    // 删除将发生在表头或链表之中
    {
        p = front;  // 保存头结点地址
        front = front->nextNode();  //将其从链表中分离
    }
    else    //分离prevPtr之后的一个内部结点，保存其地址
        p = prevPtr->deleteAfter();

    if (p == rear)  // 如果表尾结点被删除
    {
        rear = prevPtr; //新的表尾是prevPtr 
        position--; //position自减
    }
    currPtr = p->nextNode();    // 使currPtr越过被删除的结点
    freeNode(p);    // 释放结点，并
    size--; //使链表长度自减
}

template <class T>
T& LinkedList<T>::data()    //返回一个当前结点数值的引用
{
    if (size == 0 || currPtr == NULL)   // 如果链表为空或已经完成遍历则出错
    {
        cerr << "Data: invalid reference!" << endl;
        exit(1);
    }
    return currPtr->data;
}

template <class T>
void LinkedList<T>::clear() //清空链表
{
    Node<T> *currPosition, *nextPosition;
    currPosition = front;
    while (currPosition != NULL)
    {
        nextPosition = currPosition->nextNode(); //取得下一结点的地址
        freeNode(currPosition); //删除当前结点
        currPosition = nextPosition;    //当前指针移动到下一结点
    }
    front = rear = NULL;
    prevPtr = currPtr = NULL;
    size = 0;
    position = -1;
}
#endif  //LINKEDLIST_H
```

### 6.栈类

```c++
//Stack.h
#ifndef STACK_H
#define STACK_H
#include <cassert> 
template <class T, int SIZE = 50>
class Stack {
private:
    T list[SIZE];
    int top;
public:
    Stack();
    void push(const T &item);
    T pop();
    void clear();
    const T &peek() const;
    bool isEmpty() const;
    bool isFull() const;
};

//模板的实现
template <class T, int SIZE>
Stack<T, SIZE>::Stack() : top(-1) { }   
template <class T, int SIZE>
void Stack<T, SIZE>::push(const T &item) {  
    assert(!isFull());  
    list[++top] = item; 
}
template <class T, int SIZE>
T Stack<T, SIZE>::pop() {   
    assert(!isEmpty()); 
    return list[top--]; 
}
template <class T, int SIZE>
const T &Stack<T, SIZE>::peek() const {
    assert(!isEmpty()); 
    return list[top];   //返回栈顶元素
}
template <class T, int SIZE>
bool Stack<T, SIZE>::isEmpty() const {
    return top == -1;
}
template <class T, int SIZE>
bool Stack<T, SIZE>::isFull() const {   
    return top == SIZE - 1;
}

template <class T, int SIZE>
void Stack<T, SIZE>::clear() {  
    top = -1;
}

#endif  //STACK_H
```



## 第10章 STL

### 1.泛型程序设计的基本概念

#### 泛型程序设计（尽量地将程序设计得通用）

- 编写不依赖于具体数据类型的程序
- 将算法从特定的数据结构中抽象出来，成为通用的
- C++的模板为泛型程序设计奠定了关键的基础

#### 术语：概念

- 用来界定具备一定功能的数据类型。例如：
  - 将“可以比大小的所有数据类型（有比较运算符）”这一概念记为**Comparable**
  - 将“具有公有的复制构造函数并可以用‘=’赋值的数据类型”这一概念记为**Assignable**
  - 将“可以比大小、具有公有的复制构造函数并可以用‘=’赋值的所有数据类型”这个概念记作**Sortable**
- 对于两个不同的概念A和B，如果概念A所需求的所有功能也是概念B所需求的功能，那么就说概念B是概念A的子概念。例如：
  - Sortable既是Comparable的子概念，也是Assignable的子概念

#### 术语：模型

模型（model）：符合一个概念的数据类型称为该概念的模型，例如：

- int型是Comparable概念的模型。
- 静态数组类型不是Assignable概念的模型（无法用“=”给整个静态数组赋值）

#### 用概念做模板参数名

- 很多STL的实现代码就是使用概念来命名模板参数的。

- 为概念赋予一个名称，并使用该名称作为模板参数名。

- 例如

  - 表示insertionSort这样一个函数模板的原型：

  - ```C++
    template <class Sortable>
    void insertionSort(Sortable a[], int n);
    ```

    

### 2.STL简介

#### STL

- 标准模板库（Standard Template Library，简称STL）定义了一套概念体系，为泛型程序设计提供了逻辑基础
- STL中的各个类模板、函数模板的参数都是用这个体系中的概念来规定的。
- 使用STL的模板时，类型参数既可以是C++标准库中已有的类型，也可以是自定义的类型——只要这些类型是所要求概念的模型。

#### STL的基本组件

- 容器（container）
- 迭代器（iterator）
- 函数对象（function object）
- 算法（algorithms）

#### STL的基本组件间的关系

- Iterators（迭代器）是算法和容器的桥梁。
  - 将迭代器作为算法的参数、通过迭代器来访问容器而不是把容器直接作为算法的参数。
- 将**函数对象**作为算法的参数而不是将函数所执行的运算作为算法的一部分。（比如说将一个比较大小的函数，作为函数对象，传给排序算法。那么这个排序算法就可以按照我传过来的这个比较大小的功能去实现具体的排序功能。）
- 使用STL中提供的或自定义的迭代器和函数对象，配合STL的算法，可以组合出各种各样的功能。
- ![image.png](https://i.loli.net/2020/02/03/tlY1rQhWxPymEXb.png)

#### STL的基本组件——容器（container）

- 容纳、包含一组元素的对象。
- 基本容器类模板
  - 顺序容器
    - array（数组）、vector（向量）、deque（双端队列）、forward_list（单链表）、list（列表）
  - (有序)关联容器
    - set（集合）、multiset（多重集合）、map（映射）、multimap（多重映射）
  - 无序关联容器
    - unordered*set （无序集合）、unordered*multiset（无序多重集合）
    - unordered*map（无序映射）、unorder*multimap（无序多重映射）
- 容器适配器
  - stack（栈）、queue（队列）、priority_queue（优先队列）
- 使用容器，需要包含对应的头文件

#### STL的基本组件——迭代器（iterator）

- 迭代器是泛化的指针，提供了顺序访问容器中每个元素的方法
- 提供了顺序访问容器中每个元素的方法；
- 可以使用“++”运算符来获得指向下一个元素的迭代器；
- 可以使用“*”运算符访问一个迭代器所指向的元素，如果元素类型是类或结构体，还可以使用“->”运算符直接访问该元素的一个成员；
- 有些迭代器还支持通过“--”运算符获得指向上一个元素的迭代器；
- 迭代器是泛化的指针：指针也具有同样的特性，因此指针本身就是一种迭代器；
- 使用独立于STL容器的迭代器，需要包含头文件<iterator>。

#### STL的基本组件——函数对象（function object）

- 一个行为类似函数的对象，对它可以像调用函数一样调用。
- 函数对象是泛化的函数：任何普通的函数和任何重载了“()” 运算符的类的对象都可以作为函数对象使用
- 使用STL的函数对象，需要包含头文件<functional>

#### STL的基本组件——算法（algorithms）

- STL包括70多个算法
  - 例如：排序算法，消除算法，计数算法，比较算法，变换算法，置换算法和容器管理等
- 可以广泛用于不同的对象和内置的数据类型。
- 使用STL的算法，需要包含头文件<algorithm>。
- 例10-1从标准输入读入几个整数，存入向量容器，输出它们的相反数

#### 例子

从标准输入读入几个整数，存入向量容器，输出它们的相反数

![image.png](https://i.loli.net/2020/02/03/xQOzBeXkrgKwU9H.png)

#### transform算法的实现

```c++
template <class InputIterator, class OutputIterator, class UnaryFunction>
OutputIterator transform(InputIterator first, InputIterator last, OutputIterator result, UnaryFunction op) {
    for (;first != last; ++first, ++result)
        *result = op(*first);
    return result;
}
```

- transform算法顺序遍历**first**和**last**两个迭代器所指向的元素；
- 将每个元素的值作为函数对象**op**的参数；
- 将op的返回值通过迭代器**result**顺序输出；
- 遍历完成后**result**迭代器指向的是输出的最后一个元素的下一个位置，transform会将该迭代器返回

### 3.迭代器

#### 迭代器

- 迭代器是算法和容器的桥梁
  - 迭代器用作访问容器中的元素
  - 算法不直接操作容器中的数据，而是通过迭代器间接操作
- 算法和容器独立
  - 增加新的算法，无需影响容器的实现
  - 增加新的容器，原有的算法也能适用

#### 输入流迭代器和输出流迭代器

- 输入流迭代器

- ```c++
  istream_iterator<T>
  ```

  - 以输入流（如cin）为参数构造
  - 可用*(p++)获得下一个输入的元素

- 输出流迭代器

- ```c++
  ostream_iterator<T>
  ```

  - 构造时需要提供输出流（如cout）
  - 可用(*p++) = x将x输出到输出流

- 二者都属于适配器

  - 适配器是用来为已有对象提供新的接口的对象
  - 输入流适配器和输出流适配器为流对象提供了迭代器的接口

- 例子

```c++
//从标准输入读入几个实数，分别将它们的平方输出
#include <iterator>
#include <iostream>
#include <algorithm>
using namespace std;

//求平方的函数
double square(double x) {
    return x * x;
}
int main() {
    //从标准输入读入若干个实数，分别将它们的平方输出
    transform(istream_iterator<double>(cin), istream_iterator<double>(),
        ostream_iterator<double>(cout, "\t"), square);
    cout << endl;
    return 0;
}
```

#### 迭代器的分类

![image.png](https://i.loli.net/2020/02/03/43s8CdFiETKhjoV.png)

#### 迭代器支持的操作

- 迭代器是泛化的指针，提供了类似指针的操作（诸如++、*、->运算符）
- 输入迭代器
  - 可以用来从序列中读取数据，如输入流迭代器
- 输出迭代器
  - 允许向序列中写入数据，如输出流迭代器
- 前向迭代器
  - 既是输入迭代器又是输出迭代器，并且可以对序列进行单向的遍历
- 双向迭代器
  - 与前向迭代器相似，但是在两个方向上都可以对数据遍历
- 随机访问迭代器
  - 也是双向迭代器，但能够在序列中的任意两个位置之间进行跳转，如指针、使用vector的begin()、end()函数得到的迭代器

#### 迭代器的区间

- 两个迭代器表示一个区间：[p1, p2)
- STL算法常以迭代器的区间作为输入，传递输入数据
- 合法的区间
  - p1经过n次(n > 0)自增(++)操作后满足p1 == p2
- 区间包含p1，但不包含p2

```c++
#include <algorithm>
#include <iterator>
#include <vector>
#include <iostream>
using namespace std;

//将来自输入迭代器的n个T类型的数值排序，将结果通过输出迭代器result输出
template <class T, class InputIterator, class OutputIterator>
void mySort(InputIterator first, InputIterator last, OutputIterator result) {
    //通过输入迭代器将输入数据存入向量容器s中
    vector<T> s;
    for (;first != last; ++first)
        s.push_back(*first);
    //对s进行排序，sort函数的参数必须是随机访问迭代器
    sort(s.begin(), s.end());  
    copy(s.begin(), s.end(), result);   //将s序列通过输出迭代器输出
}

int main() {
    //将s数组的内容排序后输出
    double a[5] = { 1.2, 2.4, 0.8, 3.3, 3.2 };
    mySort<double>(a, a + 5, ostream_iterator<double>(cout, " "));
    cout << endl;
    //从标准输入读入若干个整数，将排序后的结果输出
    mySort<int>(istream_iterator<int>(cin), istream_iterator<int>(), ostream_iterator<int>(cout, " "));
    //依次从键盘输入数据，以空格分隔，ctrl+z作为结束
    cout << endl;
      return 0;
}
/*
运行结果：
0.8 1.2 2.4 3.2 3.3
2 -4 5 8 -1 3 6 -5
-5 -4 -1 2 3 5 6 8
*/
```

#### 迭代器的辅助函数

- advance(p, n)
  - 对p执行n次自增操作
- distance(first, last)
  - 计算两个迭代器first和last的距离，即对first执行多少次“++”操作后能够使得first == last

### 4.容器的基本功能与分类

#### 容器的基本功能与分类

- 容器类是容纳、包含一组元素或元素集合的对象。
- 基于容器中元素的组织方式：顺序容器、关联容器
- 按照与容器所关联的迭代器类型划分：可逆容器、随机访问容器

#### 容器

- 顺序容器
  - array（数组）、vector（向量）、deque（双端队列）、forward_list（单链表）、list（列表）
- (有序)关联容器
  - set（集合）、multiset（多重集合）、map（映射）、multimap（多重映射）
- 无序关联容器
  - unordered*set （无序集合）、unordered*multiset（无序多重集合）
  - unordered*map（无序映射）、unorder*multimap（无序多重映射）

#### 容器的分类

![image.png](https://i.loli.net/2020/02/03/kzoFgH4G1LfKj7p.png)

![image.png](https://i.loli.net/2020/02/03/enPjOSCN3WYEiuX.png)

#### 容器的通用功能

- 容器的通用功能
  - 用默认构造函数构造空容器
  - 支持关系运算符：==、!=、<、<=、>、>=
  - begin()、end()：获得容器首、尾迭代器
  - clear()：将容器清空
  - empty()：判断容器是否为空
  - size()：得到容器元素个数
  - s1.swap(s2)：将s1和s2两容器内容交换
- 相关数据类型（S表示容器类型）
  - S::iterator：指向容器元素的迭代器类型
  - S::const_iterator：常迭代器类型

#### 对可逆容器的访问(可以用rbegin,rend的容器)

- STL为每个可逆容器都提供了逆向迭代器，逆向迭代器可以通过下面的成员函数得到：
  - rbegin() ：指向容器尾的逆向迭代器
  - rend()：指向容器首的逆向迭代器
- 逆向迭代器的类型名的表示方式如下：
  - S::reverse_iterator：逆向迭代器类型
  - S::const*reverse*iterator：逆向常迭代器类型

#### 随机访问容器（像数组一样）

随机访问容器支持对容器的元素进行随机访问

- s[n]：获得容器s的第n个元素

### 5.顺序容器

#### 随机访问：就是通过下标随意访问

#### 顺序容器的基本功能

**顺序容器**

- STL中的顺序容器
  - 向量（vector）
  - 双端队列（deque）
  - 列表（list）
  - 单向链表（forward_list） （以上四种在逻辑上可看作是一个长度可扩展的数组）
  - 数组（array）:（数组容器长度是固定的）
- 元素线性排列，可以随时在指定位置插入元素和删除元素。
- 必须符合Assignable这一概念（即具有公有的拷贝构造函数并可以用“=”赋值）。
- 有两个容器比较特殊：
  - array对象的大小固定，forward_list有特殊的添加和删除操作。

**顺序容器的接口（不包含单向链表（forward_list）和数组（array））**

- 构造函数
- 赋值函数
  - assign:   示例：`s.assign(l.begin(), l.end());`
- 插入函数
  - insert， push_front（只对list和deque）， push_back，emplace，emplace_front
- 删除函数
  - erase，clear，pop_front（只对list和deque） ，pop_back，emplace_back
- 首尾元素的直接访问
  - front，back
- 改变大小
  - resize

```c++
#include <iostream>
#include <list>
#include <deque>

//输出指定的顺序容器的元素
template <class T>
void printContainer(const char* msg, const T& s) {
    cout << msg << ": ";
    copy(s.begin(), s.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
}

int main() {
    //从标准输入读入10个整数，将它们分别从s的头部加入
    deque<int> s;
    for (int i = 0; i < 10; i++) {
        int x;
        cin >> x;
        s.push_front(x);
    }
    printContainer("deque at first", s);
    //用s容器的内容的逆序构造列表容器l
    list<int> l(s.rbegin(), s.rend());
    printContainer("list at first", l);

    //将列表容器l的每相邻两个元素顺序颠倒
    list<int>::iterator iter = l.begin();
    while (iter != l.end()) {
        int v = *iter;  
        iter = l.erase(iter);
        l.insert(++iter, v);
    }
    printContainer("list at last", l);
    //用列表容器l的内容给s赋值，将s输出
    s.assign(l.begin(), l.end());
    printContainer("deque at last", s);
    return 0;
}
/*  
运行结果如下： 
0 9 8 6 4 3 2 1 5 4
deque at first: 4 5 1 2 3 4 6 8 9 0
list at first: 0 9 8 6 4 3 2 1 5 4
list at last: 9 0 6 8 3 4 1 2 4 5
deque at last: 9 0 6 8 3 4 1 2 4 5
/*
```

#### 顺序容器的特性

- 顺序容器：向量、双端队列、列表、单向链表、数组

**向量（Vector）**

- 特点
  - 一个可以扩展的动态数组
  - 随机访问、在尾部插入或删除元素快
  - 在中间或头部插入或删除元素慢
- 向量的容量
  - 容量(capacity)：实际分配空间的大小
  - s.capacity() ：返回当前容量
  - s.reserve(n)：若容量小于n，则对s进行扩展，使其容量至少为n

**双端队列（deque）**

- 特点
  - 在两端插入或删除元素快
  - 在中间插入或删除元素慢
  - 随机访问较快，但比向量容器慢

```c++
//奇偶排序
//先按照从大到小顺序输出奇数，再按照从小到大顺序输出偶数。
//1 2 3 4 5 6 7 8
//7 5 3 1 2 4 6 8
int main() {
    istream_iterator<int> i1(cin), i2;  //建立一对输入流迭代器
    vector<int> s1(i1, i2); //通过输入流迭代器从标准输入流中输入数据
    sort(s1.begin(), s1.end()); //将输入的整数排序
    deque<int> s2;
    //以下循环遍历s1
    for (vector<int>::iterator iter = s1.begin(); iter != s1.end(); ++iter) 
    {
         if (*iter % 2 == 0)    //偶数放到s2尾部
             s2.push_back(*iter);
         else       //奇数放到s2首部
             s2.push_front(*iter);
    }
    //将s2的结果输出
    copy(s2.begin(), s2.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}
```

**列表（List)**

- 特点
  - 在任意位置插入和删除元素都很快
  - **不支持随机访问**
- 接合(splice)操作
  - s1.splice(p, s2, q1, q2)：将s2中[q1, q2)移动到s1中p所指向元素之前（或者说该指向位置）
  - 这是**剪切**操作
  - 比如：底下的s2.splice(s2.end(), s1, s1.begin());将s1的第一个元素放到s2的最后
  - s1变成了："Helen", "Lucy", "Susan"
  - s2变成了："Bob", "David", "Levin", "Mike" ,\"Alice"

```c++
int main() {
    string names1[] = { "Alice", "Helen", "Lucy", "Susan" };
    string names2[] = { "Bob", "David", "Levin", "Mike" };
    //用names1数组的内容构造列表s1
    list<string> s1(names1, names1 + 4); 
    //用names2数组的内容构造列表s2
    list<string> s2(names2, names2 + 4); 

    //将s1的第一个元素放到s2的最后
    s2.splice(s2.end(), s1, s1.begin());
    list<string>::iterator iter1 = s1.begin(); //iter1指向s1首
    advance(iter1, 2); //iter1前进2个元素，它将指向s1第3个元素
    list<string>::iterator iter2 = s2.begin();  //iter2指向s2首
    ++iter2; //iter2前进1个元素，它将指向s2第2个元素
    list<string>::iterator iter3 = iter2; //用iter2初始化iter3
    advance(iter3, 2); //iter3前进2个元素，它将指向s2第4个元素
    //将[iter2, iter3)范围内的结点接到s1中iter1指向的结点前
    s1.splice(iter1, s2, iter2, iter3); 

    //分别将s1和s2输出
    copy(s1.begin(), s1.end(), ostream_iterator<string>(cout, " "));
    cout << endl;
    copy(s2.begin(), s2.end(), ostream_iterator<string>(cout, " "));
    cout << endl;
    return 0;
}
//Helen Lucy David Levin Suan
//Bob Mike Alice
```

**单向链表(forward_list)**

- 单向链表每个结点只有指向下个结点的指针，没有简单的方法来获取一个结点的前驱；
- 未定义insert、emplace和erase操作，而定义了insert_after、emplace_after和erase_after操作，其参数与list的insert、emplace和erase相同，但并不是插入或删除迭代器p1所指的元素，而是对迭代器p1所指元素之后的结点进行操作；
- 不支持size操作。

**数组(array)**

- array是**对内置数组的封装**，提供了更安全，更方便的使用数组的方式
- array的对象的大小是固定的，定义时除了需要指定元素类型，还需要指定容器大小。
- 不能动态地改变容器大小

**顺序容器的比较**

STL所提供的顺序容器各有所长也各有所短，我们在编写程序时应当根据我们对容器所需要执行的操作来决定选择哪一种容器。

- 如果需要执行大量的随机访问操作，而且当扩展容器时只需要向容器尾部加入新的元素，就应当选择向量容器vector；
- 如果需要少量的随机访问操作，需要在容器两端插入或删除元素，则应当选择双端队列容器deque；
  - 如果不需要对容器进行随机访问，但是需要在中间位置插入或者删除元素，就应当选择列表容器list或forward_list；
- 如果需要数组，array相对于内置数组类型而言，是一种更安全、更容易使用的数组类型。

#### 顺序容器的插入迭代器与适配器

**顺序容器的插入迭代器**

- 用于向容器头部、尾部或中间指定位置插入元素的迭代器
- 包括前插迭代器（front_inserter）、后插迭代器（back_inserter）和任意位置插入迭代器（inserter）
- 例：

```c++
list<int> s;
back_inserter iter(s);
*(iter++) = 5; //通过iter把5插入s末尾
```

**顺序容器的适配器**

以顺序容器为基础构建一些常用数据结构，是对顺序容器的封装

- 栈(stack)：最先压入的元素最后被弹出
- 队列(queue)：最先压入的元素最先被弹出
- 优先级队列(priority_queue)：最“大”的元素最先被弹出

**栈和队列模板**

栈=基础顺序容器+栈适配器

队列=支持前插的基础顺序容器+队列适配器

- 栈模板

  - ```c++
    template <class T, class Sequence = deque<T> > class stack;
    ```

- 队列模板

  - ```c++
    template <class T, class FrontInsertionSequence = deque<T> > class queue;
    ```

- **栈**可以用任何一种**顺序容器作为基础容器**，而**队列**只允许用**前插顺序容器（双端队列或列表）**

**栈和队列共同支持的操作**

- s1 op s2 op可以是==、!=、<、<=、>、>=之一，它会对两个容器适配器之间的元素按字典序进行比较
- s.size() 返回s的元素个数
- s.empty() 返回s是否为空
- s.push(t) 将元素t压入到s中
- s.pop() 将一个元素从s中弹出，对于栈来说，每次弹出的是最后被压入的元素，而对于队列，每次被弹出的是最先被压入的元素
- **不支持迭代器，因为它们不允许对任意元素进行访问**

**栈和队列不同的操作**

- 栈的操作
  - s.top() 返回栈顶元素的引用
- 队列操作
  - s.front() 获得队头元素的引用
  - s.back() 获得队尾元素的引用

```c++
//利用栈反向输出单词
int main() {
    stack<char> s;
    string str;
    cin >> str; //从键盘输入一个字符串
    //将字符串的每个元素顺序压入栈中
    for (string::iterator iter = str.begin(); iter != str.end(); ++iter)
        s.push(*iter);
    //将栈中的元素顺序弹出并输出
    while (!s.empty()) {
        cout << s.top();
        s.pop();
    }
    cout << endl;
    return 0;
}
运行结果如下：
congratulations
snoitalutargnoc
```

**优先级队列**

- 优先级队列也像栈和队列一样支持元素的压入和弹出，但元素弹出的顺序与元素的大小有关，每次弹出的总是容器中最“大”的一个元素。

  - ```c++
    template <class T, class Sequence = vector<T> > class priority_queue;
    ```

- 优先级队列的基础容器必须是支持随机访问的顺序容器。

- 支持栈和队列的size、empty、push、pop几个成员函数，用法与栈和队列相同。

- 优先级队列并不支持比较操作。

- 与栈类似，优先级队列提供一个top函数，可以获得下一个即将被弹出元素（即最“大”的元素）的引用。

```c++
//细胞分裂模拟
//一种细胞在诞生（即上次分裂）后会在500到2000秒内分裂为两个细胞，每个细胞又按照同样的规律继续分裂。
const int SPLIT_TIME_MIN = 500;    //细胞分裂最短时间
const int SPLIT_TIME_MAX = 2000;  //细胞分裂最长时间

class Cell;
priority_queue<Cell> cellQueue;

class Cell {    //细胞类
private:
    static int count;   //细胞总数
    int id;     //当前细胞编号
    int time;   //细胞分裂时间
public:
    Cell(int birth) : id(count++) { //birth为细胞诞生时间
        //初始化，确定细胞分裂时间
        time = birth + (rand() % (SPLIT_TIME_MAX - SPLIT_TIME_MIN))+ SPLIT_TIME_MIN;
    }
    int getId() const { return id; }        //得到细胞编号
    int getSplitTime() const { return time; }   //得到细胞分裂时间
    bool operator < (const Cell& s) const      //定义“<”
    { return time > s.time; }
    void split() {  //细胞分裂
        Cell child1(time), child2(time);    //建立两个子细胞
        cout << time << "s: Cell #" << id << " splits to #"
        << child1.getId() << " and #" << child2.getId() << endl;
        cellQueue.push(child1); //将第一个子细胞压入优先级队列
        cellQueue.push(child2); //将第二个子细胞压入优先级队列
    }
};
int Cell::count = 0;
int main() {
    srand(static_cast<unsigned>(time(0)));
    int t;  //模拟时间长度
    cout << "Simulation time: ";
    cin >> t;
    cellQueue.push(Cell(0));    //将第一个细胞压入优先级队列
    while (cellQueue.top().getSplitTime() <= t) {
        cellQueue.top().split();    //模拟下一个细胞的分裂
        cellQueue.pop();    //将刚刚分裂的细胞弹出
    }
    return 0;
}
/*
运行结果如下：
Simulation time: 5000
971s: Cell #0 splits to #1 and #2
1719s: Cell #1 splits to #3 and #4
1956s: Cell #2 splits to #5 and #6
2845s: Cell #6 splits to #7 and #8
3551s: Cell #3 splits to #9 and #10
3640s: Cell #4 splits to #11 and #12
3919s: Cell #5 splits to #13 and #14
4162s: Cell #10 splits to #15 and #16
4197s: Cell #8 splits to #17 and #18
4317s: Cell #7 splits to #19 and #20
4686s: Cell #13 splits to #21 and #22
4809s: Cell #12 splits to #23 and #24
4818s: Cell #17 splits to #25 and #26
*/
```

### 6.关联容器

#### 关联容器的分类和基本功能

**关联容器的特点和接口**

- 关联容器的特点
  - 每个关联容器都有一个键(key)
  - 可以根据键高效地查找元素
- 接口
  - 插入：insert
  - 删除：erase
  - 查找：find
  - 定界：lower_bound、upper_bound、equal_range
  - 计数：count

**关联容器概念图**

![image.png](https://i.loli.net/2020/04/04/JM9UwR1iPxdnX4u.png)

**四种关联容器**

- 单重关联容器(set和map)
  - 键值是唯一的，一个键值只能对应一个元素
- 多重关联容器(multiset和multimap)
  - 键值是不唯一的，一个键值可以对应多个元素
- 简单关联容器(set和multiset)
  - 容器只有一个类型参数，如set、multiset，表示键类型
  - 容器的元素就是键本身
- 二元关联容器(map和multimap)
  - 容器有两个类型参数，如map、multimap，分别表示键和附加数据的类型
  - 容器的元素类型是pair，即由键类型和元素类型复合而成的二元组

**无序关联容器**

- C++11新标准中定义了4个无序关联容器
  - unordered_set、unordered_map、unordered_multiset、unordered_multimap
- 不是使用比较运算符来组织元素的，而是通过一个哈希函数和键类型的==运算符。
- 提供了与有序容器相同的操作
- 可以直接定义关键字是内置类型的无序容器。
- 不能直接定义关键字类型为自定义类的无序容器，如果需要，必须提供我们自己的hash模板。

#### 集合(set)

集合用来存储一组无重复的元素。由于集合的元素本身是有序的，可以高效地查找指定元素，也可以方便地得到指定大小范围的元素在容器中所处的区间。

```c++
//输入一串实数，将重复的去掉，取最大和最小者的中值，分别输出小于等于此中值和大于等于此中值的实数
#include <set>
#include <iterator>
#include <utility>
#include <iostream>
using namespace std;

int main() {
    set<double> s;
    while (true) {
        double v;
        cin >> v;
        if (v == 0) break;  //输入0表示结束
        //尝试将v插入
       pair<set<double>::iterator,bool> r=s.insert(v); 
        if (!r.second)  //如果v已存在，输出提示信息
           cout << v << " is duplicated" << endl;
    }
  //得到第一个元素的迭代器
    set<double>::iterator iter1=s.begin();
    //得到末尾的迭代器
    set<double>::iterator iter2=s.end();    
  //得到最小和最大元素的中值    
    double medium=(*iter1 + *(--iter2)) / 2;    
    //输出小于或等于中值的元素
    cout<< "<= medium: "
    copy(s.begin(), s.upper_bound(medium), ostream_iterator<double>(cout, " "));
    cout << endl;
    //输出大于或等于中值的元素
    cout << ">= medium: ";
    copy(s.lower_bound(medium), s.end(), ostream_iterator<double>(cout, " "));
    cout << endl;
    return 0;
}
运行结果如下：
  1 2.5 5 3.5 5 7 9 2.5 0
  5 is duplicated
  2.5 is duplicated
  <= medium: 1 2.5 3.5 5
  >= medium: 5 7 9
```

#### 映射(map)

- 映射与集合同属于单重关联容器，它们的主要区别在于，集合的元素类型是键本身，而映射的元素类型是由键和附加数据所构成的二元组。
- 在集合中按照键查找一个元素时，一般只是用来确定这个元素是否存在，而在映射中按照键查找一个元素时，除了能确定它的存在性外，还可以得到相应的附加数据。

```c++
//有五门课程，每门都有相应学分，从中选择三门，输出学分总和
#include <iostream>
#include <map>
#include <string>
#include <utility>
using namespace std;
int main() {
    map<string, int> courses;
    //将课程信息插入courses映射中
    courses.insert(make_pair("CSAPP", 3));
    courses.insert(make_pair("C++", 2));
    courses.insert(make_pair("CSARCH", 4));
    courses.insert(make_pair("COMPILER", 4));
    courses.insert(make_pair("OS", 5));
    int n = 3;      //剩下的可选次数
    int sum = 0;    //学分总和
    while (n > 0) {
        string name;
        cin >> name;    //输入课程名称
        map<string, int>::iterator iter = courses.find(name);//查找课程
        if (iter == courses.end()) {    //判断是否找到
            cout << name << " is not available" << endl;
        } else {
            sum += iter->second;    //累加学分
            courses.erase(iter);    //将刚选过的课程从映射中删除
            n--;
        }
    }
    cout << "Total credit: " << sum << endl;    //输出总学分
    return 0;
}
运行结果如下：
C++
COMPILER
C++
C++ is not available
CSAPP
Total credit: 9    
```

```c++
//统计一句话中每个字母出现的次数
#include <iostream>
#include <map>
#include <cctype>
using namespace std;
int main() {
    map<char, int> s;   //用来存储字母出现次数的映射
    char c;     //存储输入字符
    do {
      cin >> c; //输入下一个字符
      if (isalpha(c)){ //判断是否是字母
          c = tolower(c); //将字母转换为小写
          s[c]++;      //将该字母的出现频率加1
      }
    } while (c != '.'); //碰到“.”则结束输入
    //输出每个字母出现次数
    for (map<char, int>::iterator iter = s.begin(); iter != s.end(); ++iter)
        cout << iter->first << " " << iter->second << "  ";
    cout << endl;
    return 0;
}
```

#### 多重集合和多重映射

- 多重集合是允许有重复元素的集合，多重映射是允许一个键对应多个附加数据的映射。
- 多重集合与集合、多重映射与映射的用法差不多，只在几个成员函数上有细微差异，其差异主要表现在去除了键必须唯一的限制。

```c++
//上课时间查询
#include <iostream>
#include <map>
#include <utility>
#include <string>
using namespace std;
int main() {
    multimap<string, string> courses;
    typedef multimap<string, string>::iterator CourseIter;

    //将课程上课时间插入courses映射中
    courses.insert(make_pair("C++", "2-6"));
    courses.insert(make_pair("COMPILER", "3-1"));
    courses.insert(make_pair("COMPILER", "5-2"));
    courses.insert(make_pair("OS", "1-2"));
    courses.insert(make_pair("OS", "4-1"));
    courses.insert(make_pair("OS", "5-5"));
    //输入一个课程名，直到找到该课程为止，记下每周上课次数
    string name;
    int count;
    do {
        cin >> name;
        count = courses.count(name);
        if (count == 0)
          cout << "Cannot find this course!" << endl;
    } while (count == 0);
    //输出每周上课次数和上课时间
    cout << count << " lesson(s) per week: ";
    pair<CourseIter, CourseIter> range = courses.equal_range(name);
    for (CourseIter iter = range.first; iter != range.second; ++iter)
        cout << iter->second << " ";
    cout << endl;

    return 0;
}
运行结果如下：
JAVA
Cannot find this course!
OS
3 lesson(s) per week: 1-2 4-1 5-5
```

### 7.函数对象

#### 函数对象

- 一个行为类似函数的对象

- 可以没有参数，也可以带有若干参数

- 其功能是获取一个值，或者改变操作的状态。

- 例

- - 普通函数就是函数对象
  - 重载了“()”运算符的类的实例是函数对象

**函数对象概念图**

![G08k0P.png](https://s1.ax1x.com/2020/04/04/G08k0P.png)

- 使用两种方式**定义**表示乘法的**函数对象**

- - **通过定义普通函数**（例10-13）
  - **通过重载 类的“()”运算符**（例10-14）

- 用到以下算法：

- ```c++
  template<class InputIterator, class Type, class BinaryFunction>
  Type accumulate(InputIterator first, InputIterator last, Type val, BinaryFunction binaryOp);
  ```

  - 对[first, last)区间内的数据进行累“加”，binaryOp为用二元函数对象表示的“加”运算符，val为累“加”的初值

```c++
//例10-13
//定义普通函数
//过程是，第一次mult(1,1)=1,然后mult(1,2)=2,mult(2,3)=6,mult(6,4)=24,mult(24,5)=120;最后返回120
#include <iostream>   
#include <numeric> //包含数值算法头文件
using namespace std;

//定义一个普通函数
int mult(int x, int y) { return x * y; };   

int main() {
    int a[] = { 1, 2, 3, 4, 5 };
    const int N = sizeof(a) / sizeof(int);
    cout << "The result by multipling all elements in a is "
        << accumulate(a, a + N, 1, mult)
        << endl;
    return 0;
}
```

```c++
//例10-14
//定义一个类，重载运算符();这也算是函数对象
#include <iostream>
#include <numeric> //包含数值算法头文件
using namespace std;
class MultClass{  //定义MultClass类
public:
  //重载操作符operator()
    int operator() (int x, int y) const { return x * y; }   
};
int main() {
    int a[] = { 1, 2, 3, 4, 5 };
    const int N = sizeof(a) / sizeof(int);
    cout << "The result by multipling all elements in a is "
        << accumulate(a, a + N, 1, MultClass()) //将类multclass传递给通用算法
        << endl;
    return 0;
}
```

**STL提供的函数对象（有一些常用的stl封装好了，不用自己写，比如加减乘除等等）**

- 用于算术运算的函数对象：
- - 一元函数对象(一个参数) ：negate
  - 二元函数对象(两个参数) ：plus、minus、multiplies、divides、modulus
- 用于关系运算、逻辑运算的函数对象(要求返回值为bool)

- - 一元谓词(一个参数)：logical_not
  - 二元谓词(两个参数)：equal_to、not_equal_to、greater、less、greater_equal、less_equal、logical_and、logical_or
  - //所谓几元，就是函数有几个输入变量
    //所谓谓词，就是返回值是bool类型的函数。

```c++
//利用STL标准函数对象
#include <iostream>   
#include <numeric>   //包含数值算法头文件
#include <functional>  //包含标准函数对象头文件
using namespace std;    
int main() {
    int a[] = { 1, 2, 3, 4, 5 };
    const int N = sizeof(a) / sizeof(int);
    cout << "The result by multipling all elements in A is “
            << accumulate(a, a + N, 1, multiplies<int>())
         << endl; //将标准函数对象传递给通用算法
    return 0;
}
```

```c++
//利用STL中的二元谓词函数对象，实现数组由大到小排序功能
//greater<int>()就是一个二元谓词函数对象。
//所谓几元，就是函数有几个输入变量
//所谓谓词，就是返回值是bool类型的函数。
#include <functional>
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main() {
    int intArr[] = { 30, 90, 10, 40, 70, 50, 20, 80 };
    const int N = sizeof(intArr) / sizeof(int);
    vector<int> a(intArr, intArr + N);
    cout << "before sorting:" << endl;
    copy(a.begin(),a.end(),ostream_iterator<int>(cout,"\t"));
    cout << endl;

    sort(a.begin(), a.end(), greater<int>());

    cout << "after sorting:" << endl;
    copy(a.begin(),a.end(),ostream_iterator<int>(cout,"\t"));
    cout << endl;
    return 0;
}
```

#### 函数适配器

- 绑定适配器：bind1st、bind2nd
  - 将n元函数对象的指定参数绑定为一个常数，得到n-1元函数对象
- 组合适配器：not1、not2
  - 将指定谓词的结果取反
- 函数指针适配器：ptr_fun
  - 将一般函数指针转换为函数对象，使之能够作为其它函数适配器的输入。
  - 在进行参数绑定或其他转换的时候，通常需要函数对象的类型信息，例如bind1st和bind2nd要求函数对象必须继承于binary_function类型。但如果传入的是函数指针形式的函数对象，则无法获得函数对象的类型信息。
- 成员函数适配器：ptr_fun、ptr_fun_ref
  - 对成员函数指针使用，把n元成员函数适配为n + 1元函数对象，该函数对象的第一个参数为调用该成员函数时的目的对象
  - 也就是需要将“object->method()”转为“method(object)”形式。将“object->method(arg1)”转为二元函数“method(object, arg1)”。

**绑定适配器**

- binder2nd的实例构造通常比较冗长，bind2nd函数用于辅助构造binder2nd，产生它的一个实例。
- binder1st和bind1st，将一个具体值绑定到二元函数的第一个参数。

```c++
//函数适配器实例——找到数组中第一个大于40的元素
#include <functional>
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main() {
    int intArr[] = { 30, 90, 10, 40, 70, 50, 20, 80 };
    const int N = sizeof(intArr) / sizeof(int);
    vector<int> a(intArr, intArr + N);
    //find_if的UnaryPredicate pred要求是一元谓词，但是greater<int>()是二元谓词，因此用bind2nd把第二个参数给它用40绑定到第二个参数上。
    vector<int>::iterator p = find_if(a.begin(), a.end(), bind2nd(greater<int>(), 40));
    if (p == a.end())
        cout << "no element greater than 40" << endl;
    else
        cout << "first element greater than 40 is: " << *p << endl;
    return 0;
}

注：
find_if算法在STL中的原型声明为：
template<class InputIterator, class UnaryPredicate>
InputIterator find_if(InputIterator first, InputIterator last, UnaryPredicate pred);
它的功能是查找数组[first, last)区间中第一个pred(x)为真的元素。
```

**组合适配器**

- 对于一般的逻辑运算，有时可能还需要对结果求一次逻辑反。
- unary_negate和binary_negate实现了这一适配功能。STL还提供了**not1**和**not2**辅助生成相应的函数对象实例，分别用于**一元谓词和二元谓词的逻辑取反。**

```c++
//ptr_fun、not1和not2产生函数适配器实例
#include <functional>
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

bool g(int x, int y) {
    return x > y;
}//跟greater<int>(x,y)其实是一样的

int main() {
    int intArr[] = { 30, 90, 10, 40, 70, 50, 20, 80 };
    const int N = sizeof(intArr) / sizeof(int);
    vector<int> a(intArr, intArr + N);
    vector<int>::iterator p;
    p = find_if(a.begin(), a.end(), bind2nd(ptr_fun(g), 40));
    if (p == a.end())
        cout << "no element greater than 40" << endl;
    else
        cout << "first element greater than 40 is: " << *p << endl;
    p = find_if(a.begin(), a.end(), not1(bind2nd(greater<int>(), 15)));
    if (p == a.end())
        cout << "no element is not greater than 15" << endl;
    else
        cout << "first element that is not greater than 15 is: " << *p << endl;

    p = find_if(a.begin(), a.end(), bind2nd(not2(greater<int>()), 15));//跟上边用not1的效果一致
    if (p == a.end())
        cout << "no element is not greater than 15" << endl;
    else
        cout << "first element that is not greater than 15 is: " << *p << endl;
    return 0;
}
```

```c++
//成员函数适配器实例
#include <functional>
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Car {
    int id;
    Car(int id) { this->id = id; }
    void display() const { cout << "car " << id << endl; }
};

int main() {
    vector<Car *> pcars;
    vector<Car> cars;
    for (int i = 0; i < 5; i++)
        pcars.push_back(new Car(i));
    for (int i = 5; i < 10; i++)
        cars.push_back(Car(i));
    cout << "elements in pcars: " << endl;
    for_each(pcars.begin(), pcars.end(), std::mem_fun(&Car::display));
    //pcars存的是对象指针，用men_fun()适配出来的就是对象指针
    cout << endl;

    cout << "elements in cars: " << endl;
    for_each(cars.begin(), cars.end(), std::mem_fun_ref(&Car::display));
    //cars存的是对象，mem_fun_ref()适配出来的是对象引用
    cout << endl;

    for (size_t i = 0; i < pcars.size(); ++i)
        delete pcars[i];

    return 0;
}
```

### 8.算法

#### STL算法特点

- STL算法本身是一种函数模版

- - 通过迭代器获得输入数据
  - 通过函数对象对数据进行处理
  - 通过迭代器将结果输出

- STL算法是通用的，独立于具体的数据类型、容器类型

#### STL算法分类

- 不可变序列算法
- 可变序列算法
- 排序和搜索算法
- 数值算法

#### 不可变序列算法

- 不直接修改所操作的容器内容的算法

- 用于查找指定元素、比较两个序列是否相等、对元素进行计数等

- 例：

  - ```c++
    template<class InputIterator, class UnaryPredicate>
    InputIterator find_if(InputIterator first, InputIterator last, UnaryPredicate pred);
    ```

  - 查找[first, last)区间内pred(x)为真的首个元素

#### 可变序列算法

- 可以修改它们所操作的容器对象

- 包括对序列进行复制、删除、替换、倒序、旋转、交换、分割、去重、填充、洗牌的算法及生成一个序列的算法

- 例：

  - ```c++
    template<class ForwardIterator, class T>
    InputIterator find_if(ForwardIterator first, ForwardIterator last, const T& x);
    ```

  - 将[first, last)区间内的元素全部改写为x。

#### 排序和搜索算法

- 对序列进行排序

- 对两有序序列进行合并

- 对有序序列进行搜索

- 有序序列的集合操作

- 堆算法

- 例：

  - ```c++
    template <class RandomAccessIterator , class UnaryPredicate>
    void sort(RandomAccessIterator first, RandomAccessIterator last, UnaryPredicate comp);
    ```

  - 以函数对象comp为“<”，对 [first, last)区间内的数据进行排序

#### 数值算法

- 求序列中元素的“和”、部分“和”、相邻元素的“差”或两序列的内积

- 求“和”的“+”、求“差”的“-”以及求内积的“+”和“·”都可由函数对象指定

- 例：

  - ```c++
    template<class InputIterator, class OutputIterator, class BinaryFunction>
    
    OutputIterator partial_sum(InputIterator first, InputIterator last, OutputIterator result, BinaryFunction op);
    ```

  - 对[first, last)内的元素求部分“和”（所谓部分“和”，是一个长度与输入序列相同的序列，其第n项为输入序列前n个元素的“和”），以函数对象op为“+”运算符，结果通过result输出，返回的迭代器指向输出序列最后一个元素的下一个元素

#### 算法应用举例

- 例10-20——例10-23演示了几类算法的应用。
- 详见教材第10章

## 第11章 流类库与输入/输出

### 1.I/O流的概念及流类库结构

**程序与外界环境的信息交换**

- 当程序与外界环境进行信息交换时，存在着两个对象：程序中的对象、文件对象。 
- 流是信息流动的一种抽象，负责在数据的生产者和数据的消费者之间建立联系，并管理数据的流动。
  - 比如从程序空间流向外部设备（显示器），从外部设备（键盘）流向程序空间。这就是输出流，输入流

**流对象与文件操作**

- 程序建立一个流对象
- 指定这个流对象与某个文件对象建立连接
- 程序操作流对象
- 流对象通过文件系统对所连接的文件对象产生作用。

**提取与插入**

- 读操作在流数据抽象中被称为（从流中）提取
- 写操作被称为（向流中）插入。

**流类库结构**

![NNye76.png](https://s1.ax1x.com/2020/06/23/NNye76.png)

**流类列表**

![NNyVn1.png](https://s1.ax1x.com/2020/06/23/NNyVn1.png)

### 2.输出流

#### 2.1输出流概述

**最重要的三个输出流**

- ostream
- ofstream
- ostringstream

**预先定义的输出流对象**

默认情况下都指向标准输出设备。写程序时可以根据需要，将这3个输出流对象重新定向到不同的地方去，比如让cout往文件里输出，cerr往显示器上输出紧急的错误信息，clog输出到日志中

- cout 标准输出流对象
- cerr 标准错误输出流对象，没有缓冲，发送给它的内容立即被输出。
- clog 类似于cerr，但是有缓冲，缓冲区满时被输出。

**标准输出换向**

暂时地重定向某个标准输出流（cout、cerr、clog都可以）

```c++
ofstream fout("b.out");//将文件输出流对象fout绑定到文件“b.out”上
streambuf*  pOld  =cout.rdbuf(fout.rdbuf());  //将当前输出流cout的指向暂存起来（保存到pOld）中，然后将cout指向fout.rdbuf()，也就是"b.out"

//..接下来cout的输出都将输出到"b.out"中，不输出到显示器上了
//..
//往"b.out"的输出完成了，你又想恢复到标准输出的时候，
cout.rdbuf(pOld);//恢复原来的标准输出，也就是显示器
```

**构造输出流对象**

- ofstream类支持磁盘文件输出

- 方法1：如果在构造函数中指定一个文件名，当构造这个文件时该文件是自动打开的

  ```c++
  ofstream myFile("filename");
  ```

- 方法2：可以在调用默认构造函数之后使用open成员函数打开文件

  ```c++
  ofstream myFile; //声明一个静态文件输出流对象
  myFile.open("filename");   //打开文件，使流对象与文件建立联系
  ```

- 在构造对象或用open打开文件时可以指定模式

  ```c++
  ofstream myFile("filename", ios_base::out | ios_base::binary);//表示打开一个用于输出的文件，并且是二进制文件。注意因为ofstream类对象本身就是用于输出的，所以ios_base::out不写也可以，它是默认的
  
  或者
      
  ofstream myFile;
  myFile.open("filename", ios_base::out | ios_base::binary);   
  ```

**文件输出流成员函数的三种类型**

- 与操纵符等价的成员函数。
- 执行非格式化写操作的成员函数。
- 其它修改流状态且不同于操纵符或插入运算符的成员函数。

**文件输出流成员函数**

- open函数
  - 把流与一个特定的磁盘文件关联起来。("filename")
  - 需要指定打开模式。(ios_base::out | ios_base::binary)
- put函数
  - 把一个字符写到输出流中。
- write函数
  - 把内存中的一块内容写到一个文件输出流中（实现低级的二进制写操作）
- seekp和tellp函数
  - 操作文件流的内部指针
  - seekp可以移动文件流内部的写指针。
  - tellp返回文件里当前写指针的位置
- close函数
  - 关闭与一个文件输出流关联的磁盘文件
- 错误处理函数
  - 在写到一个流时进行错误处理

#### 2.2向文本文件输出

其实显示器也是一种文本文件，以向标准输出设备显示器输出为例，展示如何控制输出文本文件时的格式

**插入运算符**

- 插入(<<)运算符
- 为所有**标准**C++数据类型预先设计的，用于传送字节到一个输出流对象。
- 将文本信息，插入到标准输出流里边去，顺着标准输出流就流到文本文件了

**操纵符（manipulator）**

- 插入运算符与操纵符一起工作
  - 控制输出格式。
- 很多操纵符都定义在
  - ios_base类中（如hex()）、头文件（如setprecision()）。
- 控制输出宽度
  - 在流中放入setw操纵符或调用width成员函数为每个项指定输出宽度。
  - **setw和width仅影响紧随其后的输出项，但其它流格式操纵符保持有效直到发生改变。**
- dec、oct和hex操纵符设置输入和输出的默认进制。

**例11-1 使用width控制输出宽度**

```c++
#include <iostream>
using namespace std;

int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    for(int i = 0; i < 4; i++) {
        cout.width(10);//每个输出项将占最少10个字符的位置，如果本身比10个多，那可以多输出。如果不够，那么要填充空格凑够10个
        cout << values[i] << endl;
    }
    return 0;
}
输出结果:
      1.23//不足10个字符的，用默认前导字符空格来填充
     35.36
     653.7
   4358.24
```

**例11-2使用setw操纵符指定宽度**

```c++
//11_2.cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    string names[] = { "Zoot", "Jimmy", "Al", "Stan" };
    for (int i = 0; i < 4; i++)
      cout << setw(6) << names[i] //names[i] 6个字符
     << setw(10) << values[i] << endl;//如果不加setw(10)，那么values[i]也不会享受之前的setw(6)效果，setw和width仅影响紧随其后的输出项
    return 0;
}
输出结果://用了setw或者width之后，输出将变得对齐很整齐
  Zoot      1.23
 Jimmy     35.36
    Al     653.7
  Stan   4358.24
```

**例11-3设置对齐方式**

```c++
//11_3.cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    string names[] = { "Zoot", "Jimmy", "Al", "Stan" };
    for (int i=0;i<4;i++)
      cout << setiosflags(ios_base::left)//左对齐。setiosflags一旦设置，一直生效，除非又修改
           << setw(6) << names[i]
           << resetiosflags(ios_base::left)//将左对齐的设置取消。那么又恢复默认的右对齐了
           << setw(10) << values[i] << endl;
    return 0;
}
输出结果:
Zoot        1.23
Jimmy      35.36
Al         653.7
Stan     4358.24
```

**setiosflags操纵符**

- 这个程序中，通过使用带参数的setiosflags操纵符来设置左对齐，setiosflags定义在头文件iomanip中。
- 参数ios*base::left是ios*base类的静态常量，因此引用时必须包括ios_base::前缀。
- 这里需要用resetiosflags操纵符关闭左对齐标志。**setiosflags不同于width和setw，它的影响是持久的，直到用resetiosflags重新恢复默认值时为止 。**
- setiosflags的参数是该流的格式标志值，可用按位或（|）运算符进行组合

**setiosflags的参数（流的格式标识）**

- ios_base::skipws 在输入中跳过空白 。
- ios_base::left 左对齐值，用填充字符填充右边。
- ios_base::right 右对齐值，用填充字符填充左边（默认对齐方式）。
- ios_base::internal 在规定的宽度内，指定前缀符号之后，数值之前，插入指定的填充字符。
- ios_base::dec 以十进制形式格式化数值（默认进制）。
- ios_base::oct 以八进制形式格式化数值 。
- ios_base::hex 以十六进制形式格式化数值。
- ios_base::showbase 插入前缀符号以表明整数的数制。
- ios_base::showpoint 对浮点数值显示小数点和尾部的0 。
- ios_base::uppercase 对于十六进制数值显示大写字母A到F，对于科学格式显示大写字母E 。
- ios_base::showpos 对于非负数显示正号（“+”）。
- ios_base::scientific 以科学格式显示浮点数值。
- ios_base::fixed 以定点格式显示浮点数值（没有指数部分） 。
- ios_base::unitbuf 在每次插入之后转储并清除缓冲区内容。

**精度**

- 浮点数输出精度的默认值是6，例如：3466.98。
- 要改变精度：**setprecision操纵符**（定义在头文件iomanip中）。
- 如果不指定fixed（定点形式）或scientific（科学记数法形式），那么精度值表示有效数字位数。
- 如果设置了ios*base::fixed或ios*base::scientific，那么精度值表示小数点之后的位数。

**例11-4控制输出精度——未指定fixed或scientific**

```c++
//11_4_1.cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    string names[] = { "Zoot", "Jimmy", "Al", "Stan" };
    for (int i=0;i<4;i++)
      cout << setiosflags(ios_base::left)
        << setw(6) << names[i]
        << resetiosflags(ios_base::left)//清除左对齐设置
        << setw(10) << setprecision(1) << values[i] << endl;//setprecision(1) 表示有效数字的位数为1位
    return 0;
}
输出结果：//因为未指定fixed或scientific，系统自动调整方式，有时这个有时那个
Zoot           1
Jimmy     4e+001
Al        7e+002
Stan      4e+003
```

**例11-4控制输出精度——指定fixed**

```c++
//11_4_2.cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;
int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    string names[] = { "Zoot", "Jimmy", "Al", "Stan" };
    cout << setiosflags(ios_base::fixed); //设置成只能使用定点输出，不能用科学记数法  
    for (int i=0;i<4;i++)
      cout << setiosflags(ios_base::left)
        << setw(6) << names[i]
        << resetiosflags(ios_base::left)//清除左对齐设置
        << setw(10) << setprecision(1) << values[i] << endl;//setprecision(1)表示小数点后几位
    return 0;
}
输出结果：
Zoot         1.2
Jimmy       35.4
Al         653.7
Stan      4358.2
```

**例11-4控制输出精度——指定scientific**

```c++
//11_4_3.cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;
int main() {
    double values[] = { 1.23, 35.36, 653.7, 4358.24 };
    string names[] = { "Zoot", "Jimmy", "Al", "Stan" };
  cout << setiosflags(ios_base::scientific);//设置成只能使用科学记数法，不能使用定点法
    for (int i=0;i<4;i++)
      cout << setiosflags(ios_base::left)
        << setw(6) << names[i]
        << resetiosflags(ios_base::left)//清除左对齐设置
        << setw(10) << setprecision(1) << values[i] << endl;//setprecision(1)也是表示小数点后几位
    return 0;
}
输出结果：
Zoot    1.2e+000
Jimmy   3.5e+001
Al      6.5e+002
Stan    4.4e+003
```

#### 2.3向二进制文件输出

二进制文件一般不是给人阅读的，一般是给其他程序读入处理

**二进制文件流**

- 方法1：使用ofstream构造函数中的模式参量指定二进制输出模式；
- 方法2：或者以通常方式构造一个流，然后使用setmode成员函数，在文件打开后改变模式；
- 通过二进制文件输出流对象完成输出。

**例11-5 向二进制文件输出**

c++基本类库不支持将类对象直接写入文件，但是支持结构体对象整体写入文件

```c++
//11_5.cpp
#include <fstream>
using namespace std;
struct Date { 
    int mon, day, year;  
};
int main() {
    Date dt = { 6, 10, 92 };
    ofstream file("date.dat", ios_base::binary);//将输出流对象file与文件“date.dat"绑定，并设置打开方式为二进制文件
    file.write(reinterpret_cast<char *>(&dt),sizeof(dt));//将结构体对象dt整个写到二进制文件中去
    file.close();
    return 0;
}
```

#### 2.4向字符串输出

向程序内存中的某个字符串输出。我们可以**把内存中的某个字符串作为输出流的目标**，**可以实现将其他数据类型转换为字符串的功能**

**字符串输出流（ ostringstream ）**

- 用于构造字符串
- 功能
  - 支持ofstream类的除open、close外的所有操作
  - str函数可以返回当前已构造的字符串
- 典型应用
  - 将数值转换为字符串

**例11-6用ostringstream将数值转换为字符串**

```c++
//11_6.cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

//函数模板toString可以将各种支持“<<“插入符的类型的对象转换为字符串。

template <class T>
inline string toString(const T &v) {//类型T必须得能进行插入运算
    
    ostringstream os;   //创建字符串输出流
    os << v;        //将变量v的值写入字符串流
    return os.str();    //返回输出流生成的字符串
}

int main() {
    string str1 = toString(5);
    cout << str1 << endl;
    string str2 = toString(1.2);
    cout << str2 << endl;
    return 0;
}
输出结果：
5
1.2
```

### 3.输入流

#### 3.1输入流概述

**重要的输入流类**

- istream类最适合用于顺序文本模式输入。cin是它的实例。
- ifstream类支持磁盘文件输入。
- istringstream，从字符串输入信息

**构造输入流对象**

- 方法1：如果在构造函数中指定一个文件名，在构造该对象时该文件便自动打开。 

  - ```c++
    ifstream myFile("filename");//构造的同时，自动打开了
    ```

- 方法2：在调用默认构造函数之后使用open函数来打开文件。

  - ```c++
     ifstream myFile; //建立一个文件流对象，还需要手动打开文件
     myFile.open("filename"); //打开文件"filename”
    ```

- 打开文件时可以指定模式

  - ```c++
     ifstream myFile("filename", ios*base::in | ios*base::binary);
    或者
     ifstream myFile; //建立一个文件流对象，还需要手动打开文件
     myFile.open("filename",ios*base::in | ios*base::binary); //打开文件"filename”  
    ```

**使用提取运算符从文本文件输入**

- 提取运算符(>>)对于所有标准C++数据类型都是预先设计好的。
- 是从一个输入流对象获取字节最容易的方法。
- ios类中的很多操纵符都可以应用于输入流。但是只有少数几个对输入流对象具有实际影响，其中最重要的是进制操纵符dec、oct和hex。//10 \8\16进制

**输入流类相关成员函数**

- open 把该流与一个特定磁盘文件相关联。
- get 功能与提取运算符（>>）很相像，
  - 主要的不同点是get函数在读入数据时包括空白字符。
  - 而提取运算符（>>)是以空白字符为分隔符的，遇到空白符就停止了
- getline 功能是从输入流中读取多个字符，并且允许指定输入终止字符，读取完成后，从读取的内容中删除终止字符。
  - 可以读取一行，并且可以定制终止字符
- read 从一个文件读字节到一个指定的内存区域，由长度参数确定要读的字节数。当遇到文件结束或者在文本模式文件中遇到文件结束标记字符时结束读取。(读二进制文件用)
- seekg 用来设置文件输入流中读取数据位置的指针。
- tellg 返回当前文件读指针的位置。
- close 关闭与一个文件输入流关联的磁盘文件。

#### 3.2输入流应用举例

**例11-7 get函数应用举例**

```c++
//11_7.cpp
#include <iostream>
using namespace std;
int main() {
    char ch;
    while ((ch = cin.get()) != EOF)//EOF是文件结束标记
        cout.put(ch);
    return 0;
}
```

**例11-8为输入流指定一个终止字符：**

```c++
//11_8.cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
    string line;
    cout << "Type a line terminated by 't' " << endl; 
    //getline可以读入一行包含空格的字符串，cin只能读入一个单词，不能有空格
    getline(cin, line, 't');//cin就说明从键盘提取； ‘t'是指分隔符，默认以换行为分隔符。
    cout << line << endl;
    return 0;
}
```

**例11-9 从文件读一个二进制记录到一个结构中**

```c++
//11_9.cpp
#include <iostream>
#include <fstream>
#include <cstring>
using namespace std;

struct SalaryInfo {
    unsigned id;
    double salary;
}; 
int main() {
    SalaryInfo employee1 = { 600001, 8000 };
    ofstream os("payroll", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(&employee1), sizeof(employee1));
    os.close();//将某个结构体对象写入到文件“payroll”中
    
    ifstream is("payroll", ios_base::in | ios_base::binary);
    if (is) {
        SalaryInfo employee2;
        is.read(reinterpret_cast<char *>(&employee2), sizeof(employee2));//从文件中读取数据，保存到employee2中
        cout << employee2.id << " " << employee2.salary << endl;
    } else {
        cout << "ERROR: Cannot open file 'payroll'." << endl;
    }
    is.close();
    return 0;
}
```

**例11-10用seekg函数设置位置指针**

```c++
//11_10.cpp, 头部分省略
int main() {
    int values[] = { 3, 7, 0, 5, 4 };
    ofstream os("integers", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(values), sizeof(values));
    os.close();//将5个整数一体写到文件“integers"去

    ifstream is("integers", ios_base::in | ios_base::binary);
    if (is) {
        is.seekg(3 * sizeof(int));//将指针指到第4个整数开始处
        int v;
        is.read(reinterpret_cast<char *>(&v), sizeof(int));//只读一个Int大小
        cout << "The 4th integer in the file 'integers' is " << v << endl;
    } else {
        cout << "ERROR: Cannot open file 'integers'." << endl;
    }
    return 0;
}
输出：
    The 4th integer in the file 'integers' is 5
```

**例11-11 读一个文件并显示出其中0元素的位置**

```c++
//11_11.cpp, 头部分省略
int main() {
    ifstream file("integers", ios_base::in | ios_base::binary);
    if (file) {
        while (file) {//读到文件尾file为0
            streampos here = file.tellg();
            int v;
            file.read(reinterpret_cast<char *>(&v), sizeof(int));
            if (file && v == 0) 
            cout << "Position " << here << " is 0" << endl;
        }
    } else {
        cout << "ERROR: Cannot open file 'integers'." << endl;
    }
    file.close();
    return 0;
}
```

#### 3.3从字符串输入

将字符串作为文本输入流的源，可以将字符串转换为其他数据类型

**字符串输入流（ istringstream）**

- 用于从字符串读取数据
- 在构造函数中设置要读取的字符串
- 功能
  - 支持ifstream类的除open、close外的所有操作
- 典型应用
  - 将字符串转换为数值

**例11-12用istringstream将字符串转换为数值**

```c++
//11_12.cpp, 头部分省略
template <class T>
inline T fromString(const string &str) {
    istringstream is(str);  //创建字符串输入流
    T v;
    is >> v;    //从字符串输入流中读取变量v
    return v;   //返回变量v
}

int main() {
    int v1 = fromString<int>("5");
    cout << v1 << endl;
    double v2 = fromString<double>("1.2");
    cout << v2 << endl;
    return 0;
}
输出结果：
5
1.2
```



### 4.输入/输出流

一旦这个流打开了，就可以用它输入数据，又可以向同一个文件输出数据

#### 两个重要的输入/输出流

- 一个iostream对象可以是数据的源或目的。
- 两个重要的I/O流类都是从iostream派生的，它们是fstream和stringstream。这些类继承了前面描述的istream和ostream类的功能。

#### fstream类

- fstream类支持磁盘文件输入和输出。
- 如果需要在同一个程序中从一个特定磁盘文件读,并再写到该磁盘文件，可以构造一个fstream对象。
- 一个fstream对象是有两个逻辑子流的单个流，两个子流一个用于输入，另一个用于输出。

#### stringstream类

- stringstream类支持面向字符串的输入和输出
- 可以用于对同一个字符串的内容交替读写，同样是由两个逻辑子流构成。

## 第12章 异常处理

### 1.异常处理的思想与程序实现

#### 异常处理的基本思想



![NA5ePO.png](https://s1.ax1x.com/2020/06/17/NA5ePO.png)

#### 异常处理的语法

- 抛掷异常的程序段（抛出给本函数的调用者）

  - ```c++
    ......
    throw 表达式;
    ......
    ```

- 捕获并处理异常的程序段

  - 有可能发生异常的代码，放在try块里。如果发生了异常，将暂停局部代码的执行，跳转到catch块捕获异常，catch里可以决定是就地处理还是向调用者抛出异常。

  - ```c++
    ......
    try
        复合语句——————保护段
    catch(异常声明)
        复合语句——————异常处理程序
    catch(异常声明)
        复合语句
        ...
    ```

- 若有异常则通过throw创建一个异常对象并抛掷

- **将可能抛出异常的程序段嵌在try块之中**。通过正常的顺序执行到达try语句，然后执行try块内的保护段

- **如果在保护段执行期间没有引起异常，那么跟在try块之后的catch子句就不执行**。程序从try块后的最后一个catch子句后面的语句继续执行

- catch子句按其在try块后出现的顺序被检查。匹配的catch子句将捕获并处理异常（或继续抛掷异常）。

- 如果匹配的处理器未找到（抛出的异常类型与所有的catch都不匹配），则库函数terminate将被自动调用，其默认功能是调用abort终止程序。

#### 处理除零异常

```c++
//12_1.cpp
#include <iostream>
using namespace std;
int divide(int x, int y) {
    if (y == 0)
        //遇到异常，没有就地处理，抛出给它的调用者
        throw x;//直接就把被除数作为异常对象抛出了。其实抛什么类型的对象都可以，只要抛出和接受约定好就行
    return x / y;
}
int main() {
    try {
        cout << "5 / 2 = " << divide(5, 2) << endl;
        cout << "8 / 0 = " << divide(8, 0) << endl;//出现异常之后，其后的try代码都不执行了
        cout << "7 / 1 = " << divide(7, 1) << endl;
    } catch (int e) {//捕获也是Int类型，跟throw抛出类型是呼应的
        cout << e << " is divided by zero!" << endl;
    }
    cout << "That is ok." << endl;
    return 0;
}
输出：
5 / 2 = 2
8 / 0 = 8 is divided by zero!
That is ok.
```

#### 异常接口声明

如果一个函数知道自己可能抛出哪些类型的异常，最好就在编写函数的时候声明一下，我这个函数是预期可能抛出什么什么类型异常的。

- 一个函数**显式声明可能抛出的异常，有利于函数的调用者为异常处理做好准备**

- 可以在函数的声明中列出这个函数可能抛掷的所有异常类型。

  - ```c++
    例如：void fun() throw(A，B，C，D);//fun函数运行时可能抛出4种异常
    ```

- 不抛掷任何类型异常的函数声明如下：

  - ```c++
    void fun() throw();//表示fun函数一定不会抛出任何异常。//C++11之前
    void fun() noexcept;//表示fun函数一定不会抛出任何异常。//c++11之后
    ```

- **若无异常接口声明，则此函数可以抛掷任何类型的异常。**

### 2.异常处理中的构造与析构

如果在**try块中构造了一些对象**，**还没来得及析构就触发异常**了，后边的析构部分没有来得及执行 。那么会不会出现资源泄露的情况呢？答案，不会，**在catch块中会自动析构**。

#### 自动的析构

- 找到一个匹配的catch异常处理后
  - 初始化异常参数。（类似于调用子函数，实参初始化形参）
  - **将从对应的try块开始到异常被抛掷处之间构造（且尚未析构）的所有自动对象进行析构。**
  - 从最后一个catch处理之后开始恢复执行。

#### 例12-2 带析构语义的类的C++异常处理

```c++
//12_2.cpp
#include <iostream>
#include <string>
using namespace std;
class MyException {
public:
    MyException(const string &message) : message(message) {}
    ~MyException() {}
    const string &getMessage() const { return message; }
private:
    string message;
};

class Demo {
public:
    Demo() { cout << "Constructor of Demo" << endl; }
    ~Demo() { cout << "Destructor of Demo" << endl; }
};
void func() throw (MyException) {//显示声明，有可能抛出MyException类型的异常
    Demo d;
    cout << "Throw MyException in func()" << endl;
    throw MyException("exception thrown by func()");//构造异常对象并抛出，这里是100%抛出，一般抛出不抛出是要自己If来写的
}

int main() {
    cout << "In main function" << endl;
    try {
        func();
    } catch (MyException& e) {
        cout << "Caught an exception: " << e.getMessage() << endl;
    } 
    cout << "Resume the execution of main()" << endl;
    return 0;
}
运行结果：
In main function
Constructor of Demo
Throw MyException in func()
Destructor of Demo//func函数中Demo对象d还没来得及析构，在catch中自动析构了
Caught an exception: exception thrown by func()
Resume the execution of main()
```



### 3.标准程序库异常处理

之前不是说抛出异常可以是任何类型吗，比如一个int。C++自己封装了一些常用的类型，方便使用，所以一般抛出异常的类型都用标准异常类

#### 标准异常类的继承关系

![NGTXyF.png](https://s1.ax1x.com/2020/06/22/NGTXyF.png)

exception是基类，后边都是各级派生类

#### C++标准库各种异常类所代表的异常

![NGTxeJ.png](https://s1.ax1x.com/2020/06/22/NGTxeJ.png)

#### 标准异常类的基础

- exception：标准程序库异常类的公共基类

- logic_error表示可以在程序中被预先检测到的异常

- - 如果小心地编写程序，这类异常能够避免

- runtime_error表示难以被预先检测的异常

#### 例12-3 三角形面积计算

编写一个计算三角形面积的函数，函数的参数为三角形三边边长a、b、c，可以用[Heron公式](https://zh.wikipedia.org/zh/海伦公式)计算：

```c++
//12_3.cpp
#include <iostream>
#include <cmath>
#include <stdexcept>
using namespace std;
//给出三角形三边长，计算三角形面积
double area(double a, double b, double c)  throw (invalid_argument)//显示声明可能抛出的异常
{
   //判断三角形边长是否为正
    if (a <= 0 || b <= 0 || c <= 0)
        throw invalid_argument("the side length should be positive");
    //invalid_argument就是标准库里的异常类
    
   //判断三边长是否满足三角不等式
    if (a + b <= c || b + c <= a || c + a <= b)
        throw invalid_argument("the side length should fit the triangle inequation");
   //由Heron公式计算三角形面积
    double s = (a + b + c) / 2; 
    return sqrt(s * (s - a) * (s - b) * (s - c));
}
int main() {
    double a, b, c; //三角形三边长
    cout << "Please input the side lengths of a triangle: ";
    cin >> a >> b >> c;
    try {
        double s = area(a, b, c);   //尝试计算三角形面积
        cout << "Area: " << s << endl;
    } catch (exception &e) {//exception是基类，invalid_argument是派生类
        cout << "Error: " << e.what() << endl;
    }
    return 0;
}
•   运行结果1：
Please input the side lengths of a triangle: 3 4 5
Area: 6
•   运行结果2：
Please input the side lengths of a triangle: 0 5 5
Error: the side length should be positive
•   运行结果2：
Please input the side lengths of a triangle: 1 2 4
Error: the side length should fit the triangle inequation

```

