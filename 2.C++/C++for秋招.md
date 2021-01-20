[TOC]



# C++

## 一、基本语法

### 1.存储持续性、作用域和链接性

#### 1.存储持续性：

- 自动存储持续性：
  - 在函数中声明的函数参数和变量的存储持续性为自动，作用域为局部，没有链接性。
  - 寿命：它们在程序开始执行其所属的函数或代码块时被创建，在执行完函数或代码块时，它们使用的内存被释放
- 静态存储持续性：（可以再细分成3种，见小标题4）
  - 寿命：这种变量的寿命都持续到程序结束。
  - 在**函数定义外定义的变量**和**使用关键字static定义的变量**的存储持续性都是静态。
  - C++为静态存储持续性变量提供了3种链接性：外部链接性（可在其他文件中访问）、内部链接性（只能在当前文件中访问）和无链接性（只能在当前函数或者代码块中访问）。
  - 如果没有显式地初始化静态变量，编译器将把它设置为0。默认情况下，静态数组和结构将每个元素或成员的所有位都设置为0。
- 动态存储持续性：
- 线程存储持续性：

#### 2.作用域

作用域描述了名称在文件（翻译单元）的多大范围内可见。

例如，函数中定义的变量可在该函数中使用，但不能在其他函数中使用；而在文件中的函数定义之前定义的变量则可在所有函数中使用。

- **函数原型作用域：**函数原型作用域中使用的名称只在包含参数列表的括号内可用
- **局部作用域：**作用域为局部的变量只能在定义它的代码块中可用。代码块是由花括号括起来的一系列语句
- **类作用域：**再类中声明的成员的作用域为整个类
- **作用域为全局**（也叫文件作用域）的变量在定义位置到文件结尾之间都可用
- 在**命名空间**中声明的变量的作用域为整个名称空间（全局作用域是名称空间作用域的特例）

C++函数的作用域可以是整个类或者整个名称空间（包括全局）的，但不能是局部的（也就是不能在代码块内定义函数，否则将无法被其他函数调用）。

#### 3.链接性

链接性表述了名称如何在不同单元间共享。

- 链接性为**外部**的名称可在文件间共享
- 链接性为**内部**的名称只能由一个文件中的函数共享。

自动变量的名称没有链接性，因为它们不能共享。

#### 4.静态存储持续性（即全局变量、全局static变量、局部static变量。这3种变量的寿命都持续到程序结束）

- 要想创建链接性为外部（可在其他文件中访问）的静态持续变量，必须在代码块的外面声明它
  - 全局变量，无static修饰的
- 要创建链接性为内部（只能在当前文件中访问）的静态持续变量，必须在代码块外面声明它，并使用static限定符。
  - 全局静态变量
- 要创建没有链接性（只能在当前函数或者代码块中访问）的静态持续变量，必须在代码块内声明它，并使用static限定符
  - 局部静态变量

```c++
...
int global=1000;//全局变量，静态持续变量，外部链接性。可以在本文件，也可以在其他文件中使用。
static int one_file=50;//全局静态变量，静态持续变量，内部链接性。只可以在本文件使用，从声明位置到文件结尾
int main(){
    ...
}

void funct1(int n)
{
    static int count=0;//局部静态变量，静态持续变量，无链接性。只能在funct1函数中使用，但它具备静态持续性。即生命周期持续到程序结束。
    int llama=0;
}
```

#### 5.extern的跨文件用法

extern:该变量/函数是从别的文件来的，或者该变量/函数可以被别的文件使用。

`extern int yyd;//这是声明`

`extern int yyd=1;//这是定义`

1.两个文件都加上extern声明，不过只能有一个定义。正确

```c++
//a.cpp
#include <iostream>
using namespace std;
extern int yyd = 1;
void test2();
int main() {
    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
extern int yyd;
void test2() {
    cout << yyd<<endl;
}
```

2.第一个文件里没有extern，第二个文件extern声明。正确

```c++
//a.cpp
#include <iostream>
using namespace std;
int yyd = 1;
void test2();
int main() {
    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
extern int yyd;
void test2() {
    cout << yyd<<endl;
}
```

3.各种错误实例

```c++
//a.cpp
int yyd;
//b.cpp
int yyd;//错误!


//a.cpp
extern int yyd=0;
//b.cpp
int yyd;//错误！
```

**总结：**

- 只能有一个份定义。定义时加不加extern都行
- 定义
  - `extern int  yyd=1;`
  - 或者
  - `int yyd=1;`
- 声明
  - `extern int yyd;`

#### 6.使用C++的作用域解析运算符(::)来访问被隐藏的外部变量

```c++
//a.cpp
#include <iostream>
using namespace std;
int yyd=1;
void test2();
int main() {

    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
extern int yyd;
void test2() {
    int yyd = 2;
    cout << "局部变量："<<yyd<<endl;//局部变量
    cout << "全局变量:" << ::yyd << endl;//该运算符表示使用变量的全局版本
}
//输出
 "局部变量：" 2
 "全局变量:"  1
```

#### 7.全局静态变量有什么用？

- 如果文件a定义了一个静态外部变量（全局静态变量），其名称与另一个文件b中声明的常规外部变量（普通全局变量）相同，则在该文件a中，全局静态变量将隐藏外部的全局变量.

- 可使用链接性为内部的静态变量（全局静态变量）**在同一个文件**中的多个函数之间共享数据。

- 如果将作用域为整个文件的变量变为静态的，就不必担心其名称与其他文件中的作用域为整个文件的变量发生冲突。

- 其实总结就是用static将变量的链接性变为了内部的，使之只能在一个文件中使用。

- 用static也可以将函数的链接性变为了内部的，使之只能在一个文件中使用。要知道**函数，默认都是自带extern的。使用static可以将函数只能在本文件使用**。

  - ```c++
    //a.cpp
    static int parivate(double x);
    ...
    static int private(double x);
    ```
```c++
- extern和static肯定是不能同时出现的。因为extern是为了说明这是个跨文件的变量。而static则是限定，这是个只支持在本文件使用的变量。二者是矛盾和对立的。

​```c++
//a.cpp
int yyd=20;//普通的全局变量
//b.cpp
static int yyd=10;//你要是写int yyd=10;会报错的
//你要是写extern int yyd;//那就是引入了a文件的变量
//而写static int yyd=10，则是不会报错，隐藏了a文件的普通全局变量yyd
```

```c++
//a.cpp
#include <iostream>
using namespace std;
int yyd=1;
void test2();
int main() {

    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
static int yyd;
void test2() {
    int yyd = 2;
    cout << "局部变量："<<yyd<<endl;
    cout << "全局变量:" << ::yyd << endl;
}
//输出
"局部变量：" 2
"全局变量:"  0
```

```c++
//a.cpp
int tom=3;
int dick=30;
static int harry=300;

//b.cpp
extern int tom;
static int dick=10;
int harry=200;

//两个文件使用了同一个tom变量，但使用了不同的dick和harry变量。两个tom变量的地址相同，而两个dick和harry变量的地址不同。
```

#### 8.默认const全局变量的链接性是内部的，如同自带static。如果你希望是外部的，需要定义里自己加上extern

一般的全局变量的链接性为外部的，但是const全局变量的链接性是内部的。也就是说，在C++看来，全局const定义，就如同自带static一样

```c++
int a=1;//全局变量，链接性是外部的
static int b=2;//全局静态变量，链接性是内部的
const int c=3;//全局const变量，链接性是内部的
extern const int d=4;//全局const变量，链接性是外部的
int mian()
{
    ...
}
```

#### 8.名称空间

- using声明将名称std添加到局部声明区域中。
- 在main里用了using，test2里没有，所以test2里找不到cout
- 在函数外面使用using声明时，可以把名称std添加到全局名称空间中，那么整个文件都可用了

错误

```c++
#include <iostream>
int yyd = 1;
void test2();
int main() {
    using namespace std;//
    test2();
    return 0;
}

void test2() {
    int yyd = 2;
    cout << "局部变量：" << yyd << endl;//出错，未定义的标识符cout,endl
    cout << "全局变量:" << ::yyd << endl;
}
```

正确

```c++
#include <iostream>
int yyd = 1;
void test2();
 using namespace std;//
int main() {

    test2();
    return 0;
}

void test2() {
    int yyd = 2;
    cout << "局部变量：" << yyd << endl;//出错，未定义的标识符cout,endl
    cout << "全局变量:" << ::yyd << endl;
}
```

#### 9.C++在哪里查找函数

如果该文件中的函数原型指出该函数是静态的，则编译器将只在该文件中查找函数定义；否则， 编译器（包括链接程序）将在所有的程序文件中查找。如果找到两个定义，编译器将发出错误消息，因为每个外部函数只能有一个定义。如果在程序文件中没有找到，编译器将在库中搜索。这意味着如果定义了一个与库名称同名的名称，编译器将使用程序员定义的版本而不是库函数（然而C++保留了标准库函数的名称，即程序员不应该使用它们）。

### 2.说一下static关键字的作用

- 对于全局变量或者一个普通函数，static是个紧箍咒，他们从本来可以跨文件使用，被限制在了只能在本文件使用。不过同时也提供了一点便利性，不必担心名称与其他文件中的作用域为整个文件的变量、函数发生冲突（或者说叫做文件隔离）
- 对于局部变量，static为其提供了长久的寿命，可以每次进入其作用域时取出旧值。
- 对于类内静态数据成员，它提供了所有该类型对象共用一个数据
- 对于类内静态函数成员，它不需要对象名，直接通过类名就可以调用类内静态函数成员

#### 1.全局静态变量

在全局变量前加上关键字static，全局变量就定义成一个全局静态变量。

**内存中的位置**：**静态存储区**，在整个程序运行期间一直存在。（即**初始化数据段+未初始化数据段（BSS段）**）

**初始化**：未经初始化的全局静态变量会被自动初始化为0（自动对象（也就是非静态变量）的值是任意的，除非他被显式初始化）；

**作用域**：全局静态变量（全局变量+static修饰）在声明他的文件之外是不可见的，准确地说是**从定义之处开始，到本文件结尾。**

**附注：**

- 全局变量和全局静态变量不一样。
- 全局变量和全局静态变量都具有全局作用域
- **静态全局变量(全局变量被static修饰)与全局变量(全局变量未被static修饰)的区别**在于如果程序包含多个文件的话，静态全局变量作用于定义它的文件里，**不能作用到其它文件里，即被static关键字修饰过的全局变量只能在当前文件中访问。static的全局变量作用域只在本文件中，所以extern和static不能同时修饰一个变量**。这样即使两个不同的源文件都定义了相同名字的静态全局变量，它们也是不同的变量。
- 而全局变量（没有被static修饰的），只需在一个源文件中定义，就可以作用于所有的源文件。当然，其他不包含全局变量定义的源文件需要用extern关键字再次声明这个全局变量。
- 一般定义static全局变量时，都把它放在源文件中而不是头文件，这样就不会给其他模块造成不必要的信息污染。
  - 因为如果你在头文件中定义static全局变量，你可能想当然的认为，两个包含该头文件的源文件中的该变量是共通的，然而实际上是独立的两个变量。这将造成一些不必要的误会。

#### 2.局部静态变量

在局部变量之前加上关键字static，局部变量就成为一个局部静态变量。

**内存中的位置**：静态存储区（即**初始化数据段+未初始化数据段（BSS段）**）

**初始化**：未经初始化的局部静态变量会被自动初始化为0（与之相对的自动对象的值是任意的，除非他被显式初始化）；

**作用域**：作用域仍为局部作用域，当定义它的函数或者语句块结束的时候，作用域结束。但是当局部静态变量离开作用域后，并没有销毁，而是仍然驻留在内存当中，只不过我们不能再对它进行访问，直到该函数再次被调用，并且值不变；

#### 3.静态函数

在函数返回类型前加static，函数就定义为静态函数。函数的定义和声明在默认情况下都是extern的，但**静态函数只是在声明他的文件当中可见，不能被其他文件所用**。

函数的实现使用static修饰，那么这个函数只可在本cpp内使用，不会同其他cpp中的同名函数引起冲突；

warning：不要在头文件中声明static的全局函数，不要在cpp内定义多个非static的全局函数，如果你要在多个cpp中复用该函数，就把它的声明提到头文件里去，否则cpp内部声明需加上static修饰；

#### 4.类的静态成员

- 在类中，静态成员可以实现多个对象之间的数据共享，并且使用静态数据成员还不会破坏隐藏的原则，即保证了安全性。因此，静态成员是类的所有对象中共享的成员，而不是某个对象的成员。对多个对象来说，静态数据成员只存储一处，供所有对象共用。
- **类的静态数据成员必须在类外定义和初始化**，用(::)来指明所属的类。

#### 5.类的静态函数

- 静态成员函数和静态数据成员一样，它们都属于类的静态成员，它们都不是对象成员。因此，对静态成员的引用不需要用对象名。
- 静态成员函数主要用于处理该类的静态数据成员。在静态成员函数的实现中不能直接引用类中说明的非静态成员，可以引用类中说明的静态成员（这点非常重要）。
- 如果静态成员函数中要引用非静态成员时，可通过参数传递的方式得到对象名，然后再通过对象名来引用。从中可看出，调用静态成员函数使用如下格式：<类名>::<静态成员函数名>(<参数表>);
- 与普通的成员函数相比，静态成员函数由于不是与任何的对象相联系，因此它不具有this指针。从这个意义上来说，它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，只能调用其他的静态成员函数。

### 3.说一下C++和C的区别

#### 1.按照Effective C++的理解

C++是一个语言联邦，包括：C、面向对象的C++、模板C++、STL。

- C语言：
  - 区块（blocks）、语句、预处理器、内置数据类型、数组、指针等通通来自C。C里没有模板、没有异常、没有重载...
- Object-Oriented C++：
  - 这部分就是C with Classes所诉求的：类classes（包括构造函数和析构函数），封装，继承，多态，虚函数（动态绑定）等等。
  - 这就是面向对象的思想在C++上的体现。
- Template C++：
  - C++的泛型编程部分。
  - 泛型编程，编程范型，模板元编程（TMP）（模板元编程可以在编译的过程中计算出结果：例如计算一个数的阶乘，运行      时当作常量使用）
  - 通用编程意味着您不是在编写按原样编译的源代码，而是编写源代码的“模板”，这些模板在编译过程中会转换为源代码。
  - c不支持泛型。c++的范型只是让编译器自动生成代码，编译器自动生成和手写的只是人力成本上的差异，因为C语言不支持泛型，所以需要多费点体力，或者写个自动生成代码的工具。
- STL：
  - STL是一个模板程序库。包含 容器，迭代器，算法，函数对象等

#### 2.设计思想上：

C++是面向对象的语言，而C是面向过程的结构化编程语言

#### 3.详细的：

跟上边4个基本类似，多详细添加了3个：类型检查机制、异常处理、重载的机制

- **增强了类型检查机制**
  - C/C++ 是静态数据类型语言，类型检查发生在编译时，因此编译器知道程序中每一个变量对应的数据类型。C++ 的类型检查相对更严格一些。
  - 传统上 C 使用 void* 指针指向不同对象，使用时强制转换回原始类型或兼容类型。这样做的缺陷是绕过了编译器的类型检查，如果错误转换了类型并使用，会造成程序崩溃等严重问题。
  - C++ 通过使用基类指针或引用来代替 void* 的使用，避免了这个问题（其实也是体现了类继承的多态性）。
- 增加了面向对象的机制
  - 类classes（包括构造函数和析构函数），封装，继承，多态，虚函数（动态绑定）等等。
- 增加了泛型编程的机制（template）
  - 所谓泛型编程，简而言之就是不同的类型采用相同的方式来操作。在 C++ 的使用过程中，直接 template 用的不多，但是用 template 写的库是不可能不用的。
  - 意味着您不是在编写按原样编译的源代码，而是编写源代码的“模板”，这些模板在编译过程中会转换为源代码。
- **增加了异常处理**
  - **C 语言**不提供对错误处理的直接支持，但它以**返回值的形式**允许程序员访问底层数据。
  - **C++ 提供了一系列标准的异常**，定义在 <exception> 中，我们可以在程序中使用这些标准的异常。
- **增加了重载的机制**
  - 函数重载、运算符重载
- 增加了标准模板库（STL）

#### 4.具体版本

1. C是面向过程的语言，而C++是面向对象的语言
2. C和C++动态管理内存的方法不一样，C是使用malloc、free函数，而C++不仅有malloc/free，还有new/delete关键字
3. C++的类是C中没有的，C中的struct可以在C++中等同类来使用，struct和类的差别是，struct的成员默认访问修饰符是public，而类默认是private。
4. C++支持重载，而C不支持重载，C++支持重载在于C++名字的修饰符与C不同，例如在C++中函数 int f(int) 经过名字修饰之后变为`_f_int`,而C是`_f`，所以C++才会支持不同的参数调用不同的函数。
5. C++中有引用，而C没有
6. C++全局变量的默认连接属性是外链接，而C是内链接。
7. C中用const修饰的变量不可以用在定义数组时的大小，但是C++用const修饰的变量可以。
8. C++有很多特有的输入输出流。
9. C++具有封装、继承和多态三种特性
10. C++相比C，增加多许多类型安全的功能，比如强制类型转换、
11. C++支持范式编程，比如模板类、函数模板等

### 4.说一说c++中四种cast转换

C++中四种类型转换是：static_cast, dynamic_cast, const_cast, reinterpret_cast

#### 1、const_cast，移除变量的常量性（将底层const指针变为非const指针）

- 1.常量指针 被强转为 非常量指针，且仍然指向原来的对象；
- 2.常量引用 被强转为 非常量引用，且仍然指向原来的对象；
- 3.常量对象 被强转为 非常量对象。

用于将const变量转为非const,移除变量的常量性

const_cast不是万能的，它可以修改指向一个值的指针，但修改const值的结果是不确定的。

```c++
high bar;
const High* pbar=&bar;
High* pb=const_cast<High*> (pbar);//正确
const Low* pl=const_cat<const Low*> (pbar);//不正确，只能够修改const，不能变成其他类型（从原来的const high*变成const low*）
```



#### 2、static_cast，静态转型

**相当于传统的C语言里的强制转换**

- （1）主要用于内置数据类型之间的相互转换；
  - double转int，float转long等各种数值转换
- （2）用于自定义类时，静态转换会判断转换类型之间的关系，如果转换类型之间没有任何关系，则编译器会报错，不可转换；
  - **多态向上转化。**从派生类指针转成基类指针（dynamic_cast也可以）
  - **多态向下转化。**从基类指针转成派生类指针（dynamic_cast有时行，行的情况见3.（5）。用dynamic_cast将更安全）。
- （3）把void类型指针转为目标类型指针（不安全）。
- （4）把任何类型的表达式转换成void类型
- （5）还有各种隐式转换，比如非const转const等。(const转非const是const_cast)

```c++
//static_cast.cpp
//内置类型的转换
double dValue = 12.12;
float fValue = 3.14; // VS2013 warning C4305: “初始化”从“double”到“float”截断
int nDValue = static_cast<int>(dValue); // 12
int nFValue = static_cast<int>(fValue); // 3
//自定义类的转换
class A{};
class B : public A{};
class C{};
void main(){
    A *pA = new A;
    B *pB = static_cast<B*>(pA); // 编译不会报错, B类继承于A类。这是从基类指针转换成派生类指针
    pB = new B;
    pA = static_cast<A*>(pB); // 编译不会报错, B类继承于A类。这是从派生类指针转换成基类指针
    C *pC = static_cast<C*>(pA); // 编译报错, C类与A类没有任何关系。error C2440: “static_cast”: 无法从“A *”转换为“C *”
}
```

#### 3、dynamic_cast，向上转换 或者安全地向下转化（下行可能不安全，上行是百分百安全的）

- （1）**dynamic_cast是运行时处理的，运行时要进行类型检查，而其他三种都是编译时完成的；**

  - 它通过判断在执行到该语句的时候变量的运行时类型和要转换的类型是否相同来判断是否能够进行向下转换。
  - 成功则指针有值。失败则是nullptrs

- （2）不能用于内置基本数据类型间的强制转换；(但是static_cast可以）

- （3）使用dynamic_cast进行转换时，**基类中一定要有虚函数，否则编译不通过；**

- （4）dynamic_cast转换若成功，返回的是指向类的指针或引用；若失败则会返回NULL；

- （5）在类的转换时，在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的。在进行下行转换时，**dynamic_cast具有类型检查的功能，比static_cast更安全**。向下转换的成败取决于将要转换的类型，即要强制转换的指针所指向的对象实际类型与将要转换后的类型一定要相同，否则转换失败。

  - **向下转换成功本质上是：本来使用了一个 ""基类指针"" c 指向派生类对象，然后将这个"指向 派生类对象 的基类指针"c 强制转换成 “派生类指针”d**

  - ```c++
    //举例
    class base {
    public:
    	virtual void show() {//必须有个虚函数，否则dynamic_cast那一行无法编译通过
    		cout << "base" << endl;
    	}
    };
    class  derived: public base {
    public:
    	virtual void show() {
    		cout << "derived" << endl;
    	}
    };
    
    int main() {
    	base* a = new base;
    	derived* b = new derived;
    	base* c = new derived;//要强制转换的指针所指向的对象实际类型与将要转换后的类型一定要相同。比如原来这个base指针就是指向derived对象的。
    	c->show();
    	derived* d = dynamic_cast<derived*> (c);//原来这个base指针就是指向derived对象的。那么我把它变成了derived指针指向原来的derived对象，完全合理
    	if (d == nullptr)
    		cout << "nullptr" << endl;
    	else
    		d->show();
    	return 0;
    }
    //输出
    derived
    derived
    ```

  - ```c++
    //举例
    class base {
    public:
    	virtual void show() {
    		cout << "base" << endl;
    	}
    };
    class  derived: public base {
    public:
    	virtual void show() {
    		cout << "derived" << endl;
    	}
    };
    
    int main() {
    	base* a = new base;
    	derived* b = new derived;
    	base* c = new derived;//
    	c->show();
    	derived* d = dynamic_cast<derived*> (a);//base指针a本来指向的就是base对象，现在想用一个derived指针去指向base对象，完全不合理。所以转化失败，d是nullptr
    	if (d == nullptr)
    		cout << "nullptr" << endl;
    	else
    		d->show();
    	return 0;
    }
    //输出
    derived
    nullptr
    ```

- **dynamic_cast和static_cast的区别**

- 对于向上转化，dynamic_cast和static_cast都可以。对于向下转化二者的处理方式不同。

  - 向下成功时，二者无异。

  - dynamic_cast失败时，将变成nullptr，而static_cast可不会

  - ```c++
    class base {
    public:
    	virtual void show() {
    		cout << "base" << endl;
    	}
    };
    class  derived: public base {
    public:
    	virtual void show() {
    		cout << "derived" << endl;
    	}
    };
    
    int main(){
        base* a = new base;
        derived* b = new derived;
    	base* c = new derived;//
    	c->show();
    	derived* d = dynamic_cast<derived*> (c);//这种成功情况与derived* d = static_cast<derived*> (c) 无异
        
        derived* e = dynamic_cast<derived*> (a);//e将是nullptr
        derived* f = static_cast<derived*> (a);//f将有值，将发生不可思议的，一个派生类指针指向了一个基类对象的情况。这是不安全的
        f->show();//将输出base
    }
    ```

    

#### 4、**reinterpret_cast， 重新解释转型**:

- 几乎什么都可以转，比如将int转指针，可能会出问题，尽量少用；
- new_type a = reinterpret_cast <new_type> (value)
  - 将value的值转成new_type类型的值，a和value的值一模一样。比特位不变

更具体的：

- 从指针类型到一个足够大的整数类型
- 从整数类型或者枚举类型到指针类型
- 从一个指向函数的指针到另一个不同类型的指向函数的指针
- 从一个指向对象的指针到另一个不同类型的指向对象的指针
- 从一个指向类函数成员的指针到另一个指向不同类型的函数成员的指针
- 从一个指向类数据成员的指针到另一个指向不同类型的数据成员的指针

#### 5、为什么不使用C的强制转换？

C的强制转换表面上看起来功能强大什么都能转，但是转化不够明确，不能进行错误检查，容易出错。

#### 6.总结

- const_cast：对于未定义const版本的成员函数，我们通常需要使用const_cast来去除const引用对象的const，完成函数调用。另外一种使用方式，结合static_cast，可以在非const版本的成员函数内添加const，调用完const版本的成员函数后，再使用const_cast去除const限定。
- static_cast：完成基础数据类型；同一个继承体系中类型的转换；任意类型与空指针类型void* 之间的转换。
- dynamic_cast：这种其实也是不被推荐使用的，更多使用static_cast，dynamic本身只能用于存在虚函数的父子关系的强制类型转换，对于指针，转换失败则返回nullptr，对于引用，转换失败会抛出异常
- reinterpret_cast：可以用于任意类型的指针之间的转换，对转换的结果不做任何保证

### 5.请你说一下你理解的c++中的smart pointer四个智能指针:shared_ptr,unique_ptr,weak_ptr,auto_ptr

C++里面的四个智能指针: auto_ptr, shared_ptr, weak_ptr, unique_ptr 其中后三个是c++11支持，并且第一个已经被11弃用。

#### 1.C++11智能指针介绍

**智能指针主要用于管理在堆上分配的内存**，它将普通的指针封装为一个栈对象。当栈对象的生存周期结束后，会在析构函数中释放掉申请的内存，从而防止内存泄漏。C++ 11中最常用的智能指针类型为shared_ptr,它采用引用计数的方法，记录当前内存资源被多少个智能指针引用。该引用计数的内存在堆上分配。当新增一个时引用计数加1，当过期时引用计数减一。只有引用计数为0时，智能指针才会自动释放引用的内存资源。**对shared_ptr进行初始化时不能将一个普通指针直接赋值给智能指针，因为一个是指针，一个是类。可以通过make_shared函数或者通过构造函数传入普通指针**。并可以通过get函数获得普通指针。

#### 2.为什么要使用智能指针：

智能指针的作用是管理一个指针，因为存在以下这种情况：申请的空间在函数结束时忘记释放，造成内存泄漏。使用智能指针可以很大程度上的避免这个问题，因为智能指针就是一个类，当超出了类的作用域是，类会自动调用析构函数，析构函数会自动释放资源。所以**智能指针的作用原理就是在函数结束时自动释放内存空间，不需要手动释放内存空间。**

#### 3.auto_ptr

（C++98的方案，C++11已经抛弃）采用所有权模式。

```c++
auto_ptr<string> p1 (new string ("I reigned lonely as a cloud.")); 
auto_ptr<string> p2; 
p2 = p1; //赋值auto_ptr不会报错，编译会通过
cout<<*p2<<endl;//正常输出
cout<<*p1<<endl;//程序崩溃
```

此时不会报错，**p2剥夺了p1的所有权，但是当程序运行时访问p1将会报错。所以auto_ptr的缺点是：存在潜在的内存崩溃问题！**

#### 4.unique_ptr（替换auto_ptr，仅允许单个指针指向某个资源）

（替换auto_ptr）unique_ptr实现独占式拥有或严格拥有概念，**保证同一时间内只有一个智能指针可以指向该对象**。它对于避免资源泄露(例如“以new创建对象后因为发生异常而忘记调用delete”)特别有用。

采用所有权模式，还是上面那个例子

```c++
unique_ptr<string> p3 (new string ("auto"));   //#4
unique_ptr<string> p4；                       //#5
p4 = p3;//赋值unique_ptr此时会报错！！编译就通不过
```

编译器认为p4=p3非法，避免了p3不再指向有效数据的问题。**尝试复制p3时会编译期出错**，而auto_ptr能通过编译期从而在运行期埋下出错的隐患。因此，**unique_ptr比auto_ptr更安全**。

另外unique_ptr还有更聪明的地方：**当程序试图将一个 unique_ptr 赋值给另一个时**，如果源 unique_ptr 是个临时右值，编译器允许这么做；如果源 unique_ptr 将存在一段时间，编译器将禁止这么做，比如：

```c++
unique_ptr<string> pu1(new string ("hello world")); 
unique_ptr<string> pu2; 
pu2 = pu1;                                      // #1 不允许。

unique_ptr<string> pu3; 
pu3 = unique_ptr<string>(new string ("You"));   // #2 允许

pu1=unique_ptr<string>(new string ("me"));  //成立。
//赋值operator=调用之前被unique_ptr对象拥有的任何对象都将被删除（就像调用了unique_ptr的析构函数一样）。
//也就是说，hello world会先被安全地delete，赋值操作相当于先reset一下，然后赋值成新的"me"，不会造成资源泄露
```

其中#1留下悬挂的unique_ptr(pu1)，这可能导致危害。而#2不会留下悬挂的unique_ptr，因为它调用 unique_ptr 的构造函数，该构造函数创建的临时对象在其所有权让给 pu3 后就会被销毁。这种随情况而已的行为表明，unique_ptr 优于允许两种赋值的auto_ptr 。

**注：如果确实想执行类似与#1的操作（也就是保存旧值，再重新赋值。如果只是想重新赋值，那直接重新赋值就行，参看上边的new string("me")，当然不能是用另一个unique来赋值这个unique,参看上边的pu2=pu1，因为这样将有两个指针指向同一地方。）**，要安全的重用这种指针，可给它赋新值。C++有一个标准库函数std::move()，让你能够将一个unique_ptr赋给另一个。尽管转移所有权后 还是有可能出现原有指针调用（调用就崩溃）的情况。但是这个语法能强调你是在转移所有权，让你清晰的知道自己在做什么，从而不乱调用原有指针。

```c++
//保存旧值，再重新赋值
//如果只是想重新赋值，那直接重新赋值就行，参看上边的new string("me")，不会发生资源泄露
unique_ptr<string> ps1, ps2;
ps1 = demo("hello");
ps2 = move(ps1);//ps1的旧值赋给了ps2，ps1里边就是nullptr了
ps1 = demo("alexia");//ps1重新赋值
cout << *ps2 << *ps1 << endl;
```



另外：reset与release的区别，只有unique_ptr才有这两个。shared_ptr仅有reset

```c++
unique_ptr<string> pu1(new string ("hello world")); 
pu1.reset();//那么"hello world"资源将被释放，pu1指向空
string* temp=pu1.release();//剥离pu1与其内部指针的联系，并将内部指针返回回来，然后pu1指向空
```



#### 5.shared_ptr（共享指针，允许多个指针指向同一资源）

shared_ptr实现共享式拥有概念。多个智能指针可以指向相同对象，该对象和其相关资源会在“最后一个引用被销毁”时候释放。从名字share就可以看出了**资源可以被多个指针共享**，它使用计数机制来表明资源被几个指针共享。可以通过成员函数use_count()来查看资源的所有者个数。**除了可以通过new来构造，还可以通过传入auto_ptr, unique_ptr,weak_ptr来构造**。当我们调用release()时，当前指针会释放资源所有权，计数减一。当计数等于0时，资源会被释放。

shared_ptr 是为了解决 auto_ptr 在对象所有权上的局限性(auto_ptr 是独占的), 在使用引用计数的机制上提供了可以共享所有权的智能指针。

成员函数：

- use_count 返回引用计数的个数

- unique 返回是否是独占所有权( use_count 为 1)

- swap 交换两个 shared_ptr 对象(即交换所拥有的对象)

- reset 放弃内部对象的所有权或拥有对象的变更, 会引起原有对象的引用计数的减少

- get 返回内部对象(指针), 由于已经重载了()方法, 因此和直接使用对象是一样的.如

  - ```c++
    shared_ptr<int> sp(new int(1)); 
    ```

- sp 与 sp.get()是等价的。

**重点问题：**

- 多个智能指针可以指向相同对象，仅限于复制构造智能指针和赋值！

- 因为每个只能指针构造时，会new一个counter

- ```c++
  int main()
  {
  	string* s1 = new string("s1");
  
  	shared_ptr<string> ps1(s1);
  	shared_ptr<string> ps2(ps1);//复制构造，正确！
  	//shared_ptr<string> ps2;//或者赋值，正确！
  	//ps2 = ps1;
  	cout << ps1.use_count() << endl;	//2
  	cout << ps2.use_count() << endl;	//2
  	cout << ps1.unique() << endl;	//0
  	cout << ps2.unique() << endl;	//0
  	return 0;
  }
  ```

- ```c++
  //这样是各计数各的！并且会有大bug，s1被delete两次，引发异常
  int main()
  {
  	string* s1 = new string("s1");
  
  	shared_ptr<string> ps1(s1);//构造，
  	shared_ptr<string> ps2(s1);//构造，这样写是个大bug!
  
  	cout << ps1.use_count() << endl;	//1
  	cout << ps2.use_count() << endl;	//1
  	cout << ps1.unique() << endl;	//1
  	cout << ps2.unique() << endl;	//1
  	return 0;
  }
  ```

- ```c++
  //空指针的use_count是0，unique也是0
  int main()
  {
  	string* s1 = new string("s1");
  
  	shared_ptr<string> ps1(s1);
  	shared_ptr<string> ps2;//空指针
  
  	cout << ps1.use_count() << endl;	//1
  	cout << ps2.use_count() << endl;	//0
  	cout << ps1.unique() << endl;	//1
  	cout << ps2.unique() << endl;	//0
  	return 0;
  }
  ```

  

**share_ptr的简单例子：**

```c++
int main()
{
	string *s1 = new string("s1");

	shared_ptr<string> ps1(s1);
	shared_ptr<string> ps2;
	ps2 = ps1;

	cout << ps1.use_count()<<endl;	//2
	cout<<ps2.use_count()<<endl;	//2
	cout << ps1.unique()<<endl;	//0

	string *s3 = new string("s3");
	shared_ptr<string> ps3(s3);

	cout << (ps1.get()) << endl;	//033AEB48
	cout << ps3.get() << endl;	//033B2C50
	swap(ps1, ps3);	//交换所拥有的对象
	cout << (ps1.get())<<endl;	//033B2C50
	cout << ps3.get() << endl;	//033AEB48

	cout << ps1.use_count()<<endl;	//1
	cout << ps2.use_count() << endl;	//2
	ps2 = ps1;
	cout << ps1.use_count()<<endl;	//2
	cout << ps2.use_count() << endl;	//2
	ps1.reset();	//放弃ps1的拥有权，引用计数的减少
	cout << ps1.use_count()<<endl;	//0
	cout << ps2.use_count()<<endl;	//1
    cout << ps3.use_count() << endl;//1
}
```

**shared_ptr造成资源泄露的情况:**

虽然a和b智能指针析构了，但是没有删除对应的两个对象

```c++
#include <memory>
#include <iostream>

class TA
{
public:
	~TA(){
		std::cout << "de TA" << std::endl;
	}
	std::shared_ptr<TB> tb;
};

class TB
{
public:
	~TB(){
		std::cout << "de TB" << std::endl;
	}
	std::shared_ptr<TA> ta;
};

int main()
{
	{
		TA* yydA=new TA;
		TB* yydB=new TB;
		shared_ptr<TA> a = shared_ptr<TA>(yydA); // step1
		shared_ptr<TB> b = shared_ptr<TB>(yydB); // step2

		a->tb = b; // step3,等同于a->tb=shared_ptr<TB>(yydB);
		b->ta = a; // step4，等同于b->ta=shared_ptr<TA>(yydA);
        // step 5
        //那么，在离开作用域后，pA和pB的引用计数都是1，彼此都在等待对方释放，两个指针的引用计数永远不可能下降为0,资源永远不会释放，造成资源的泄露
//两个shared_ptr相互引用，死锁问题
	} 
    // step6
	return 0;
}
//在程序执行到 step1 的时候, 创建智能指针a，并赋值yydA，yydA指向的TA对象引用加1；
//在程序执行到 step2 的时候，创建智能指针b，并赋值yydB，yydB指向的TB对象引用加1；
//在程序执行到 step3 的时候，给a->tb赋值TB的对象，yydB指向的TB对象引用加1；
//在程序执行到 step4 的时候，给b->ta赋值TA的对象，yydA指向的TA对象引用加1；
//在程序执行到 step5 的时候，yydA指向的TA对象引用为2，yydB指向的TB对象引用为2；
//在程序执行到 step6 的时候，b智能指针超出作用域开始析构，yydB指向的TB对象引用减1；a智能指针超出作用域开始析构，yydA指向的TA对象引用减1；
//此时TA对象引用为1，TB的对象引用为1；
//虽然a和b智能指针析构了，记录这个引用次数的内存（在堆上）还在，它还记着至今都各还有一次引用,所以a，b指针析构时，没有删除对应的两个对象，堆上的TA TB对象泄露了，因为再也没有指针指向它们。
```

#### 6.weak_ptr（配合shared_ptr使用，它不会计数; 它不可以直接访问它管理的对象，转化成shared_ptr才行）

share_ptr虽然已经很好用了，但是有一点share_ptr智能指针还是有内存泄露的情况，当两个对象相互使用一个shared_ptr成员变量指向对方，会造成循环引用，使引用计数失效，从而导致内存泄漏。

weak_ptr 是一种不控制对象生命周期的智能指针, 它指向一个 shared_ptr 管理的对象。进行该对象的内存管理的是那个强引用的 shared_ptr。weak_ptr只是提供了对管理对象的一个访问手段。weak_ptr 设计的目的是为配合 shared_ptr 而引入的一种智能指针来协助 shared_ptr 工作**, 它只可以从一个 shared_ptr 或另一个 weak_ptr 对象构造, 它的构造和析构不会引起引用记数的增加或减少**。weak_ptr是用来解决shared_ptr相互引用时的死锁问题,如果说两个shared_ptr相互引用,那么这两个指针的引用计数永远不可能下降为0,资源永远不会释放。**它是对对象的一种弱引用，不会增加对象的引用计数，和shared_ptr之间可以相互转化，shared_ptr可以直接赋值给它，它可以通过调用lock函数来获得shared_ptr。**



**只要把上边两个类内的任意一个shared_ptr改为weak_ptr就可以避免这个死锁问题。因为weak_ptr将不引起计数的改变**

```c++
#include <memory>
#include <iostream>
using namespace std;
class TB;

class TA
{
public:
	~TA() {
		std::cout << "de TA" << std::endl;
	}
	std::weak_ptr<TB> tb;//
};

class TB
{
public:
	~TB() {
		std::cout << "de TB" << std::endl;
	}
	std::shared_ptr<TA> ta;
};

int main()
{
	{
		TA* yydA=new TA;
		TB* yydB=new TB;
		shared_ptr<TA> a = shared_ptr<TA>(yydA); // step1
		shared_ptr<TB> b = shared_ptr<TB>(yydB); // step2

		a->tb = b; // step3
		b->ta = a; // step4
		// step 5
	// step6
	return 0;
}
//在程序执行到 step1 的时候, 创建智能指针a，并赋值yydA，yydA指向的TA对象引用加1；
//在程序执行到 step2 的时候，创建智能指针b，并赋值yydB，yydB指向的TB对象引用加1；
//在程序执行到 step3 的时候，给a->tb赋值TB的对象，yydB指向的TB对象引用不增加；因为weak_ptr
//在程序执行到 step4 的时候，给b->ta赋值TA的对象，yydA指向的TA对象引用加1；
//在程序执行到 step5 的时候，yydA指向的TA对象引用为2，yydB指向的TB对象引用为1；
//在程序执行到 step6 的时候，b智能指针超出作用域开始析构，yydB指向的TB对象引用减1；a智能指针超出作用域开始析构，yydA指向的TA对象引用减1；
//此时TA对象引用为1，TB的对象引用为0；
//a和b智能指针先析构，b指针析构时发现TB对象引用为0，TB对象被析构，TB对象里的ta指针也被析构，所以TA对象引用减为0，然后TA对象也析构了。
```

**注意：**

- **我们不能通过weak_ptr直接访问对象的方法**
  
- 也就是这个指针不能访问它存的对象，得通过lock转化成shared_ptr才能访问
  
- 比如

  ```c++
  weak_ptr<TA> a=shared_ptr<TA>(new TA);
  a->tb;//错误，就禁止->这个用法
  ```

  - weak_ptr 没有重载*和->但可以使用 lock 获得一个可用的 shared_ptr 对象. 注意, weak_ptr 在使用前需要检查合法性.

  - ```c++
    weak_ptr<TA> a = shared_ptr<TA>(new TA);
    shared_ptr<TA> temp = a.lock();//通过lock转化成shared_ptr才能访问
    temp->tb;
    ```

- expired 用于检测所管理的对象是否已经释放, 如果已经释放, 返回 true; 否则返回 false.

- lock 用于获取所管理的对象的强引用(shared_ptr). 如果 expired 为 true, 也就是对象已经释放，那么返回一个空的 shared_ptr; 否则返回一个 shared_ptr, 其内部对象指向与 weak_ptr 相同.
- use_count 返回与 shared_ptr 共享的对象的引用计数.
- reset 将 weak_ptr 置空.
- weak_ptr 支持拷贝或赋值, 但不会影响对应的 shared_ptr 内部对象的计数.

### 6.智能指针有没有内存泄露的情况，如何解决

参看6.里的shared_ptr部分，解决方法是，两个类内的任意一个shared_ptr改为weak_ptr就可以避免这个死锁问题。因为weak_ptr将不引起计数的改变。

### 7.请你来说一说重载和重写覆盖

#### 1.重载（overload）（同名，入参一定要不一样，返回值随意）

重载：在同一作用域中，同名函数的形式参数（参数个数、类型或者顺序）不同时，构成函数重载，返回值类型没有要求。

```c++
class A
{
public:
	int 	func(int a);
	void 	func(int a, int b);
	void 	func(int a, int b, int c);    	
	int 	func(char* pstr, int a);//四个函数均构成重载。
};
```

需要注意的是：

1. **函数返回值类型与构成重载无任何关系**
2. **类的静态成员函数与普通成员函数可以形成重载**
3. **函数重载发生在同一作用域，如类成员函数之间的重载、全局函数之间的重载**

对于重载，最出名的应该就是**运算符重载**了吧。

#### 2.隐藏（hiding）(同名就可以覆盖，无论入参和返回值)

隐藏定义：**指不同作用域中定义的同名函数构成隐藏（不要求函数返回值和函数参数类型相同，只要同名就行）**。比如派生类成员函数隐藏与其同名的基类成员函数、类成员函数隐藏全局外部函数。

重写继承而来的非虚函数，就构成了隐藏。但是也可以通过HideA::hidefunc();把隐藏的函数给调出来

```c++
void hidefunc(char* pstr)
{
	cout << "global function: " << pstr << endl;
}

class HideA
{
public:
	void hidefunc()
	{
		cout << "HideA function" << endl;
	}

	void usehidefunc()
	{
		//隐藏外部函数hidefunc，使用外部函数时要加作用域
		hidefunc();
		::hidefunc("lvlv");
	}
};

class HideB : public HideA
{
public:
	void hidefunc()
	{
		cout << "HideB function" << endl;
	}

	void usehidefunc()
	{
		//隐藏基类函数hidefunc，使用外部函数时要加作用域
		hidefunc();
		HideA::hidefunc();
	}
};

```

#### 3.重写/覆盖（override）（一般是同名，入参，返回值都要相同;除了协变返回类型，就是说返回类型可以改变，首先得是返回指针，且指针指向需要是之前返回的指针指向类型的子类型）

- 重写的定义：**派生类中与基类同返回值类型、同名和同参数的虚函数重定义，构成虚函数覆盖，也叫虚函数重写**。
- **重写（覆盖）要求具有相同的参数和返回值**。而这个规则对于协变则会放松。需要注意的是，这里有一个特殊情况，即**协变返回类型**。
- 定义是：**如果虚函数返回指针或者引用时（不包括value语义），子类中重写的函数返回的指针或者引用是父类中被重写函数所返回指针或引用的子类型**。这样的类型称为协变返回类型（Covariant returns type)。看示例代码：

```c++
//B是A的子类型才行
class Base
{
public:
    virtual A& show()
    {
        cout<<"In Base"<<endl;
        return *(new A);
    }
};

class Derived : public Base
{
public:
     //返回值协变，构成虚函数重写
     B& show()
     {
        cout<<"In Derived"<<endl;
        return *(new B);
    }
};

```

对比覆盖和隐藏，不难发现**函数覆盖其实是函数隐藏的特例**。如果派生类中定义了一个与基类虚函数同名但是参数列表不同的非virtual函数，则此函数是一个普通成员函数，并形成对基类中同名虚函数的隐藏，而非虚函数覆盖。

**隐藏是一个静态概念，它代表了标识符之间的一种屏蔽现象，而覆盖则是为了实现动态联编，是一个动态概念**。

#### 4.final和override说明符

final 和 override 说明符出现在形参列表以及尾置返回类型之后。

final 还可以跟在类的后面，意思这个类不能当做其它类的基类。

##### 1.override写在子类里

通过上面的介绍，**我们知道派生类可以定义一个函数与基类中虚函数的名字相同但是形参列表不同的函数（隐藏，看清了这种情况是隐藏，没有覆盖，入参不一样）**。编译器将认为新定义的这个函数与基类中原有的函数是相互独立的。

但是我们在写虚函数时，想让派生类中的虚函数覆盖掉基类虚函数，有时我们会不小心写错，造成了隐藏（派生类函数与基类中虚函数的名字相同但是形参列表不同），这不是我们想要看到的结果。所以**C++ 11新标准中我们可以使用override关键字来说明派生类中的虚函数**。

**如果我们使用override标记了某个函数，但该函数并没有覆盖已存在的虚函数，此时编译器将报错**。

```c++
class B
{
	virtual void f1( int ) const;
	virtual void f2();
	void f3();
};
class C : B
{
	void f1( int ) const override;	//正确，f1与基类中的f1匹配
	void f2( int ) override;		//错误：B没有形如f2（int）的函数。如果不加override，将不会报错，你以为可以实现多态了，其实是你写错了隐藏了！并没有多态性
	void f3() override;			//错误：f3不是虚函数
	void f4() override;			//错误：B中没有名为f4的函数
};

```

使用override是希望能覆盖基类中的虚函数，如果不符合则编译器报错。

##### 2.final写在上一级（父类）的类里

我们还能把某个函数指点为 **final** ，意味着任何尝试覆盖该函数的操作都将引发错误：

```c++
class D : B
{
	//从B继承 f2() 和 f3()，覆盖 f1( int )
	void f1( int ) const final; //不允许后续的其它类覆盖 f1（int）
};
class E : D
{
	void f2();				//正确：覆盖从间接类B继承而来的f2
	void f1( int ) const;	//错误：D已经将 f2 声明成 final
};
```

#### 5.总结

在讨论相关概念的区别时，抓住定义才能区别开来。C++中函数重载、隐藏和覆盖的区别并不难。

在这里，牢记以下几点，就可区分函数重载、函数隐藏、函数覆盖和函数重写的区别：

1. **函数重载发生在相同作用域**
2. **函数隐藏发生在不同作用域**
3. **函数覆盖就是函数重写。准确地叫做虚函数覆盖和虚函数重写，也是函数隐藏的特例**

关于三者的对比，如下表所示：

| 三者                               | 作用域 | 有无virtual | 函数名 | 形参列表   | 返回值类型           |
| :--------------------------------- | :----- | ----------- | ------ | ---------- | -------------------- |
| 重载（函数重载，运算符重载）       | 相同   | 可有可无    | 相同   | 不同       | 可同可不同           |
| 隐藏（变量隐藏，函数隐藏）         | 不同   | 可有可无    | 相同   | 可同可不同 | 可同可不同           |
| 覆盖重写（虚函数覆盖和虚函数重写） | 不同   | 有          | 相同   | 相同       | 相同（协变可以不同） |

### 8.说一说strcpy和strlen

#### 1.strcpy

- strcpy
  - strcpy是字符串拷贝函数，原型：`char *strcpy(char* dest, const char *src);`
  - 从src逐字节拷贝到dest，直到遇到'\0'结束，因为没有指定长度，可能会导致拷贝越界，造成缓冲区溢出漏洞,安全版本是strncpy函数。
- 安全版本strncpy
  - 安全版本**char \*strncpy(char \*dest, const char \*src, size_t n)** 
  - 把 **src** 所指向的字符串复制到 **dest**，最多复制 **n** 个字符。当 src 的长度小于 n 时，dest 的剩余部分将用空字节填充。

#### 2.strlen

- strlen函数是计算字符串长度的函数，返回从开始到'\0'之间的字符个数。

- strlen("hello")返回的结果是5，是**不包含字符串结尾处的‘\0’**，但是strcpy(str1,str2)，**会拷贝str2中的‘\0’**。

- 建议在使用strcpy的时候，目标数组（第一个参数）的大小应该设置为strlen（）函数返回值+1 的值，或者建议使用如下的初始化数组方式：

  - ```c++
     char char_array[sizeof("hello")];//sizeof("hello")=6
     char * char_array_two = new char[strlen(str) + 1];//strlen(str)=5
    ```

### 9.回答一下++i和i++的区别

- ++i先自增1，再返回

  - ```c++
    int&  int::operator++（）
    {
    *this +=1；
    return *this；
    }
    ```

- i++先返回i,再自增1

  - ```c++
    const int  int::operator（int）
    {
    int oldValue = *this；
    ++（*this）；
    return oldValue；//返回旧值
    }
    ```

### 10.请你来写个函数在main函数执行前先运行

#### 方案1：通过gcc关键字

在 gcc 中可以使用 __attribute__ 关键字指定如下（在编译器编译的时候就决定了）

```c++
#include <stdio.h>

void startt() __attribute__((constructor));//__attribute__((constructor)) void startt();也行
void endd() __attribute__((destructor));

void startt() {
    printf("this is function %s\n", __func__);
    return;
}

void endd() {
    printf("this is function %s\n", __func__);
    return;
}

int main() {
    printf("this is function %s\n", __func__);
    return 0;
}

// 输出结果
// this is function before
// this is function main
// this is function after
```

#### 方案2：通过全局变量+lambda函数

**全局static变量**的初始化在程序初始阶段，先于main函数的执行，所以可以利用这一点。

注意x是全局静态变量，不是函数。最后一个()的意思就是执行

```c++
static int x = []() {//不加static，因为普通全局变量和static全局变量的区别就是外部链接性和内部链接性的区别，初始化和生命期是一样的
    cout << "before main" << endl;
    return 2;
}();//[](){}是定义了一个lambda函数，然后再()就是执行了这个函数，x是其返回值2。
int main()
{
    cout<<"main"<<endl;
    return0;
}
//输出
//before main
//main
```

趁此演示一下lambda函数的用法

```c++
static int x = []() {
    cout << "before main" << endl;
    return 2;
}();//[](){}是定义了一个lambda函数，然后再()就是执行了这个函数，x是其返回值2
int main()
{
    cout<<"main"<<endl;
    cout<<x<<endl;//2
    //底下演示一下lambda函数用法
    
    auto func = []() {//注意，它没有运行
     	cout << "hello" << endl;
        return;
    };
    func();//hello
    
    []() {
        cout << "yyd" << endl;
    }();//yyd
    return 0;
}
//输出
//before main
//main
//2
//hello
//yyd
```

```c++
//注意：这里的func就是个函数。
auto func=[](){
    return 1;
};//对于没有运行的纯lambda函数，必须使用auto来承接，不管你返回什么类型，或者不返回
auto func2=[](){
    return 1.2;
}
int a=func();
double b=func2();


//对于这种直接执行了的，它返回的就是个数值！
auto a=[](){
    return 1;
}();//

```

#### 方案3：通过全局变量+类的构造函数

```c++
class BeforeMain {
public:
    BeforeMain() {
        cout << "Before main" << endl;
    };
};
BeforeMain bM; // 利用全局变量和构造函数的特性,通过全局变量的构造函数执行

int main()
{
    cout << "main" << endl;
    return 0;
}
//输出
//before main
//main
```



### 11.有段代码写成了下边这样，如果在只修改一个字符的前提下，使代码输出20个hello? 

```c++
for(int i = 0; i < 20; i--) 
	cout << "hello" << endl;
```

答案：

```c++
for(int i = 0; i + 20; i--)
cout << "hello" << endl;
```

### 12.shared_ptr和weak_ptr的核心实现

#### 1.方案一:一个counter，只有一个问题，无法实现weakPtr::expired()!（这种写法比较简单）

```c++
template<class T>
class WeakPtr;

template<class T>
class SharePtr
{
friend class WeakPtr<T>;//方便weak_ptr与share_ptr设置引用计数和赋值
public:
	SharePtr(T* p = nullptr) :_ptr(p)//构造函数
	{
		cnt = new int(0);
		if (p)
			*cnt = 1;//传入非空指针，将share_ptr计数从0置为1，
	}

	~SharePtr()//析构函数
	{		
		reset();
	}

	SharePtr(SharePtr<T> const& s) :_ptr(s._ptr), cnt(s.cnt)//复制构造
	{
		(*cnt)++;
	}

	SharePtr(WeakPtr<T> const& w) //为了用weak_ptr的lock()，来生成share_ptr用，需要拷贝构造用
	{
		_ptr = w._ptr;
		cnt = w.cnt;
		(*cnt)++;
	}

	SharePtr<T>& operator=(SharePtr<T>& s)//赋值
	{
		if (this != &s)//处理自我赋值
		{
			reset();
			_ptr = s._ptr;
			cnt = s.cnt;
			(*cnt)++;
		}
		return *this;
	}

	T& operator*()
	{
		return *_ptr;
	}

	T* operator->() {
		return _ptr;
	}


	void reset()
	{
        if (cnt != nullptr)
        {
            (*cnt)--;
            if ((*cnt) <= 0)
            {
                delete _ptr;
                delete cnt;
            }
            _ptr = nullptr;
            cnt = nullptr;
        }
	}

private:
	T* _ptr;//管理的对象的raw指针
	int* cnt;//引用计数
};


template<class T>
class WeakPtr
{
friend class SharePtr<T>;//写friend class SharePtr不行，必须得有<T>
public:

	WeakPtr()
	{
		_ptr = nullptr;
		cnt = nullptr;
	}

	WeakPtr(SharePtr<T>& s) :_ptr(s._ptr), cnt(s.cnt) {};
	WeakPtr(WeakPtr<T>& w) :_ptr(w._ptr), cnt(w.cnt) {};
	~WeakPtr()
	{
		reset();
	}
	void reset()
	{
		_ptr = nullptr;
		cnt = nullptr;
	}

	WeakPtr<T>& operator=(WeakPtr<T>& w)
	{
		_ptr = w._ptr;
		cnt = w.cnt;
		return *this;
	}

	WeakPtr<T>& operator=(SharePtr<T>& s)
	{
		cnt = s.cnt;
		_ptr = s._ptr;
		return *this;
	}

	SharePtr<T> lock()
	{
		return SharePtr<T>(*this);
	}

	/*bool expired()//这个无法实现
	{
		//return _ptr == nullptr;
	}*/

private:
	T* _ptr;
	int* cnt;
};
```



#### 2.两个counter，一个记录share_ptr，一个记录weak_ptr（这个方案没深究，写起来比较复杂）

- 可能纠结为什么counter的析构在shared_ptr中，其实因为weak_ptr总是依赖于shared_ptr构造的，所以weak_ptr总是比shared_ptr晚构造，也就比它先析构。所以shared_ptr中释放counter可以成立。但是你要是写成static weakptr()..这counter肯定没释放，资源泄露了。
- 这种写法可以实现weak_ptr::expired()
- s是share_ptr的引用计数，w是weak_ptr的引用计数。s为0时，删除引用对象；当w为0时，删除Counter对象。

##### Counter简单实现

counter对象的目地就是用来申请一个块内存来存引用基数，s是share_ptr的引用计数，w是weak_ptr的引用计数，当w为0时，删除Counter对象。

```c++
class Counter
{
public:
    Counter() : s(0), w(0){};
    int s;	//share_ptr的引用计数
    int w;	//weak_ptr的引用计数
};
```



##### shared_ptr的简单实现

shared_ptr的给出的函数接口为：构造，拷贝构造，赋值，解引用，通过release来在引用计数为0的时候删除_ptr和cnt的内存。

```c++
template <class T>
class WeakPtr; //为了用weak_ptr的lock()，来生成share_ptr用，需要拷贝构造用

template <class T>
class SharePtr
{
public:
    SharePtr(T* p = 0) : _ptr(p)
    {
        cnt = new Counter();
        if (p)
            cnt->s = 1;

    }
    ~SharePtr()
    {
        release();
    }

    SharePtr(SharePtr<T> const& s)
    {

        _ptr = s._ptr;
        (s.cnt)->s++;

        cnt = s.cnt;
    }
    SharePtr(WeakPtr<T> const& w) //为了用weak_ptr的lock()，来生成share_ptr用，需要拷贝构造用
    {
        
        _ptr = w._ptr;
        (w.cnt)->s++;
        cnt = w.cnt;
    }
    SharePtr<T>& operator=(SharePtr<T>& s)
    {
        if (this != &s)
        {
            release();
            (s.cnt)->s++;
            cnt = s.cnt;
            _ptr = s._ptr;
        }
        return *this;
    }
    T& operator*()
    {
        return *_ptr;
    }
    T* operator->()
    {
        return _ptr;
    }
    friend class WeakPtr<T>; //方便weak_ptr与share_ptr设置引用计数和赋值

    void release()
    {
        if (cnt)
        {
            cnt->s--;
            if (cnt->s < 1)
            {
                delete _ptr;
                _ptr = nullptr;
                if (cnt->w < 1)
                {
                    delete cnt;
                    cnt = NULL;
                }
            }
        }
    }

private:
    T* _ptr;
    Counter* cnt;
};
```

##### weak_ptr简单实现

```c++
template <class T>
class WeakPtr
{
public: //给出默认构造和拷贝构造，其中拷贝构造不能有从原始指针进行构造
    WeakPtr()
    {
        _ptr = nullptr;
        cnt = nullptr;
    }
    WeakPtr(const SharePtr<T>& s) : _ptr(s._ptr), cnt(s.cnt)//从shareptr构造weakptr
    {
        if(cnt)
            cnt->w++;
    }
    WeakPtr(const WeakPtr<T>& w) : _ptr(w._ptr), cnt(w.cnt)//从weakptr构造weakptr
    {
        if(cnt)
        cnt->w++;
    }
    ~WeakPtr()//析构
    {
        release();
    }
    WeakPtr<T>& operator=(const WeakPtr<T>& w)
    {
        if (this != &w)
        {
            release();
            cnt = w.cnt;
            cnt->w++;
            _ptr = w._ptr;
        }
        return *this;
    }
    WeakPtr<T>& operator=(SharePtr<T>& s)
    {
        release();
        cnt = s.cnt;
        cnt->w++;
        _ptr = s._ptr;
        return *this;
    }
    SharePtr<T> lock()
    {
        return SharePtr<T>(*this);
    }
    bool expired()
    {
        if (cnt)
        {
            if (cnt->s > 0)
            {
                return false;
            }
        }
        return true;
    }
    friend class SharePtr<T>; //方便weak_ptr与share_ptr设置引用计数和赋值

    void release()
    {
        if (cnt)
        {
            cnt->w--;
            _ptr = nullptr;
            cnt = nullptr;
        }
    }

private:
    T* _ptr;
    Counter* cnt;
};
```

### 13.内存

#### 1.从系统层次上考虑

#### 1.1数据区、静态区、读写区（全局区）

- bss段：**指那些没有初始化的和初始化为0的全局变量\静态变量。**
  - bss类型的全局变量只占运行时的内存空间，而不占文件空间
  - os里指未初始化数据段。程序加载时会全部赋值为0。
- data段：**data指那些初始化过（非零）的非const的全局变量\静态变量。**（因为如果数据全是零，为了优化考虑，编译器把它当作bss处理）
  - data类型的全局变量是即占文件空间，又占用运行时内存空间的
  - os里指已初始化数据段。

#### 1.2正文段、代码区、只读区

-  rodata段：ro代表**read only**，即只读数据(const)。

  - 当**const全局变量取地址或者使用extern时**，会分配内存,const全局变量才会存放在rodata。(如果没有取地址操作，const全局变量就在符号表里。)

  - 常量不一定就放在rodata里，有的立即数直接编码在指令里，存放在代码段(.text)中。

    - ```
      比如cout << 1 << endl;里的1
      ```

  - 对于字符串常量，编译器会**自动去掉重复的字符串**，保证一个字符串在一个可执行文件(EXE/SO)中只存在一份拷贝。

    - ```
      chara[]="123";
      charb[]="123";
      //“123”存在rodata里，但是只保留一份
      ```

- text段：存放代码

#### 1.3堆

#### 1.4栈

#### 2.C++中内存五区

![image.png](https://i.loli.net/2020/09/22/RxzAfspX2uKEhb6.png)

- （1）**栈区**：局部变量，函数传参值，自动释放，效率高但内存少
- （2）**堆区**：malloc函数从堆上申请内存，用free释放内存，若不释放，程序结束释放
  - 这个有点狭隘了，因为堆（heap）是C语言和操作系统的术语。
  - **或者叫自由存储区**：自由存储区是C++基于new操作符的一个抽象概念。new操作符在此申请内存，用delete释放内存，若不释放，程序结束释放
  - 自由存储区是否能够是堆（问题等价于new是否能在堆上动态分配内存），这取决于operator new 的实现细节。自由存储区不仅可以是堆，还可以是静态存储区，这都看operator new在哪里为对象分配内存。
  - 个人理解，自由存储区包含堆，因为new可以使用堆内存，也可以使用其他内存,比如静态区内存（需要你提前预分配内存，供给new使用）。
- （3）**全局/静态区**：存储全局变量或静态变量。内存在编译时就分配好了（程序执行前），整个程序运行期间都存在，程序结束时释放。
  - 对应bss段和data段
- （4）**常量存储区**：存储常量（const），不允许修改。
  - 对应rodata段，包括**const全局变量取地址或者使用extern时**，会分配内存,const全局变量才会存放在rodata；
  - 还有字符串
- （5）**代码区**
  - 对应text段

堆是操作系统维护的一块内存，而自由存储是C++中通过new与delete动态分配和释放对象的抽象概念。堆与自由存储区并不等价。



### 14.C++里是怎么定义常量的？常量存放在内存的哪个位置？

https://blog.csdn.net/woainilixuhao/article/details/86521357

- c语言

  - c语言中const全局变量存储在只读数据段，编译期最初将其保存在符号表中，第一次使用时为其分配内存，在程序结束时释放。
  - 而const局部变量（局部变量就是在函数中定义的一个const变量，）存储在栈中，代码块结束时释放。
  - 在c语言中可以通过指针对const局部变量进行修改，而不可以对const全局变量进行修改。因为const全局变量是存储在只读数据段

- C++

  - **在c++中，一个const不是必需创建内存空间，而在c中，一个const总是需要一块内存空间。**

  - **C++中的const全局变量**

    - c++中**是否要为const全局变量分配内存空间，取决于这个const变量的用途**
    - **如果是充当着一个值替换（即就是将一个变量名替换为一个值），那么就不分配内存空间（那它就是在符号表里）**
    - **不过当对这个const全局变量取地址或者使用extern时，会分配内存，存储在只读数据段(rodata段)。也是不能修改的。**

  - **c++中的const局部变量**

    - 对于基础数据类型，也就是**const int a = 10这种，编译器会把它放到符号表中，不分配内存。如果当对其取地址时，会分配内存，在栈上**

      - ```c++
        const int B=100;
        int main()
        {
            volatile const int A = 2;//const局部变量有取地址操作，分配到栈上
            int* p = (int*)&A;
            *p = 1;
            cout << A << endl;//1
            cout << *p << endl;//1
            
            p = (int*)&B;
        	*p=200;//报错了，因为const全局变量有取地址操作时，会分配内存，分配在rodata段
        	return 0;
        }
        ```

    - 对于基础数据类型，如果用一个变量初始化const变量，如果const int a = b,那么也是会给a分配内存到栈

    - 对于自定数据类型，比如类对象，那么也会分配内存到栈

**简略总结：**

- 对于普通的全局变量、静态变量，存放在数据区（.bss或者.data，区别就是有没有初始化）
  - 对于const全局变量取地址或者使用extern时，const全局变量存放在rodata。如果没有取地址或者使用extern，那它就在符号表，没有分配内存。
- 对于普通的局部变量，存放在栈
  - 对于const局部变量取地址时，const局部变量会存放在栈上。如果没有取地址操作时，那它就在符号表，没有分配内存。
  - 还有两个情况也会分配内存（用一个变量初始化const变量或者是对于const局部自定义类对象）

### 15.顶层const和底层const

#### 1.底层const（指向常量的指针，其实只是不能通过指针来修改而已，只是对指针来看似乎是常量）

**指向常量的指针**：代表**不能改变其指向内容**的指针。声明时const可以放在类型名前后都可，拿int类型来说，声明时：const int和int const 是等价的。声明指向常量的指针也就是**底层const**，下面举一个例子：

```c++
int num_a = 1;
const int *p_a = &num_a; //底层const
//*p_a = 2;  //错误，指向“常量”的指针不能改变所指的对象
num_a = 2;
cout << *p_a << endl;//2
```

注意：指向“常量”的指针不代表它所指向的内容一定是常量，只是代表不能通过**解引用符**（操作符*）来改变它所指向的内容。上例中指针p_a指向的内容就不是常量，可以通过赋值语句：num_a=2;  来改变它所指向的内容。

```c++
const int a = 1;  
//int * pi = &a;  //错误，&a是底层const，不能赋值给非底层const 
const int * pi = &a; //正确，&a是底层const，可以赋值给底层const
```

#### 2.顶层const（指针常量，指针里的地址不能改变）

代表指针本身是常量，声明时必须初始化，之后**它存储的地址值就不能再改变**。声明时const必须放在指针符号`*`后面，即：`*const` 。声明常量指针就是**顶层const**，下面举一个例子：

```c++
int num_b = 2;
int *const p_b = &num_b; //顶层const
//p_b = &num_a;  //错误，常量指针不能改变存储的地址值
```

#### 3.区分

- 1.执行对象拷贝时有限制，**常量的底层const不能赋值给非常量的底层const**。也就是说，你只要能正确区分顶层const和底层const，你就能避免这样的赋值错误。下面举一个例子：

  - ```c++
    int num_c = 3;
    const int *p_c = &num_c;  //p_c为底层const的指针
    //int *p_d = p_c;  //错误，不能将底层const指针赋值给非底层const指针。自己理解一下，因为你本来就无法通过p_c来修改num_c，如果这条通过编译了，那么将允许p_d去修改num_c。这显然不合理。但是可以通过const_cast来获取该权限
    const int *p_d = p_c; //正确，可以将底层const指针复制给底层const指针
    ```

- 2 使用命名的强制类型转换函数const_cast时，需要能够分辨底层const和顶层const，因为**const_cast只能改变运算对象的底层const**。下面举一个例子：

  - ```c++
    int num_e = 4;
    const int *p_e = &num_e;
    //*p_e = 5;  //错误，不能改变底层const指针指向的内容
    int *p_f = const_cast<int *>(p_e);  //正确，const_cast可以改变运算对象的底层const。但是使用时一定要知道num_e不是const的类型。
    *p_f = 5;  //正确，非顶层const指针可以改变指向的内容
    cout << num_e;  //输出5
    ```

### 16.隐式类型转换

#### 1.内置数据类型

在数据类型上表现是**少字节数据类型，转换成多字节数据类型**，保证数据的完整性；

或者说低精度的变量给高精度变量赋值会发生隐式类型转换

```c++
int i=3;
double j = 3.1;
i+j;//i会被转换成double类型，然后才做加法运算。
```

#### 2.类类型

对于只存在单个参数的构造函数的对象构造来说，函数调用可以直接使用该参数传入，编译器会自动调用其构造函数生成临时对象。

- **可以用 单个形参来调用 的构造函数定义了从 形参类型 到 该类类型 的一个隐式转换。**
- “可以用单个形参进行调用” 并不是指构造函数只能有一个形参，而是它**可以有多个形参，但那些形参都是有默认实参**的。(**可以使用一个实参进行调用，不是指构造函数只能有一个形参。**)
-  **explicit只能用于类内部构造函数的声明。**
- **隐式类类型转换容易引起错误，除非你有明确理由使用隐式类类型转换，否则，将可以用一个实参进行调用的构造函数都声明为explicit来防止隐式转换。**

```c++
class Test
{
	public:
		Test(int i,double j=2.0);
};

Test t1 = 1;//正确，由于强制类型转换，1先被Test构造函数构造成Test对象，然后才被赋值给t1
Test t2(1);//正确
```

```c++
class Test
{
	public:
		explicit Test(int i,double j=2.0);
};

Test t1 = 1;//错误！由于禁止隐式转换，不存在由'int'到'Test'的适当构造函数
Test t2(1);//正确
```

### 17.C++函数栈空间的最大值

windows默认是1M，不过可以调整。

linux默认是8M。这个函数栈其实就是主线程栈，每个子线程也会有新的8M。ulimit -s这个命令可以查看系统默认的线程栈空间

```c++
//windows下
int main()
{
    char a[1024*1024];//1M，直接报错
    //vector<char> a(1024*1024);//这绝对不会报错，因为动态数组，数据存在堆上
    return 0;
}
//linux下
int main()
{
    char a[8*1024*1024];//8M，直接报错
    return 0;
}
```

### 18.extern的用法(与存储持续性、作用域和链接性有重复)

extern关键字有两个作用

#### 1.告知编译器extern “C” 

-  当它与"C"一起连用时，如: extern "C" void fun(int a, int b);则**告诉编译器在编译fun这个函数名时按着C的规则去翻译相应的函数名而不是C++的。**
- 因为C++支持函数的重载，所以**C++的规则在翻译这个函数名时会把fun这个名字变得面目全非**（为了区分不同的重载函数），可能是fun@aBc_int_int#%$也可能是别的，这要看编译器的"脾气"了(不同的编译器采用的方法不一样)。
- 而C不支持，这就导致了C++在编译的时候，C++的函数名会和参数一起被编成函数名，**而C只是函数名**。
- 所以在链接的时候，找不到我们定义的那个函数。（指的是c文件中编写的函数实现，而我们cpp文件中无法正确调用）

**举例：**

我们有一个函数是.c文件即c语言写的，之前写好的，我们现在要在另一个cpp文件里调用它

错误版本：

```c++
//a.c
int foo() {
	return 1;
}

//b.cpp
#include<iostream>
using namespace std;
int foo();//没有extern"C“声明时

int main()
{
    cout << foo() << endl;
    return 0;
}
//可以通过编译!但是链接时报错:b.obj中无法解析的外部符号"int_cdecl foo(void)" (?foo@@YAHXZ)，函数main中引用了该符号
```

正确版本：

```c++
//a.c
int foo() {
	return 1;
}

//b.cpp
#include<iostream>
using namespace std;
extern"C" {
    int foo();
}

int main()
{
    cout << foo() << endl;
    return 0;
}
//运行正确！
```

**典型的写法：在头文件中加入**

```c++
#ifdef __cplusplus
extern "C" {
#endif

/*add somethings*/

#ifdef __cplusplus
}
#endif
```

#### 2.共享(参见一、1.extern的跨文件用法)

extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外extern也可用来进行链接指定。

extern:该变量/函数是从别的文件来的，或者该变量/函数可以被别的文件使用。

`extern int yyd;//这是声明`

`extern int yyd=1;//这是定义`

1.两个文件都加上extern声明，不过只能有一个定义。正确

```c++
//a.cpp
#include <iostream>
using namespace std;
extern int yyd = 1;
void test2();
int main() {
    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
extern int yyd;
void test2() {
    cout << yyd<<endl;
}
```

2.第一个文件里没有extern，第二个文件extern声明。正确

```c++
//a.cpp
#include <iostream>
using namespace std;
int yyd = 1;
void test2();
int main() {
    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
extern int yyd;
void test2() {
    cout << yyd<<endl;
}
```

3.各种错误实例

```c++
//a.cpp
int yyd;
//b.cpp
int yyd;//错误!


//a.cpp
extern int yyd=0;
//b.cpp
int yyd;//错误！
```

**总结：**

- 只能有一个份定义。定义时加不加extern都行
- 定义
  - `extern int  yyd=1;`
  - 或者
  - `int yyd=1;`
- 声明
  - `extern int yyd;`

#### 3. extern 和 static

-  (1) extern 表明该变量在别的地方已经定义过了,在这里要使用那个变量.
-  (2) static 表示静态的变量，分配内存的时候, 存储在静态区,不存储在栈上面.

**区别：**这两个相当于水火不容，一个是为了限制在本文件内，一个是为了跨文件

- extern和static不能同时修饰一个变量
- static修饰的全局变量声明与定义同时进行，也就是说当你在头文件中使用static声明了全局变量后，它也同时被定义了。
  - extern修饰的全局变量，声明和定义可以分开。
- static修饰全局变量的作用域只能是本身的编译单元，也就是说它的“全局”只对本编译单元有效，其他编译单元则看不到它。
  - extern就是为跨文件而生

**全局静态变量的作用**

- 如果文件a定义了一个静态外部变量（全局静态变量），其名称与另一个文件b中声明的常规外部变量（普通全局变量）相同，则在该文件a中，全局静态变量将隐藏外部的全局变量.

- 可使用链接性为内部的静态变量（全局静态变量）**在同一个文件**中的多个函数之间共享数据。

- 如果将作用域为整个文件的变量变为静态的，就不必担心其名称与其他文件中的作用域为整个文件的变量发生冲突。

- 其实总结就是用static将变量的链接性变为了内部的，使之只能在一个文件中使用。

- 用static也可以将函数的链接性变为了内部的，使之只能在一个文件中使用。要知道**函数，默认都是自带extern的。使用static可以将函数只能在本文件使用**。

  - ```c++
    //a.cpp
    static int parivate(double x);
    ...
    static int private(double x);
    ```

```c++
- extern和static肯定是不能同时出现的。因为extern是为了说明这是个跨文件的变量。而static则是限定，这是个只支持在本文件使用的变量。二者是矛盾和对立的。

​```c++
//a.cpp
int yyd=20;//普通的全局变量
//b.cpp
static int yyd=10;//你要是写int yyd=10;会报错的
//你要是写extern int yyd;//那就是引入了a文件的变量
//而写static int yyd=10，则是不会报错，隐藏了a文件的普通全局变量yyd
```

```c++
//a.cpp
#include <iostream>
using namespace std;
int yyd=1;
void test2();
int main() {

    test2();
    return 0;
}
//b.cpp
#include <iostream>
using namespace std;
static int yyd;
void test2() {
    int yyd = 2;
    cout << "局部变量："<<yyd<<endl;
    cout << "全局变量:" << ::yyd << endl;
}
//输出
"局部变量：" 2
"全局变量:"  0
```

```c++
//a.cpp
int tom=3;
int dick=30;
static int harry=300;

//b.cpp
extern int tom;
static int dick=10;
int harry=200;

//两个文件使用了同一个tom变量，但使用了不同的dick和harry变量。两个tom变量的地址相同，而两个dick和harry变量的地址不同。
```

#### 4.extern 和const

- 默认const全局变量的链接性是内部的，如同自带static。如果你希望是外部的，需要定义里自己加上extern
- 一般的全局变量的链接性为外部的，但是const全局变量的链接性是内部的。也就是说，在C++看来，全局const定义，就如同自带static一样

```c++
int a=1;//全局变量，链接性是外部的
static int b=2;//全局静态变量，链接性是内部的
const int c=3;//全局const变量，链接性是内部的
extern const int d=4;//全局const变量，链接性是外部的
const char* g_str="123456";//特别注意！这是个普通的全局const变量，链接性是外部的。因为const修饰的是char*而不是g_str
int mian()
{
    ...
}
```

**特别注意**：`const char* g_str = "123456"`，g_str只是一个普通的全局变量，不是const全局变量，链接性是外部，因为const修饰的是char*而不是g_str;

```c++
我只是想提醒你，
 const char* g_str = "123456" 与 const char g_str[] ="123465"是不同的， 前面那个const 修饰的是char *而不是g_str,它的g_str并不是常量，它被看做是一个定义了的全局变量（可以被其他编译单元使用）， 所以如果你像让char*g_str遵守const的全局常量的规则，最好这么定义
 const char* const g_str="123456".
    
//a.cpp
const char* g_str="123456";//特别注意！这是个普通的全局const变量，链接性是外部的。因为const修饰的是char*而不是g_str
const int a=1;//全局const变量，链接性是内部的
extern const int b=1;//全局const变量，链接性是外部的
//b.cpp
extern const char* g_str;//正确
extern const int a;//错误！链接时找不到
extern const int b;//正确
```

### 19.回答一下new与malloc的区别是什么

在这里注意了，free()释放的是指针指向的内存！注意！释放的是内存，不是指针！这点非常非常重 要！指针是一个变量，只有程序结束时才被销毁。释放了内存空间后，原来指向这块空间的指针还是存在！只不过现在指针指向的内容的垃圾，是未定义的，所以说是垃圾。因此，释放内存后应把把指针指向NULL，防止指针在后面不小心又被使用，造成无法估计的后果。

#### 1.申请的内存所在位置（一个自由存储区一个堆）

new操作符从**自由存储区（free store）**上为对象动态分配内存空间，而malloc函数从**堆**上动态分配内存。自由存储区是C++基于new操作符的一个抽象概念，凡是通过new操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C语言使用malloc从堆上分配内存，使用free释放已分配的对应内存。

那么自由存储区是否能够是堆（问题等价于new是否能在堆上动态分配内存），这取决于operator new 的实现细节。自由存储区不仅可以是堆，还可以是静态存储区，（或者栈也都有可能）这都看operator new()在哪里为对象分配内存。

**举例：new从静态存储区分配内存的例子。思路就是提前分配好一块静态区内存，供给new使用。（其实内存不是new分配的，而是new来定位）**

堆上new的对象需要手动delete，而静态区上new的无需自己管理。

```c++

#include <iostream>
#include<stdio.h>
#include <new>//需要添加该头文件
using namespace std;
const int ARRLEN = 100;
int static_buf[ARRLEN];//提前分配好一块静态区内存，供给new使用。
const int N = 5;
int main()
{
    int* p1;//指针变量p1存在栈中,存储的地址指向堆。p1需要手动释放，即delete[]
    int* p2; //指针变量p2存在栈中，存储的地址指向静态区。p2自动释放，无需手动delete，因为内存在静态区，静态区的内存程序员从来不需要管理。
    //避免了内存泄露，但是牺牲了内存访问的独立性（第二次new的时候，第一次new产生的数据的没了）。
    cout << "   static_buf address:" << static_buf << endl;
    for (int j = 0; j < 3; j++)
    {
        cout << "\n\n\n";
        p1 = new int[N];
        p2 = new (static_buf)int[N];//从指定区域static_buf地址分配内存，这里是从静态区分配。其
        //实这就是定位new，内存不是new来分配的，而是之前分配好的，new只是起到了定位的作用。new (static_buf)int[N]; new (static_buf)将地址static_buf变成void*，然后再进行对象初始化，即int[N]初始化，整个表达式返回一个int*
        for (int i = 0; i < N; i++)
        {
            p1[i] = p2[i] = i;
            cout << "   p1-->" << &p1[i] << "   value:" << p1[i] << "  ";
            cout << "   p2-->" << &p2[i] << "   value:" << p2[i] << endl;//打印出每个元素的地址和值
        }

        delete[]p1;
    }
    return 0;
}
```

上边的代码，反复执行了3次heap new和3次静态区new。每次在堆上new的地址都不一样，而使用静态区内存new，每次new都是使用的同一个静态区（你自己预留的内存）。

![image.png](https://i.loli.net/2020/09/09/j3z9RilNWLBf8cJ.png)



**定位new：（new从静态存储区分配内存的例子其实用的就是这个，内存不是new分配的，而是提前分配好的，new只是来定位）**

```c++
new (place_address) type//第一步，new将place_address转换成void*，然后在该地址处进行对象初始化工作，即type
//举例：
int* p = new (static_buf)int[N];  new (static_buf)将地址static_buf变成void*，然后再进行对象初始化，即int[N]初始化，整个表达式返回一个int*
```

place_address为一个指针，代表一块内存的地址。当使用上面这种仅以一个地址调用new操作符时，new操作符调用特殊的operator new，也就是下面这个版本：

```c++
void* operator new(size_t,void*) //不允许重定义这个版本的operator new
```

这个operator new**不分配任何的内存**，它只是简单地返回指针实参，然后右new表达式负责在place_address指定的地址进行对象的初始化工作。

简单来说就是**定位new**运算符只是返回传递给它的地址，并将其强制转换为void *，以便能够赋给任何指针类型。

#### 2.返回类型安全性（new是类型安全性操作符，无需类型转换，而malloc需要强制转换）

- **new操作符内存分配成功时，返回的是对象类型的指针**，类型严格与对象匹配，无须进行类型转换，故new是符合**类型安全**性的操作符。
- 而**malloc内存分配成功则是返回void *** ，需要通过强制类型转换将void*指针转换成我们需要的类型。

类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域。关于C++的类型安全性可说的又有很多了。

#### 3.内存分配失败时的返回值（new失败抛出bad_allloc异常，malloc失败返回NULL）

- new内存分配失败时，会抛出bad_alloc异常，它**不会返回NULL**；
- malloc分配内存失败时返回NULL。

在使用C语言时，我们习惯在malloc分配内存后判断分配是否成功：

```c++
int *a  = (int *)malloc ( sizeof (int ));
if(a==NULL)//说明malloc分配失败
{
    ...
}
else
{
    ...
}
```

从C语言走入C++阵营的新手可能会把这个习惯带入C++：

```c++
int * a = new int();
if(a==NULL)
{
    ...
}
else
{   
    ...
}
```

实际上这样做**一点意义也没有**。因为c++里的new根本不会返回NULL，而且程序能够执行到if语句已经说明内存分配成功了，如果失败早就抛异常了。正确的做法应该是使用异常机制：

```c++
try
{
    int *a = new int();
}
catch (bad_alloc)
{
    ...
}
```

#### 4.是否需要指定内存大小（new编译器会自行根据类型计算所需内存空间，malloc需要人工显式地指出所需空间）

- 使用new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算
- 而malloc则需要显式地指出所需内存的尺寸。

```c++
int* p1=new int;
int* p2=(int*)malloc(sizeof(int));
```

```c++
class A{...}
A * ptr = new A;
A * ptr = (A *)malloc(sizeof(A)); //需要显式指定所需内存大小sizeof(A);
```

当然了，我这里使用malloc来为我们自定义类型分配内存是不怎么合适的，请看下一条。

#### 5.是否调用构造函数/析构函数（new将分配内存，且调用构造函数。malloc只是机械的与该类对象匹配的内存而已，然后强行把它解释为【这是一个对象】，它并没有进行构造函数的调用）

- new的功能是在堆区新建一个对象，并返回该对象的指针。
  - 所谓的【新建对象】的意思就是，将调用该类的构造函数，因为如果不构造的话，就不能称之为一个对象。
- 而malloc只是机械的分配一块内存，如果用mallco在堆区创建一个对象的话，是不会调用构造函数的
  - 严格说来用malloc不能算是新建了一个对象，只能说是与该类对象匹配的内存而已，然后强行把它解释为【这是一个对象】，按这个逻辑来，也不存在构造函数什么事（Malloc不做任何解释，只是机械的分配一块某大小的内存，然后返回void*，解释是程序员强制转换的结果）

- 同样的，用delete去释放一个堆区的对象，会调用该对象的析构函数
- 用free去释放一个堆区的对象，不会调用该对象的析构函数

```c++
#include <iostream>
#include <malloc.h>

class TEST
{
private:
    int num1;
    int num2;
public:
    TEST()
    {
        num1 = 10;
        num2 = 20;
    }
    void Print()
    {
        std::cout << num1 << " " << num2 << std::endl;
    }
};

int main(void)
{
    // 用malloc()函数在堆区分配一块内存空间，然后用强制类型转换将该块内存空间
    // 解释为是一个TEST类对象，这不会调用TEST的默认构造函数
    TEST * pObj1 = (TEST *)malloc(sizeof(TEST));//Malloc不做任何解释，返回void*,解释是程序员强制转换的结果
    pObj1->Print();

    // 用new在堆区创建一个TEST类的对象，这会调用TEST类的默认构造函数
    TEST * pObj2 = new TEST;
    pObj2->Print();

    return 0;
}
/*
运行结果：

-----------------------------
-842150451 -842150451       |
10 20                       |
请按任意键继续. . .         |
-----------------------------

我们可以看到pObj1所指的对象中，字段num1与num2都是垃圾值。因为没有经过构造函数构造。
而pObj2所指的对象中，字段num1与num2显然是经过了构造后的值
*/
```

这也是上面我为什么说使用malloc/free来处理C++的自定义类型不合适，其实不止自定义类型，标准库中凡是需要构造/析构的类型通通不合适（比如string）。

#### 6.对数组的处理（C++提供了new[]与delete[]来专门处理数组类型，new对数组的支持体现在它会分别调用构造函数函数初始化每一个数组元素，释放对象时为每个对象调用析构函数。但是malloc就只会给你一个原始内存，并且还需要我们手动自定数组的大小）

C++提供了new[]与delete[]来专门处理数组类型:

```c++
A* ptr = new A[10];//分配10个A对象
```

使用new[]分配的内存必须使用delete[]进行释放：

```c++
delete[] ptr;
```

new对数组的支持体现在它会分别调用构造函数函数初始化每一个数组元素，释放对象时为每个对象调用析构函数。注意delete[]要与new[]配套使用，不然会找出数组对象部分释放的现象，造成内存泄漏。

至于malloc，它并不知道你在这块内存上要放的数组还是啥别的东西，反正它就给你一块原始的内存，在给你个内存的地址就完事。所以如果要动态分配一个数组的内存，还需要我们手动自定数组的大小：

```c++
int * ptr = (int *) malloc( 10*sizeof(int) );//分配一个10个int元素的数组
```

#### 7.new与malloc是否可以相互调用（operator new() 的实现可以基于malloc，而malloc的实现不可以去调用new）

operator new /operator delete的实现可以基于malloc，而malloc的实现不可以去调用new。下面是编写operator new /operator delete 的一种简单方式，其他版本也与之类似：

```c++
void * operator new (sieze_t size)
{
    void * mem = malloc(size)
    if(mem!=NULL)
        return mem;
    else
        throw bad_alloc();
}
void operator delete(void *mem) noexcept
{
    free(mem);
}
```

#### 8.是否可以被重载(new operator不可以重载，但operator new()可以，就是负责内存分配的那个函数，返回void*)

new operator 是 C++ 保留关键字，我们无法改变其含义，但我们可以改变new完成它功能时调用的两个函数，operator new() 和 placement new()。

opeartor new /operator delete可以被重载。标准库是定义了operator new函数和operator delete函数的8个重载版本：

```c++
//这些版本可能抛出异常
void * operator new(size_t);//分配某大小的内存，返回void*，这是new operator的第一步
void * operator new[](size_t);
void * operator delete (void * )noexcept;
void * operator delete[](void *0)noexcept;
//这些版本承诺不抛出异常
void * operator new(size_t ,nothrow_t&) noexcept;
void * operator new[](size_t, nothrow_t& );
void * operator delete (void *,nothrow_t& )noexcept;
void * operator delete[](void *0,nothrow_t&)noexcept;
```

我们可以自定义上面函数版本中的任意一个，前提是自定义版本必须位于全局作用域或者类作用域中。太细节的东西不在这里讲述，总之，我们知道我们有足够的自由去重载operator new /operator delete ,以决定我们的new与delete如何为对象分配内存，如何回收对象。

而malloc/free并**不允许重载**。

#### 9. 能够直观地重新分配内存（malloc分配后的内存，如果后来觉得内存不足，可以realloc函数进行内存重新分配实现内存的扩充。new不行）

使用malloc分配的内存后，如果在使用过程中发现内存不足，可以使用realloc函数进行内存重新分配实现内存的扩充。realloc先判断当前的指针所指内存是否有足够的连续空间，如果有，原地扩大可分配的内存地址，并且返回原来的地址指针；如果空间不够，先按照新指定的大小分配空间，将原有数据从头到尾拷贝到新分配的内存区域，而后释放原来的内存区域。

new没有这样直观的配套设施来扩充内存。

#### 10.客户处理内存分配不足（分配内存时内存不足时，在抛出异常之前，对于new，客户能够指定处理函数或重新制定分配器，而malloc内存不足就只会呆呆地返回NULL，无法通过用户代码进行处理）

在operator new抛出异常以反映一个未获得满足的需求之前，它会先调用一个用户指定的错误处理函数，这就是**new-handler**。new_handler是一个指针类型：

```c++
namespace std
{
    typedef void (*new_handler)();
}
```

指向了一个没有参数没有返回值的函数,即为错误处理函数。为了指定错误处理函数，客户需要调用set_new_handler，这是一个声明于的一个标准库函数:

```c++
namespace std
{
    new_handler set_new_handler(new_handler p ) throw();
}
```

set_new_handler的参数为new_handler指针，指向了operator new 无法分配足够内存时该调用的函数。其返回值也是个指针，指向set_new_handler被调用前正在执行（但马上就要发生替换）的那个new_handler函数。

对于malloc，客户并不能够去编程决定内存不足以分配时要干什么事，只能看着malloc返回NULL。

#### 总结

| 特征                                   | new/delete                                                   | malloc/free                          |
| -------------------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| 分配内存的位置                         | 自由存储区（抽象概念，可以是堆、静态区等等）                 | 堆                                   |
| 函数的返回值                           | 完整类型指针（指new operator）                               | void*                                |
| 内存分配失败返回值                     | 默认抛出异常（这其实就是operator new()与malloc的区别）       | 返回NULL                             |
| 分配内存时内存不足，<br />抛出异常之前 | 客户能够指定处理函数或重新制定分配器                         | 无法通过用户代码进行处理             |
| 分配内存的大小                         | 由编译器根据类型计算得出                                     | 必须显式指定字节数                   |
| 处理数组                               | 有处理数组的new版本new[]                                     | 需要用户计算数组的大小后进行内存分配 |
| 已分配内存的扩充                       | 无法直观地处理                                               | 使用realloc简单完成                  |
| 是否相互调用                           | 可以，看具体的operator new/delete实现（operator new()可以调用malloc） | 不可调用new                          |
| 函数重载                               | 允许(operator new()允许)                                     | 不允许                               |
| 构造函数与析构函数                     | 调用(new operator调用)                                       | 不调用                               |

### 20.C++ new的三种形态

#### 1.new operator（new运算符，包含三个步骤）

new的第一种形态是new operator，它是语言内建的， 是C++ 保留关键字，不能重载。new operator完成以下三件工作：

```c++
1. allocate memory for this object.//1.为对象分配内存。类似于malloc的工作，其实是由operator new完成,分配内存，返回void*，但是一定要区别于new operator

2. call constructor to init that memory.//2.调用构造函数来初始化这块儿内存。通过placement new完成

3. return the pointer of this object.//3.返回指向该对象的指针
```

例如：`string *pStr = new string(“Memory Management”);`

它实际完成以下三件事：

```c++
// 1. 为string对象分配raw内存

void *memroy = operator new( sizeof(string) );//operator new就类似于malloc，分配内存，返回void*，但是一定要区别于new operator

// 2. 调用构造函数初始化内存中的对象

call string::string() on memory

// 3. 获得对象指针

string *pStr = static_cast<string *>(memory);
```

第1步申请内存，通过operator new完成；第2步在指定的内存上调用构造函数初始化对象，通过placement new完成。这便是new的另外两种形态。

#### 2.operator new()（operator new()函数，仅仅是分配内存而已，也是输入需要的内存byte，然后返回void *。类似于malloc，区别就在于分配失败时异常还是返回NULL）

operator new是普通操作符，和加减乘除操作符的地位一样，可以重载。

默认情况下，operator new尝试从堆上申请内存，如果成功则返回内存指针，如果失败会调用new_handler，然后继续重复前面过程，直到抛出异常（bad_alloc）为止。

operator new函数原型：

```c++
void * operator new(size_t size);
```

operator new可以重载，可以为某个类单独重载，也可以全局重载（将改变所有operator new的行为方式）。如果重载了operator new，应该重载operator delete。

它跟malloc的区别就在于，内存分配失败时是产生异常，还是返回NULL

#### 3.placement new（定位new，不分配内存，仅用于定位内存并进行对象的构造）

（定位new）在已分配的原始内存中初始化一个对象。它与new的其他版本的不同之处在于，**它不分配内存**。相反，它接受指向已分配但未构造的内存的指针，并在该内存中初始化一个对象。**placement new表达式能够在特定的、预分配的内存地址上构造一个对象**。

placement new是c++标准库的一部分，使用时需包含`<new>`头文件。

```c++
void *s = operator new( sizeof(A) );
A *p = (A*)s;
new(p) A(2013); // p->A::A(2013);，它其实会返回一个A*的指针
// processing code…
p->~A();
```

如果显示的调用placement new，也应该显示的调用与之对应的placement delete：p->~A();。这份工作本来应该由编译器自动完成：在使用new operator时，编译器会自动生成调用placement new的代码。所以，除非特别必要，不要直接使用placement new。只有当默认的new operator对内存的管理不能满足需要，希望自己手动管理内存时，才考虑使用placement new。就像STL中的allocator一样，它借助placement new来实现更灵活有效的内存管理。

#### 4.综合实例

```c++
#include <iostream>
#include<stdio.h>
#include <new>//需要添加该头文件
using namespace std;
char test[1024];//预分配静态区内存
class A {
public:
    explicit A(int i = 0):temp(i) { cout << "构造" << endl; };
    void func() { cout << temp << endl; }
    ~A() { cout << "析构" << endl; }
private:
    int temp;
};
int main()
{
    void* a = new A(1);//new operator
    ((A*)a)->func();//使用c风格的强制转换其实不合适，应该用static_cast。
	// static_cast<A*>(a)->func();
    A* b = new A(2);//new oprtator
    b->func();

    void* c = operator new(sizeof(A));//oprator new()分配内存
    new(c)A(3);//定位new进行初始化，使用预分配的堆内存
    ((A*)c)->func();

    new(test)A(4);////定位new进行初始化，使用静态区内存
    ((A*)test)->func();

    delete (A*)a;
    delete b;
    delete (A*)c;
    ((A*)test)->~A();//使用静态区的，手动析构，或者等系统自己收回也行
    return 0;
}
/*
构造
1
构造
2
构造
3
构造
4
析构
析构
析构
析构
*/
```

### 21.C++ 中的RTTI机制详解（运行时类型信息，与typeid、dynamic_cast相关，多态的实现是靠虚函数表，其实虚表里就有RTTI机制，因为虚函数表的第一个位置存放了指向type_info的指针，它包含了运行时类型信息）

#### 1.RTTI概念

RTTI是”Runtime Type Information”的缩写，意思是**运行时类型信息，它提供了运行时确定对象类型的方法**。即通过运行时类型识别，程序能够使用基类的指针或引用来检查着这些指针或引用所指的对象的实际派生类型。

#### 2.RTTI机制的产生

C++是一种静态类型语言。其数据类型是在编译期就确定的，不能在运行时更改。然而由于面向对象程序设计中多态性的要求，C++中的指针或引用(Reference)本身的类型，可能与它实际代表(指向或引用)的类型并不一致。有时我们需要将一个多态指针转换为其实际指向对象的类型，就需要知道运行时的类型信息，这就产生了运行时类型识别的要求。**和Java相比，C++要想获得运行时类型信息，只能通过RTTI机制**，并且C++最终生成的代码是直接与机器相关的。

（Java中任何一个类都可以通过反射机制来获取类的基本信息（接口、父类、方法、属性、Annotation等），而且Java中还提供了一个关键字，可以在运行时判断一个类是不是另一个类的子类或者是该类的对象，Java可以生成字节码文件，再由JVM（Java虚拟机）加载运行，字节码文件中可以含有类的信息。）

#### 3.typeid函数

- typeid的主要作用就是让用户知道当前的变量是什么类型的，对于内置数据类型以及自定义数据类型都生效，比如以下代码：

  - ```c++
    int main()
    {
        short s = 2;
        unsigned ui = 10;
        int i = 10;
        char ch = 'a';
        wchar_t wch = L'b';
        float f = 1.0f;
        double d = 2;
    
        cout << typeid(s).name() << endl; // short
        cout << typeid(ui).name() << endl; // unsigned int
        cout << typeid(i).name() << endl; // int
        cout << typeid(ch).name() << endl; // char
        cout << typeid(wch).name() << endl; // wchar_t
        cout << typeid(f).name() << endl; // float
        cout << typeid(d).name() << endl; // double
    
        return 0;
    }
    ```

- 对于我们自定义的结构体、类，tpyeid都能支持
  - ```c++
    class A
    {
    public:
         void Print() { cout<<"This is class A."<<endl; }
    };
      
    class B : public A
    {
    public:
         void Print() { cout<<"This is class B."<<endl; }
    };
      
    struct C
    {
         void Print() { cout<<"This is struct C."<<endl; }
    };
      
    int main()
    {
         A *pA1 = new A();
         A a2;
      
         cout<<typeid(pA1).name()<<endl; // class A *
         cout<<typeid(a2).name()<<endl; // class A
      
         B *pB1 = new B();
         cout<<typeid(pB1).name()<<endl; // class B *
      
         C *pC1 = new C();
         C c2;
      
         cout<<typeid(pC1).name()<<endl; // struct C *
         cout<<typeid(c2).name()<<endl; // struct C
      
         return 0;
    }
    ```

- **实质上typeid函数是一个返回类型为type_info类型**的函数。而name()只是一个type_info类里的一个成员函数。

- **type_info类：**

- 在type_info类中，复制构造函数和赋值运算符都是私有的，同时也没有默认的构造函数；所以，我们没有办法创建type_info类的变量，例如type_info A;这样是错误的。

  - ```c++
    class type_info
    {
    public:
        virtual ~type_info();
        bool operator==(const type_info& _Rhs) const; // 用于比较两个对象的类型是否相等
        bool operator!=(const type_info& _Rhs) const; // 用于比较两个对象的类型是否不相等
        bool before(const type_info& _Rhs) const;
      
        // 返回对象的类型名字，这个函数用的很多
        const char* name(__type_info_node* __ptype_info_node = &__type_info_root_node) const;
        const char* raw_name() const;
    private:
        void *_M_data;
        char _M_d_name[1];
        type_info(const type_info& _Rhs);
        type_info& operator=(const type_info& _Rhs);
        static const char * _Name_base(const type_info *,__type_info_node* __ptype_info_node);
        static void _Type_info_dtor(type_info *);
    };
    ```

- 观察以下两个例子，体会RTTI

  - ```c++
    //例一
    #include <iostream>
    #include <typeinfo>
    using namespace std;
      
    class A
    {
    public:
         void Print() { cout<<"This is class A."<<endl; }
    };
      
    class B : public A
    {
    public:
         void Print() { cout<<"This is class B."<<endl; }
    };
      
    int main()
    {
         A *pA = new B();
         cout<<typeid(pA).name()<<endl; // class A *
         cout<<typeid(*pA).name()<<endl; // class A
         return 0;
    }
    ```

  - ```c++
    //例二
    #include <iostream>
    #include <typeinfo>
    using namespace std;
      
    class A
    {
    public:
         virtual void Print() { cout<<"This is class A."<<endl; }//变成了虚函数
    };
      
    class B : public A
    {
    public:
         void Print() { cout<<"This is class B."<<endl; }
    };
      
    int main()
    {
         A *pA = new B();
         cout<<typeid(pA).name()<<endl; // class A *
         cout<<typeid(*pA).name()<<endl; // class B
         return 0;
    }
    ```

  - pA是一个`A*`的指针，这个毋庸置疑。但是基类指针pA指向的对象其实都是派生类B，一个`*pA`被认定为class A，一个`*pA`被认定为class B。

  - 这就是RTTI在捣鬼了，**当类中不存在虚函数时，typeid是编译时期的事情，也就是静态类型，就如上面的cout<<typeid(\*pA).name()<<endl;输出class A一样；当类中存在虚函数时，typeid是运行时期的事情，也就是动态类型，就如上面的cout<<typeid(\*pA).name()<<endl;输出class B一样**，关于这一点，我们在实际编程中，经常会出错，一定要谨记。

  - 这就类似于，虚函数，运行时绑定。

- 使用type_info类中重载的==和!=比较两个对象的类型是否相等

  - ```c++
    class type_info
    {
    public:
        virtual ~type_info();
        bool operator==(const type_info& _Rhs) const; // 用于比较两个对象的类型是否相等
        bool operator!=(const type_info& _Rhs) const; // 用于比较两个对象的类型是否不相等
    private:
        ...
    }
    ```

  - 这个会经常用到，通常用于比较两个带有虚函数的类的对象是否相等，例如以下代码： 

  - ```c++
    class A
    {
    public:
         virtual void Print() { cout<<"This is class A."<<endl; }
    };
      
    class B : public A
    {
    public:
         void Print() { cout<<"This is class B."<<endl; }
    };
      
    class C : public A
    {
    public:
         void Print() { cout<<"This is class C."<<endl; }
    };
      
    void Handle(A *a)
    {
         if (typeid(*a) == typeid(A))//typeid(*a)和typeid(A)都是一个type_info对象，也就是调用了type_info类中重载的 bool operator==(const type_info& _Rhs) const; 
         {
              cout<<"I am a A truly."<<endl;
         }
         else if (typeid(*a) == typeid(B))
         {
              cout<<"I am a B truly."<<endl;
         }
         else if (typeid(*a) == typeid(C))
         {
              cout<<"I am a C truly."<<endl;
         }
         else
         {
              cout<<"I am alone."<<endl;
         }
    }
      
    int main()
    {
         A *pA = new B();
         Handle(pA);//I am a B truly.
         delete pA;
         pA = new C();
         Handle(pA);//I am a C truly.
         return 0;
    }
    这里输出的结果为：
    I am a B truly.
    I am a C truly.
    ```

#### 4.dynamic_cast的内幕

- 先看个例子

  - ```c++
    #include <iostream>
    #include <typeinfo>
    using namespace std;
      
    class A
    {
    public:
         virtual void Print() { cout<<"This is class A."<<endl; }
    };
      
    class B
    {
    public:
         virtual void Print() { cout<<"This is class B."<<endl; }
    };
      
    class C : public A, public B
    {
    public:
         void Print() { cout<<"This is class C."<<endl; }
    };
      
    int main()
    {
         A *pA = new C;
         //C *pC = pA; // Wrong
         C *pC = dynamic_cast<C *>(pA);
         if (pC != NULL)
         {
              pC->Print();
         }
         delete pA;
    }
    //这里输出为：This is class C
    ```

  - 在上面代码中，如果我们直接将pA赋值给pC，这样编译器就会提示错误，而当我们加上了dynamic_cast之后，一切就ok了。那么dynamic_cast在后面干了什么呢？

  - dynamic_cast主要用于在多态的时候，它允许在运行时刻进行类型转换，从而使程序能够在一个类层次结构中安全地转换类型，**把基类指针（引用）转换为派生类指针（引用）(这叫做安全地下行转换，上行是百分百安全的)**。我在《[COM编程——接口的背后](https://www.jb51.net/article/55881.htm)》这篇博文中总结的那样，**当类中存在虚函数时，编译器就会在类的成员变量中添加一个指向虚函数表的vptr指针**，每一个class所关联的type_info类型的object也经由virtual table被指出来，通常这个type_info object放在表格的第一个slot。当我们进行dynamic_cast时，编译器会帮我们进行语法检查。如果指针的静态类型和目标类型相同，那么就什么事情都不做；否则，首先对指针进行调整，使得它指向vftable，并将其和调整之后的指针、调整的偏移量、静态类型以及目标类型传递给内部函数。其中最后一个参数指明转换的是指针还是引用。**两者唯一的区别是，如果转换失败，前者（指指针）返回NULL，后者（指引用）抛出bad_cast异常**。

  - 总结起来dynamic_cast的原理就是，**虚表中会存放有一个也就是type_info类型的指针，它代表了该虚表到底是哪个类（基类还是派生）的虚表**，通过name()调用可以获取真实类型，也就是说虚表中存放了该指针指向真正的对象类型（其实也就是运行时类型信息获取），用于dynamic_cast下行转换时的比较。(也就是比如`dynamic_cast<C *>`，就把`C*`与那个虚表中真实存放的类型比较一下)。如果一致，那就转化成功了。如果不一致，那就返回nullptr，或者是对于引用就抛出异常

#### 5.简介版回答RTTI

运行时类型检查，在C++层面主要体现在dynamic_cast和typeid,VS中虚函数表的第一个位置存放了指向type_info的指针。对于存在虚函数的类型，typeid和dynamic_cast都会去查询type_info。

- typeid查询type_info类型是为了人工获得该指针的真正指向类型

- dynamic_cast查询type_info类型是是为了编译器获取该指针的真正指向类型（其实也就是运行时类型信息获取），以判断是否能够安全地下行转换。能则转换，不能则返回Nullptr

  - ```c++
    A *pA = new C;
    //C *pC = pA; // Wrong
    C *pC = dynamic_cast<C *>(pA);
    ```

  - **pA是一个基类指针，dynamic_cast的具体操作是，查询pA地址里指向的对象（看代码可以看到其实是个派生类C对象），对象里存着一个虚指针，虚指针指向了派生类C对应的虚表，虚表首项是type_info指针，然后通过该type_info获得真实.name()是C。`dynamic_cast<C *>`欲转化的是`C*`类型，所以可以安全地向下转化。（从基类指针转化成派生类指针）**

### 22.函数调用（栈、栈帧）

**栈**就是CPU寄存器里的**某个指针所指向的一片内存区域**。这里所说的“某个指针”通常位于x86/x64平台的`ESP寄存器`

操作栈的最常见的指令时`PUSH`（压栈）和`POP`（弹栈）。`PUSH`指令会对`ESP`**寄存器的值**进行减法运算，使之**减去4（32位）或8（64位）（这是因为栈的生长方向从高地址到低地址）**，然后将操作数**写到**ESP寄存器里的指针所指向的**内存中**。

#### 1.栈在进程中的作用如下：

1. 暂时保存函数内的局部变量。
2. 调用函数时传递参数。
3. 保存函数返回的地址。

#### 2.栈帧

**栈帧**也叫过程活动记录，是编译器用来实现过程/函数调用的一种数据结构。简言之，**栈帧**就是利用`EBP`（**帧指针EBP**，请注意不是ESP，）寄存器访问局部变量、参数、函数返回地址等的手段。

每一次函数的调用，都会在`调用栈`（call stack）上维护一个独立的`栈帧`（stack frame）。每个独立的栈帧一般包括：

- 函数的返回地址和参数
- 临时变量：包括函数的非静态局部变量以及编译器自动生成的其他临时变量
- 函数调用的上下文

**栈是从高地址向低地址延伸**，一个函数的栈帧用EBP和ESP这两个寄存器来划定范围。

- EBP寄存器又被称为`帧指针`（Frame Pointer），指向**当前栈帧**的底部（高地址）。
- ESP寄存器又被称为`栈指针`（Stack Pointer），**始终指向**栈帧的顶部（低地址）。

![image.png](https://i.loli.net/2020/09/17/pDEqVHBd8YykiIK.png)

#### 3.函数调用的过程

以实例分析：

```c++
void swap(int* a,int* b)
{
    int c;
    c=*a;
    *a=*b;
    *b=c;
}
int main(){
    int a,b;
    a=16;
   	b=32;
    swap(&a,&b);
    return(a-b);
}
```

下面是这个过程对应的栈结构（栈的生长方向是从高地址到低地址）

可以看出，在调用swap函数后，会为更改栈指针esp和帧指针ebp的指向，为swap分配一个栈帧结构。

![image.png](https://i.loli.net/2020/09/17/o1nsrRuEd25FWD8.png)

**调用swap函数过程做了什么？**

- **1、将函数参数压栈**（&b,&a）
  - 从右到左传参，即从右到左入栈
- 2、**将返回地址压栈**
  - 就是调用完swap函数之后，接下来该执行哪个指令或者哪句代码了，即下一句代码（下一条指令）的地址，该地址应该是在代码区
- 3、**将当前帧指针所指的地址（ebp寄存器的值）压栈**
  - 用于函数调用完了，恢复到之前调用者（比如main函数）的栈帧去
- 4、**移动帧指针（修改ebp寄存器）与栈指针（修改esp寄存器），为swap函数创建一个栈帧结构**
  - 由于将参数(&b,&a)压入栈中，所以在swap函数要访问**&a**和**&b**时，可以通过**%(ebp+8)**与**%(ebp+12)**访问。 swap函数结束后，会恢复帧指针与栈指针，然后继续运行。

#### 4.注意

在所有函数之外进行加减乘除运算、使用 if...else 语句、调用一个函数等都是没有意义的，这些代码位于整个函数调用链条之外，永远都不会被执行到。C语言也禁止出现这种情况，会报语法错误，请看下面的代码：

```c++
#include <stdio.h>
int a = 10, b = 20, c;

//错误：不能出现加减乘除运算
c = a + b;

//错误：不能出现对其他函数的调用
printf("c.biancheng.net");

int main(){
    return 0;
}
```

#### 5.牛客网的答案

- 每一个函数调用都会分配函数栈帧，在栈内进行函数执行过程。
- 调用前，先把返回地址压栈，然后把当前栈帧的ebp指针压栈。
- 然后移动帧指针（修改ebp寄存器）与栈指针（修改esp寄存器），为函数创建新的栈帧

### 23.C语言函数传参的压栈顺序？

从右到左

### 24.左值右值？（具体例子见25.右值引用里的例子）

- 左值是存储单元内的值，即是有实际存储地址的；
- 右值则不是存储单元内的值，比如它可能是寄存器内的值也可能是立即数。
- 左值就是一切有存放位置的值（内存，寄存器）。右值就是一切没有存放位置的值，现在又多出一种xvalue，代表一切有临时存放位置但马上要销毁的值（返回值等）。

https://zhuanlan.zhihu.com/p/111826434

https://www.cnblogs.com/Philip-Tell-Truth/p/6370019.html

#### 1.概念(左值是存储单元内的值，即是有实际存储地址的；右值就是一切没有存放位置的值;xvalue，代表一切有临时存放位置但马上要销毁的值（返回值等）)

每一个C++表达式(带有运算对象的运算符、字面值[literal]、变量名等)都有两个独立的属性———— 型别[type]和值类型[value categories，就是底下那5种]。

可以通过两个方面来对值类型进行分类：

- 1.有"身份"[has identity]：能够确定某个表达式是否和另一个表达式指涉[refers to]同一个实体，例如，通过比较它们标识[identify]出来的函数或者对象的地址(直接或间接得到的)。也可以说叫做具名
- 2.能被移动[can be moved from]：能够被移动构造函数、移动赋值操作符或者其它实现[implement]移动语义[move semantics]的重载函数绑定[bind to]。



- 1) 有"身份"但是不能"被移动"的表达式被称为左值表达式[lvalue expression];
- 2) 有"身份"同时能"被移动"的表达式被称为x值表达式[xvalue expression];
- 3) 没有"身份"但是能"被移动"的表达式被称为纯右值表达式[prvalue expression];
- 4) C++没有既没有"身份"也不能"被移动"的表达式；









![image.png](https://i.loli.net/2020/09/18/7EsaV4R9WHSuPLU.png)

- glvalue：泛左值，可能是lvalue或者xvalue

- lvalue：传统意义上的左值，（历史上称为左值是因为左值可以出现在赋值表达式的左侧），指一个函数或者对象

  - 作用域中变量或函数的名称，与类型无关，例如：std::cin，std::endl.

  - 运算符重载并且返回值是引用的方法，例如：std::getline(std::cin, str), std::cout << 1, str1 = str2, or ++it; 

  - 内建类型和组合类型是左值，例如：a = b, a += b, a %= b

  - 前自增或者前自减是左值，可以取地址，即&++a。（因为前置++的返回值是引用，后置++的返回值是非引用）

  - 内建指针类型

  - C++规定字符串是左值，such as "Hello, world!";

  - **注意等号左边的值不一定是左值，这个不是绝对的**

  - 简单的来说，能取地址的变量一定是左值，有名字的变量也一定是左值，最经典的void fun(p&& shit)，其中shit也是左值，因为右值引用是左值（所以才会有move，forward这些函数的产生，其中move出来一定是右值，forward保持变量形式和之前的不变，就是为了解决右值引用是左值的问题）。至于为什么不能把等号左边看成左值，因为在C++中，等号是可以运算符重载的，等号完全可以重载成为等号左边为右值的形式。

  - **有名字的一定是左值，哪怕你声明他的时候用的是右值引用**

    ```cpp
    void Fuck(Shit&& shit) // shit在函数体里面仍然是左值，类型是Shit&
    ```

- xvalue：消亡值，通过右值引用产生

  - 本质上，消亡值就是通过右值引用产生的值。右值一定会在表达式结束后被销毁，比如return x（x被copy以后会被销毁）, 1+2（3这个中间值会被销毁）。

- rvalue：传统意义上的右值

- prvalue：纯右值

  - 字面值常量，除了字符串，都是纯右值，包括空指针，true和false，例如：42, true or nullptr; 
  - 返回值是非引用的表达式是纯右值，such as str.substr(1, 2), str1 + str2, or it++; （后置++ --的返回值是非引用）
  - a + b, a % b, a & b, a << b，和其他的内置数学运算
  - a && b, a || b, ~a，内置逻辑运算
  - a < b, a == b, a >= b，内置比较运算
  - 取地址表达式是一个纯右值，因为地址本身是纯右值，&a
  - this指针也是纯右值，因为this也是一个地址
  - lambda表达式是纯右值
  - 纯右值是传统右值的一部分，纯右值是表达式产生的中间值，不能取地址。

```c++
	i - has identity 具名
	m - is movable 可移动
    	
    glvalue  - i          具名    
	lvalue   - i & !m     具名、不可移动
   	xvalue   - i & m      具名、可移动
    rvalue   - m          可移动
	prvalue  - !i & m     不具名、可移动


	int x;
	std::move(x);  // 把x转为xvalue，具名且可移动。
```

#### 2.实例

##### 1.lvalue - 可被操作（如取地址、赋值）的“实体”，如定义好的变量

```c++
int x = 666;   // ok
```

##### 2.rvalue - 不可被操作（如取地址、赋值）的“值”，如字符常量、临时变量

```c++
int y;
666 = y; // error!
```

##### 3.函数返回值可以是左值或右值

```c++
int setValue()
{
    int a=1;
    return a;//返回右值
}
setValue() = 3; // error!

int global = 100;
int& setGlobal()
{
    return global;//返回左值    
}
setGlobal() = 400; // OK
```

##### 4. 令lvalue= rvalue很常见 [lvalue-to-rvalue conversion]

```c++
int x = 1;		//ok
int y = 3;		//ok
int z = x + y;   // ok
```

##### 5.令rvalue=lvalue不允许 [rvalue-to-lvalue conversion is no allowed]

```c++
int x=1;
int y=2;
(x+y)=10;//error!
```

##### 6. lvalue reference很常见

```c++
int a = 1;
int& refa = a;
refa = 20; // now a is 20
```

##### 7. rvalue to lvalue reference不允许

```c++
int f(int& x){ 
return x; // x is a reference to lvalue. 
}
int a = 10;
f(a);  // ok
f(10); // error! rvalue convert to lvalue reference is not allowed!
```

##### 8.rvalue to const lvalue reference可以用（很常见）

```c++
int f(const int &x) {
return x;
}
int a = 10;
f(a);  // ok
f(10); // ok now. a rvalue-to-const-lvalue-reference has happened.
```

##### 9.现在我们有了一个图。越往上可修改性越弱。下一级可以转为上一级。

```c++
不可修改    const-lvalue-reference <------ rvalue
             ↑
可修改      lvalue <--- lvalue-reference
```

##### 10.C++OX后，引入rvalue-reference(右值引用)使得上图发生变化、右下角的空白被补全了。下面这段代码背后，临时变量所在的内存单元被固化下来了，可供程序后续使用。

```c++
SomeType&&  rref = 10;  // make temporary variable can be used.
rref = 12; // ok
```

```c++
不可修改    const-lvalue-reference <-------------- rvalue
              ↑                                     ↓
可修改      lvalue <--- lvalue-reference           rvalue-reference
```

##### 11. lvalue ---> rvalue 现在左值也可以转为右值了，便是通过`std::move`.

```c++
int i;
std::move(i),int;
```

#### 3.简单判别方法

##### 1.方法1（能取地址一定是左值，但是有的左值不可以取地址）

C++11开始，表达式一般分为三类：左值（lvalue）、消亡值（xvalue）、纯右值（prvalue），其中左值和消亡值统称泛左值（glvalue），消亡值和纯右值统称右值（rvalue）。

判断表达式是否是左值，有一个简单的办法，就是**看看能否取它的地址**，能取地址的就是左值。(除了类内初始化的static const member是左值，但是不可取地址 )。能取地址一定是左值，但是有的左值不可以取地址，充分不必要。

- 左值可看作是**“对象”**，右值可看作是**“值”** (Lvalues represent objects and rvalues represent values)
- 左值到右值的转换可看做**“读出对象的值”** (Lvalue-to-rvalue conversion represents reading the value of an object)
- std::move 允许把任何表达式以“值”的方式处理 (allows you to treat any expression as though it represents a value)
  - std::move（）实现移动语义，将传入参数全部无条件转换为右值引用。它其实就是一个static_cast，具体是否移动了对象，要看是否发生了移动构造或者移动赋值
- std::forward 允许在处理的同时，保留表达式为“对象”还是“值”的特性 (allows you to preserve whether an expression represented an object or a value)
  - std::forward() 实现了参数在传递过程中保持其值属性的功能，即若是左值，则传递之后仍然是左值，若是右值，则传递之后仍然是右值。

那么这里的“对象” (object) 和 “值” (value) 是什么意思呢？任何一个有价值的 C++ 程序都是如此：a) 反复地操作各种类型的对象 b) 这些对象在运行时创建在明确的内存范围内 c) 这些对象内存储着值。 (Every useful C++ program revolves around the manipulation of objects, which are regions of memory created at runtime in which we store values.)

这一句话的解释实际上就指向了上面的第一条方法——只有特定的内存区域才可以被取地址。

作者：顾露
链接：https://www.zhihu.com/question/39846131/answer/85277628
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

##### 2.方法2（有名字的一定是左值，哪怕你声明他的时候用的是右值引用；所有临时变量都是右值，譬如说表达式里面的函数返回值）

有名字的一定是左值，哪怕你声明他的时候用的是右值引用

```cpp
void Fuck(Shit&& shit) // shit在函数体里面仍然是左值，类型是Shit&
```



左值引用的右值引用，或者右值引用的左值引用，都是左值引用。这一条是专门为了模板元编程发明的。

所有临时变量都是右值，譬如说表达式里面的函数返回值。

左值引用不能传递到需要右值引用的地方。除非你强行使用`std::move<T>(x)`，不过你看一下代码就知道其实里面就一个强制类型转换。注意move和forward的区别。move永远把东西变成右值，而forward则保持不变——这是为了解决“shit在函数体里面仍然是左值”的问题。

右值引用和返回值优化经常被很多人混在一起，而且最要命的是如果开了优化的话，其实多半输出的代码是一样的。



作者：vczh
链接：https://www.zhihu.com/question/39846131/answer/83386143
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 4.std::move()和std::forward()

#### (强调一下，为啥有时候你move完了，发现对象还有，并没有移动。因为move只是一个强制类型转换，真正实现移动的是移动构造函数和移动赋值函数。你要具体再想想移动构造函数到底有没有实现，或者说移动构造函数里做了什么。)

std::move() 与 std::forward()本质上就是一个转换函数，在**编译期**对传入参数进行类型转换

- std::move() 实现移动语义，将传入参数全部无条件转换为右值引用
- std::forward() 实现完美转发，传入左值即返回左值引用，传入右值即返回右值引用

当参数都为右值时，二者等价。

https://blog.csdn.net/qq_29983883/article/details/107238389

##### 1.std::move()实现移动语义，将传入参数全部无条件转换为右值引用

```c++
//std::move()的作用实例    
void func(int &&args) {//右值引用

      std::cout << args << std::endl;

  }

  int a = 10;

  func(20);    // ok

  func(a);      // error, 右值引用不能绑定左值

  func(std::move(a));    // ok

```

```c++
//std::move()的作用实例 
void func(string&& arg) {
    cout << arg << endl;
}
int main() {

    string str = "asdf";
    func(move(str));
    cout << str << endl;
    return 0;
}
//asdf
//asdf
//没有发生移动构造，所以str的内容还保留了
```

```c++
//std::move()的作用实例 
void func(string arg) {
    cout << arg << endl;
}
int main() {

    string str = "asdf";
    func(move(str));
    cout << str << endl;
    return 0;
}
//asdf
//
//发生移动构造，所以str的内容被转移走了
```



```c++
//std::move()的源码剖析
// FUNCTION TEMPLATE move
template <class _Ty>
_NODISCARD constexpr remove_reference_t<_Ty>&& move(_Ty&& _Arg) noexcept { // forward _Arg as movable
    return static_cast<remove_reference_t<_Ty>&&>(_Arg);
}

//std::move() 只是一个转换函数，并没有move任何东西，只是在编译期将传入参数转换为右值引用，在运行期不执行任何操作。
//remove_reference_t<_Ty> 会将传入类型 _Ty 中引用属性去除，然后利用 static_cast() 将 _Ty 转换为右值引用。
```

 **为什么可以无论什么情况，move函数都可以转化成右值引用？因为引用折叠的规则。**

首先，函数参数_Ty&&是一个指向模板类型参数的右值引用（**这其实叫universal reference，不是右值引用**），通过引用折叠，此参数可以与任何类型的实参匹配（可以传递左值或右值，这是std::move主要使用的两种场景)。关于引用折叠如下：

- X& & ==X & && == X&& & == X&，用于处理左值

  - ```c++
    template<typename T> void f(T&&);
    int i;
    f(i);
    //这样编译器推断T为int &类型
    ```

  - ```c++
    string s("hello");
    std::move(s) 编译器推断=>形参T为string& ，所以std::move(string& &&) => 折叠后 std::move(string& )
    此时：T的类型为string&
    typename remove_reference<T>::type为string 
    整个std::move被实例化如下
    string&& move(string& t) //t为左值，移动后不能在使用t
    {
        //通过static_cast将string&强制转换为string&&
        return static_cast<string&&>(t); 
    }
    ```

- X&& && == X&&，用于处理右值

  - ```c++
    std::move(string("hello")) 编译器推断=>T是string&&类型， std::move(string&&)
    //此时：T的类型为string 
    //     remove_reference<T>::type为string 
    //整个std::move被实例如下
    string&& move(string&& t) //t为右值
    {
        return static_cast<string&&>(t);  //返回一个右值引用
    }
    ```

有了这两个引用折叠的规则，move就可以接受任何类型的实参，从而获得一个右值引用。

**注意：**

- std::move只是将参数转换为右值引用而已,它从来不会移动什么.真正的移动操作是在移动构造函数或者移动赋值操作符中发生的:

  - 

  ```c++
  T (T&& rhs);
  T& operator= (T&& rhs);
  ```

- std::move的作用只是为了让调用构造函数的时候告诉编译器去选择移动构造函数.

  - 

  ```c++
  int main() {
  
      string str = "asdf";
      string& lr = str;//左值引用，它跟str是一码事，两个名字而已
      string&& rr = std::move(str);//rr是右值引用，但它其实还是个左值，因为它有名字。与普通的左值引用lr没差别
  
      rr[0] = 'b';
      lr[1] = 'z';
      cout << rr << '\t' << lr << '\t' << str << endl;
      cout << &str << '\t' << &lr << '\t' << &rr << endl;
  
      return 0;
  }
  //bzdf    bzdf    bzdf
  //0103FE20        0103FE20        0103FE20
  //可以看出，str、lr、rr三个东西地址都完全一样，就是一个东西
  ```

  - ```c++
    int main() {
    
        string str = "asdf";
        string& lr = str;//左值引用，它跟str是一码事，两个名字而已
        string rr = std::move(str);//move(str)的返回值是一个string&&，所以将调用string类的移动构造函数去构造rr，那么str和lr的对象就被移动走了，str和lr变为了空。rr是一个全新的对象。
    
        //rr[0] = 'b';
        //lr[1] = 'z';
        cout << str << '\t' << lr << '\t' << rr << endl;
        cout << &str << '\t' << &lr << '\t' << &rr << endl;
    
        return 0;
    }
    //                asdf                        可以看到，str和lr没有输出，因为内容被移动构造给rr了
    //00CFFE2C        00CFFE2C        00CFFDFC    rr跟他们两个的地址不同
    ```

**总结：**

- move()的作用，就是一个static<>强制转换成右值引用！
- 它可以把所有类型（不管你原来是个左值、右值）都转换成右值引用的原理是，由于引用折叠规则！
- move()函数调用完，内容是否保留了，这要取决于是否发生了移动构造！
- move本身只是将参数转换为右值引用而已,它从来不会移动什么。真正的移动操作是在移动构造函数或者移动赋值操作符中发生的。

##### 2.std::forward()实现了完美转发

std::forward() 实现了参数在传递过程中保持其值属性的功能，即若是左值，则传递之后仍然是左值，若是右值，则传递之后仍然是右值。

```c++
    void func(int &arg) {//左值引用
       std::cout << "func lvalue" << std::endl;
   }

   void func(int &&arg) {//右值引用
       std::cout << "func rvalue" << std::endl;
   }

   template <typename T>
   void wrapper(T &&args) {//universal reference，需要参照引用叠加规则
       func(args);
   }

   int main() {
       int a = 10; 

       wrapper(a);//a是int型，void wrapper(T &&args)将自动推断T为int& 所以args是int& &&=int&
       wrapper(20);//20是一个右值，void wrapper(T &&args)将自动推断T为int，所以args是int&&，是右值调用。但是在wrapper()函数内部，无论如何，args都是一个左值，在调用func()函数的时候，调用的func()原型总是func(int &)。

       return 0;
   }
//通过引用叠加，分析以上示例可知，调用wrapper()时，wrapper(a) 是调用的原型是 wrapper(int &)，是左值调用；wrapper(20)调用的原型是wrapper(int &&)，是右值调用。但是在wrapper()函数内部，无论如何，args都是一个左值，在调用func()函数的时候，调用的func()原型总是func(int &)。
//func lvalue
//func lvalue
```

因此，根本原因其实是，右值引用属性不能被转发。所以，C++11提供了std::forward()函数用于完美转发。即，在转发过程中，左值引用在被转发之后仍然保持左值属性，右值引用在被转发之后依然保持右值属性。修改之后的代码如下：

```c++
 void func(int &arg) {
        std::cout << "func lvalue" << std::endl;
    }

    void func(int &&arg) {
        std::cout << "func rvalue" << std::endl;
    }

    template <typename T>
    void wrapper(T &&args) {
        func(std::forward<T>(args));//forward<T>(args)用于保留args的左右值属性，要不然args一定永远是左值
    }

    int main() {
        int a = 10; 

        wrapper(a);
        wrapper(20);

        return 0;
    }
//通过引用叠加，分析以上示例可知，调用wrapper()时，wrapper(a) 是调用的原型是 wrapper(int &)，是左值调用；wrapper(20)调用的原型是wrapper(int &&)，是右值调用。现在在wrapper()函数内部，args保留了原有的左右值属性，在调用func()函数的时候,wrapper(20)时，std::forward<T>(args)结果是一个int类型的右值。
//    func lvalue
//    func rvalue
```



#### 5.一定要注意，带模板的 T&&是universal reference，它有引用叠加规则。（不可以有const-volatile限定符）

- X& & ==X & && == X&& & == X&，用于处理左值
- X&& && == X&&，用于处理右值

```c++
template <typename T>
void f(T&& para) {
	// do something;
}
int x;
int &&a = 10; //a的类型是int&&，右值引用，但a是一个左值。(int&&a=10，与int a=10，好像差别不大，因为这样写了以后a都是左值)
int &b  = x;  //b的类型是int&，左值引用 
f(10); // 10是右值，para的类型是右值引用，T是10的原生类型也就是int,函数f会实例化为 f(int&& para)
//你也可以理解为，T被推断为int&&，所以f(int&& &&para)=f(int&& para)。其实应该是上边那种理解
f(x);  // x是左值，para的类型是左值引用，T是左值引用，即T被编译器推断为int&，函数f会实例化为 f(int& && para)=f(int& para)
f(a);  // a是右值引用，本身是左值，T是左值引用，即T被编译器推断为int&，函数f会实例化为 f(int& && para)=f(int& para)
f(b);  // b是左值引用，本身是左值，T是左值引用，即T被编译器推断为int&，函数f会实例化为 f(int& && para)=f(int& para)
```

结论就是，void f(T&& para) 可以接受所有类型参数？

- 当universal reference被lvalue初始化时，universal reference最终是左值引用.
- 当universal reference被rvalue初始化时，universal reference最终是右值引用.

### 25.左值引用、const引用、右值引用

具体地，配合修饰符（const 和 volatile），C++ 中最常用的 3 种组合是：`&`（左值引用）, `const&`（const 引用） 和 `&&`（右值引用）。为什么要把引用和修饰符放在一起讨论呢？这是因为他们都属于编译时的上下文信息。除此之外合法的组合还有 5 种，分别是 `const&&`, `volatile&`, `volatile&&`, `const volatile&` 以及 `const volatile&&`，这些组合基本没有应用价值，这里不再赘述。下面我们简单讨论一下 `&`, `const&` 和 `&&` 的使用场景。

![image.png](https://i.loli.net/2020/09/20/sVLYIwulOJZeHcj.png)

#### 1.左值引用(用做函数的参数也不是不行，不推荐而已；用于函数内部返回的堆对象的引用)

**左值引用不推荐用于函数参数**，是因为左值引用与普通的拷贝传值，在函数调用上区分不开，容易写错

```c++
bool do_something(int arg0, double arg1, result_t& result);

int arg0 = 1;
double arg1 = 2.34;
result_t my_result;
bool ok = do_something(1, 2.34, my_result);
//这里三个参数传参方式一模一样，但对于前两个参数 do_something 使用的是一个拷贝，不会修改调用方 arg0 和 arg1 的值，但第三个参数确实很有可能要修改的，这样就可能产生歧义
```

**左值引用经常用于函数返回值**，比如返回在函数内部堆上分配的对象的引用（返回栈上的引用是大错特错的）

#### 2.const引用（一般用做函数的参数，即变量只读）

与 `&` 相反，`const&` 作为函数形参是很常见的，因为其具有“只读”语义，**也就是说其实参类型甚至可以是一个纯右值**，这一点是指针做不到的。由于其无副作用且对于用户来说使用成本最低，很多**非**模板库都使用 `const&` 传递上下文只读参数。

- ```c++
  void func( int& temp) {
      cout << temp << endl;
  }
  int main() {
  
      int a = 10;
      int&& b=20;//b也是个左值，与int b=20无差
      func(a);
      func(b);
      func(100);//错误！这就像int& c=1000;错误一样，非常量引用的初始值必须是左值。或者说右值无法转换成左值引用
      
      return 0;
  }
  ```

- ```c++
  void func(const int& temp) {
      cout << temp << endl;
  }
  int main() {
  
      int a = 10;
      int&& b=20;//b也是个左值，与int b=20无差
      func(a);
      func(b);
      func(100);//正确！
      
      return 0;
  }
  ```

#### 3.右值引用（一般只用作移动构造或者移动赋值函数的参数，以及“中间层”的函数模板参数。）

- 右值引用一般只用作移动构造或者动赋值函数的参数，以及“中间层”的函数模板参数

  - 中间层就是说，善用 `std::forward()` ，保留传入参数其值属性，然后完美转发

- 右值引用一般不作为任何函数的返回值（当然`std::move()`, `std::forward()` 等特殊函数除外，move()一定是把右值引用作为返回值，forward看情况，左值引用在被转发之后仍然保持左值属性，右值引用在被转发之后依然保持右值属性，也就是返回值可能是右值引用也可能是左值引用）

- 在什么情况下我们“真的需要右值引用”呢？

  - 最常见的就是用做移动构造函数和移动赋值函数中的函数参数

    - 比如一个 `std::vector` 对象生命周期即将结束，但我们希望保存其中的所有元素到一个新的 `std::vector` 对象，相比起“拷贝数组中所有元素”我们不如直接把原来 `std::vector` 对象中保存的数组指针“偷”到新的对象中去，这样我们就免去了复制整个数组的“代价”。有些情况下“没有拷贝选项”的设计是考虑到资源所有权的问题，例如一个经典设计 `std::unique_ptr`，它不支持拷贝是为了保证一个堆对象指针的所有者只能有一个，以避免程序员使用指针时的经典车祸现场。

  - 另一种右值引用的用法就是用做中间层的函数模板参数，善用 `std::forward()` 即可

    - 

    ```c++
     void func(int &arg) {
            std::cout << "func lvalue" << std::endl;
        }
    
        void func(int &&arg) {
            std::cout << "func rvalue" << std::endl;
        }
    
        template <typename T>
        void wrapper(T &&args) {//作为中间层的函数模板参数
            func(std::forward<T>(args));//forward<T>(args)用于保留args的左右值属性，要不然args一定永远是左值
        }
    
        int main() {
            int a = 10; 
    
            wrapper(a);
            wrapper(20);
    
            return 0;
        }
    //    func lvalue
    //    func rvalue
    ```

#### 4.关于右值引用的几个实例思考（深刻理解右值引用。右值是右值，而右值引用一般是左值）

强调一下，为啥有时候你move完了，发现对象还有，并没有移动。因为move只是一个强制类型转换，真正实现移动的是移动构造函数和移动赋值函数。你要具体再想想移动构造函数到底有没有实现，或者说移动构造函数里做了什么。

- move完发生了什么，那要看移动构造到底发生了什么，还是人为定义的。
- 对于string类、vector类等等的，它肯定都写好了移动构造函数的
- 你自己写的移动构造函数，没有做出正确的处理（比如你发现移动完了，并没有真的发生移动），那是你的事情，你的移动构造函数实现里根本就没这个功能

**例子1：这个例子不太需要关注，这是没有移动构造的时候。移动构造函数被explicit了(或者没有实现移动构造函数时，会由复制构造函数接替它本该出现的地方)**

```c++
class X
{
public:
	X(int i = 0) :temp(i){ std::cout << "construct" << std::endl; }
	X(const X& x) { std::cout << "copy" << std::endl; }
	explicit X(X&& x) { std:: cout << "move construct" << endl; }//被explict，跟没实现移动构造几乎没区别
	X& operator=(const X&) { cout << "赋值函数" << endl; return *this; }
	~X() { cout << "析构" << endl; }
	void show(){cout<<temp<<endl;}
	int temp;
};

int main()
{
	X test(10);//construct
	X test2=move(test);//copy复制，因为移动构造函数被禁止隐式转换了，理论上它一般是接受隐式转换的。右值引用被转成了const左值引用
	X test3 = test2;//copy复制
	X &&test4=move(test);//相当于此时test4是test的一个普通左值引用，test4并没有构造
	cout<<&test<<endl;//0x7ffffffedc84
	cout<<&test2<<endl;//0x7ffffffedc88
	cout<<&test3<<endl;//0x7ffffffedc8c
	cout<<&test4<<endl;//0x7ffffffedc84
	cout << "123" << endl;
	test.show();//10
	test4.show();//10
	return 0;
    //test析构
	//test2析构
	//test3析构
}
```

**例子2：移动构造函数没有explicit时（也可以说是实现了移动构造时），与底下例子3联系着看，对于内置类型来说是一码事**

看起来用了移动构造，但因为你的类里移动构造啥也没做，所以就算有移动语义，也没真的移动

你像string类这种，它肯定内部真的实现了移动构造函数的，封装好了

```c++
class X
{
public:
	X(){}
	X(int i) :temp(i){ std::cout << "construct" << std::endl; }
	X(const X& x) { std::cout << "copy" << std::endl; }
	X(X&& x) { std:: cout << "move construct" << endl; }
	X& operator=(const X&) { cout << "赋值函数" << endl; return *this; }
	~X() { cout << "析构" << endl; }
	void show(){cout<<temp<<endl;}
	int temp;
};

int main()
{
	X test(10);//construct
	X test2=move(test);//隐式转换，move construct（如果没有移动构造，那将使用复制构造），但是你会发现，上边的移动构造，其实是空的，什么也没做，也就是说没有实现移动语义
	X test3 = test2;//隐式转换，copy复制
	X &&test4=move(test);//没有调用到移动构造函数，相当于此时test4是test的一个普通左值引用，test4并没有构造
	X &&test5=100;//construct，与X test5=100无异
    const X& test6 = move(test);//没有调用到任何构造函数，相当于此时test6是test的一个普通cosnt左值引用,test6没有构造
    
	cout<<&test<<endl;//0x7ffffffedc78
	cout<<&test2<<endl;//0x7ffffffedc7c
	cout<<&test3<<endl;//0x7ffffffedc80
	cout<<&test4<<endl;//0x7ffffffedc78，与test是一码事
	cout<<&test5<<endl;//0x7ffffffedc84
    cout<<&test6<<endl;//0x7ffffffedc78，与test是一码事，只是你不能通过test6来修改test对象里的任何内容
	cout << "123" << endl;
	test.show();//10
	test2.show();//-858993460
	test4.show();//10
	test5.show();//100
    cout<<test6.show();//10
	return 0;
	//test析构
	//test2析构
    //test3析构
    //test5析构
}
```

**例子3：对于内置类型**

```c++
int main()
{
	int a=1;//构造，左值
	//int &&b=a; error，                    												   右值引用无法绑定到左值
	int b=move(a);//理论应该隐式调用移动构造函数，没有的话调用复制构造函数。  与int b=a无异。           b是左值，用a复制构造（或者叫初始化）
	int c=a;//典型的复制构造                                                                  c是左值，用a复制构造（或者叫初始化）
	int &&d=move(a);//没有调用到移动构造函数，							与int &d =a; 无异。      d就是a的左值引用，d跟a是一码事，d没有构造
	int &&e=100;//构造，											与int e=100无异，          e就是个普通左值
	cout<<a<<endl;//1
	cout<<b<<endl;//1
	cout<<c<<endl;//1
    cout<<d<<endl;//1
	cout<<e<<endl;//100
    cout<<f<<endl;//1
    
	cout<<&a<<endl;//0x7ffffffedc88
	cout<<&b<<endl;//0x7ffffffedc8c
	cout<<&c<<endl;//0x7ffffffedc90
    cout<<&d<<endl;//0x7ffffffedc88，d跟a是一回事，一个地址
	cout<<&e<<endl;//0x7ffffffedc94
    cout<<&f<<endl;//0x7ffffffedc88，d跟a是一回事，一个地址，只是f是个常引用，你不能修改f，或者说通过修改f来修改a
	return 0;
}
```

#### 5.关于什么情况可以绑左右值

引用（被）绑定(到)值，（将）值绑定到引用.

举例：右值引用**绑定**右值，右值**绑定到**右值引用

- 右值引用可以被绑定到右值

  - ```c++
    int i = 42; 
    int &&r = i * 2;  //正确！ 将 i * 2 的结果(是个右值)绑定到 右值引用r 上
    ```

- 右值引用不可以被绑定到左值

  - ```c++
    int i = 42;
    int &&rr = i;     // 错误！ i 是左值
    ```

- 左值引用不可以被绑定到右值

  - ```c++
    int i = 1;
    int &l = 2*i;//错误！ 2*i是个右值，无法绑定到左值引用
    int &ll = 10;//错误！	10是个右值，无法绑定到左值引用
    ```

- const 的左值引用可以被绑定到右值

  - ```c++
    int i = 42;
    const int &l = i * 2;  // 正确! 可以将 const 引用绑定到右值上
    ```

  - 为什么左值引用不可以绑定到右值，但是const左值引用可以呢

    ```c++
    int l= i*2;//不行
     const int& l=i*2//可以
    ```

  - 一个字面值右值如 42，或者i*2，也会产生一个临时量，若不是常引用，同样会引发上文提到的两个问题（1.不是常量的引用与常量绑定；2.就算我们可以对其进行绑定，我们只能修改那个临时量，不能修改你原本想绑定的东西（这条见底下的tmp和dval）），所以，我们**只可以用常引用绑定右值，而普通引用是不可以的。**

  - 详细解释：

  - ```c++
    // 我们可以先看看常量引用被绑定到另外一种类型上时到底发生了什么
    double dval = 3.14;
    
    const int &ri = dval;
    
    //上边这句代码将被翻译成两句
    // 以上将一个双精度浮点数绑定到整型引用上，所以编译器会对其进行类型转换：
    const int tmp = dval;   // 先将双精度浮点数变成一个整型常量
    const int &ri = tmp;    // 让常量引用 ri 绑定到这个临时变量上
    ```

    这种情况下，ri 会被绑定到一个 **临时量** (temporary)对象上，**所谓临时量就是当编译器需要一个空间来暂存表达式的求值结果时临时创建的一个未命名的对象。** 创建临时量是需要消耗资源的，这也是本文的主题之一：右值引用的由来。

  - 我们再来看看如果 ri 不是常量引用会发生什么：

  - ```c++
    double dval = 3.14; 
    
    int &ri = dval;//这种写法其实是错的，编译器会报错
    //上边这句代码将被翻译成两句
    // 以上将一个双精度浮点数绑定到整型引用上，所以编译器会对其进行类型转换：
    const int tmp = dval;   // 还是先将双精度浮点数变成一个整型常量
    int &ri = tmp;          // 再让这个普通引用和 tmp 绑定
    ```

  - 可以看到，这样的情况下，我们可以对 ri 进行赋值，因为 ri 并不是常量引用。但有两个问题：

    - 1.tmp 是常量，而我们却用一个不是常量的引用与其绑定，这是不被允许的。
    - 2.就算我们可以对其进行绑定，而我们绑定的是 tmp，而不是 dval，所以我们对 ri 赋值并不会改变 dval 的值。

  - 于是乎，**编译器直接禁止了这种行为（`double dval = 3.14; int &ri = dval;//error!`）**。而常引用就不一样了，反正我们也不会改变其值，只是需要知道它的值而已，并且也不违反语法规则，所以将常引用与这种临时量绑定的行为是被允许的。

  - 所以关于左值引用的规则：引用类型必须与其所引用的对象类型保持一致，不然就会引发临时量的产生，从而引发以上两个问题。同样，常引用除外。

  - 同理我们推出原问题的解：一个字面值如 42，也会产生一个临时量，若不是常引用，同样会引发上文提到的两个问题，所以，我们只可以用常引用绑定右值，而普通引用是不可以的。

- const左值引用可以绑定右值引用

  - ```c++
    int a=1;
    const int& b=move(a);//这句话相当于const int&b=a;
    ```

    

#### 总结：

- 可以绑定到左值的：
  - 左值引用
  - const左值引用
- 可以绑定到右值的
  - 右值引用
  - const 的左值引用

#### 牛客版答案

右值引用是C++11中引入的新特性 , 它实现了转移语义和精确传递。它的主要目的有两个方面：

1. 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率。（移动构造、移动赋值）

2. 能够更简洁明确地定义泛型函数。（比如move、forward等的函数参数类型和返回值类型）



左值和右值的概念：

左值：能对表达式取地址、或具名对象/变量。一般指表达式结束后依然存在的持久对象。

右值：不能对表达式取地址，或匿名对象。一般指表达式结束就不再存在的临时对象。



右值和左值的区别：

1. 左值可以寻址，而右值不可以。

2. 左值可以被赋值，右值不可以被赋值，但可以用来给左值赋值。

3. 左值可变,右值不可变（仅对基础类型适用，用户自定义类型右值引用可以通过成员函数改变）。

### 26.C++如何处理返回值？

#### 1.**对于返回值为值类型的函数，其返回值为右值** （对于自定义类型稍有特殊性）

**总述：**

使用临时对象（temporary object）来保存函数的返回值。函数的返回值用于初始化调用点的一个临时对象，该临时对象就是函数调用的结果。

```c++
int func()  
{  
return 1;  
}  
int i = func(); 
```

此时，有一个临时对象用来保存 func() 函数的返回值 1 ，之后将临时对象的值赋值给变量 i 。像其它任何对象一样，临时对象需要存储空间，并且能够构造和销毁。临时对象和其它对象的区别在于从来看不到它们，编译器负责决定他们的去留以及它们存在的细节。

**细节：**

- **对于内置类型**（函数返回值，**是个右值**，更改它从来都不合法）

  ```c++
  int test()
  {
  	int a = 1;
  	return a;//返回右值
  }
  int main()
  {
  	test() = 10;//error!表达式必须是可修改的左值
  	return 0;
  }
  ```

- **对于自定义类型**（**居然可以更改这个函数返回值？！**它可以叫做“可以修改的右值”，或者说消亡值？）

  - **至于此时rvalue为什么可以被赋值，你可以认为它只是调用了assignment operator而已，就相当于调用一个rvalue的成员函数而已。**
  - **匿名临时对象能够调用非const成员函数是一个遗留问题，在新标准(C++11)下可以在成员函数参数列表之后使用成员函数引用限定符 &(限定左值)，&&(限定右值)。至于新标准下，匿名临时对象为什么没有被定义为 const，是为了支持移动语义。**
  - 所以当赋值函数X& operator=(const X&) &{ cout << "赋值函数" << endl; return *this; }加上左值限定时，这个奇葩的写法就会报错了

  ```c++
  #include<iostream>
  using namespace std;
  
  class X
  {
  public:
  	X(int i=0) { std::cout << "construct" << std::endl; }
  	X(const X& x) { std::cout << "copy" << std::endl; }
  	//X( X&& x) { std:; cout << "move construct" << endl; }
  	X& operator=(const X&) { cout << "赋值函数" << endl; return *this; }
  	~X() { cout << "析构" << endl; }
  };
  
  X func()
  {
  	X x;
  	return x;
  }
  
  int main()
  {
  	X test;
  	func() = test;//这个写法非常非常诡异，func()理论上应该是右值，但是它可以被赋值，但是&(func())取地址操作又是错误的
      //你可以理解为它只是调用了assignment operator而已，就相当于调用一个rvalue的成员函数而已。
      //你想杜绝它的方法就是，声明赋值函数为左值限定时X& operator=(const X&)& { cout << "赋值函数" << endl; return *this; }
      //func()=temp也就会报错了，就跟内置类型一样
  	return 0;
  }
  /*construct   构造test
  construct	  构造局部对象x	
  copy			复制构造给临时对象
  析构			局部对象x析构
  赋值函数		调用临时对象的operator=()
  析构			//临时对象析构
  析构*/		//test析构
  ```

#### 2.对于一般的自定义类型函数返回值的问题（都是局部对象复制构造给临时对象，临时对象复制构造给主函数的对象;或者是局部对象移动构造给临时对象，临时对象移动构造给主函数的对象）

- 未定义移动构造时

```c++
//对于自定义类型，无任何编译器优化的情况下
class X
{
public:
	X(int i=0) { std::cout << "construct" << std::endl; }
	X(const X& x) { std::cout << "copy" << std::endl; }
	//X( X&& x) { std:; cout << "move construct" << endl; }
	X& operator=(const X&) { cout << "赋值函数" << endl; return *this; }
	~X() { cout << "析构" << endl; }
};

X func()
{
	X x;
	return x;
}

int main()
{
	X test=func();
    cout<<"123"<<endl;
	return 0;
}
//输出:
//construct    	：func()函数内部的局部对象x构造
//copy		  	 :func()函数内部的return语句复制构造给主函数中的临时对象
//析构			:func()函数内部的局部对象x析构
//copy			:主函数中的临时对象复制构造给对象test
//析构			:临时对象析构
//123
//析构			:对象test析构
```

- 定义移动构造后（就是把所有上边所有的复制构造变成了移动构造，还是会有中间的临时变量）

```c++
#include<iostream>
using namespace std;

class X
{
public:
	X(int i=0) { std::cout << "construct" << std::endl; }
	X(const X& x) { std::cout << "copy" << std::endl; }
	X( X&& x) { std:; cout << "move construct" << endl; }
	X& operator=(const X&) { cout << "赋值函数" << endl; return *this; }
	~X() { cout << "析构" << endl; }
};

X func()
{
	X x;
	return x;
}

int main()
{
	X test=func();
	cout<<"123"<<endl;
	return 0;
}

//construct				:func()函数内部的局部变量x构造
//move construct		：x移动构造给临时变量
//析构					：局部变量x析构
//move construct		：临时变量移动构造给变量test
//析构					:临时变量析构
//123
//析构					:变量test析构
```

#### 3.返回值为引用类型的函数（返回值是左值）

- 当函数的返回值是引用类型时，其返回值即为return的变量，所以不需要临时对象保存其返回值。所以，对于返回值为引用类型的函数，其返回值为左值。
- 当函数返回引用类型时，没有复制返回值，相反，返回的是对象本身。
- 所以千万不要返回局部对象的引用！千万不要返回指向局部对象的指针！

```c++
char& get_val(string& str, int index)
{
	return str[index - 1];
}

int main()
{
	string str = "12345";
	char ch = get_val(str, 1);//ch的值是’1’
	cout << ch << endl;
	get_val(str, 1) = 'a';//此时str的值为”a2345”//函数的返回值是左值
	cout << str << endl;
	return 0;
}
```

#### 4.上述都是不考虑编译器优化时（RVO、NRVO）

RVO是一个不100%保证、不确定的编译器行为

- **返回值优化**（Return value optimization，缩写为**RVO**）
  - 是C++的一项编译优化技术。即删除保持函数返回值的临时对象。这可能会省略两次复制构造函数，即使复制构造函数有副作用
- **命名返回值优化**（Named return value optimization，NRVO）
  - NRVO去除了基于栈的返回值的构造与析构。虽然这会导致优化与未优化的程序的不同行为。

在g++中可以加上"-fno-elide-constructors"以禁止任何的优化行为

### 27. A  obj；与A obj()；的问题

- 使用默认构造函数来构造栈上对象时，切记不要写成 A obj()；这会被当做函数声明：
  - 即编译器认为obj（）是函数名，A是返回值类型
- 正确构造栈上对象的写法：
  - A obj;

```c++
class Person {
public:
	Person(int a = 0) :test(a) { cout << "普通构造" << endl; };
	Person(const Person& x) :test(x.test) { cout << "复制构造" << endl; };
	Person& operator=(Person x) { test = x.test; cout << "赋值函数" << endl;; return *this; }
	int test;
};

int main()
{

	Person a(100);//普通构造
	Person b;//普通构造
    
	Person c();//eroor!这样写会报错！因为编译器把这句话认为了是一个函数声明。
    //编译器认为，c()是一个函数声明，返回值是Person类型。它并不认为c是一个对象实例！
    
    //正确写法
    Person c;//普通构造，构造了一个对象c
    
	//在new对象的时候，加不加括号都行。在单独声明对象的时候，切记不要加括号。
    Person* temp = new Person();//普通构造
	Person* temp2 = new Person;//普通构造
    
    delete temp;
    delete temp2;
	return 0;
}
```

### 28.



## 二、指针、数组、字符串

### 1.请说一下C/C++ 中指针和引用的区别？

1.指针和引用的定义和性质区别：

- (1)指针：指针是一个变量，只不过这个变量存储的是一个地址，指向内存的一个存储单元；引用：引用跟原来的变量实质上是同一个东西，只不过是原变量的一个别名而已。

  - 

  ```c++
  int a=1;int *p=&a;//定义了一个整形变量和一个指针变量p，该指针变量指向a的存储单元，即p的值是a存储单元的地址。
  
  int a=1;int &b=a;//定义了一个整形变量a和这个整形a的引用b，事实上a和b是同一个东西，在内存占有同一个存储单元。
  ```

- (2)指针可以是空值即nullptr，在创建时可以不进行初始化，可以在任何时候被初始化。引用不可以为空，当被创建的时候，必须初始化。

- (3)指针的值在初始化后可以改变，即指向其它的存储单元，而引用在进行初始化后就不可以再改变了

- (4)可以有const指针，但是没有const引用；

  - 指的是 `int * const ptr`，即不可以改变指向的指针

  - ```c++
    int* const p = &a;//可以
     int& const b = a;//不行
    ```

- (5)指针可以有多级，但是引用只能是一级（int **p；合法 而 int &&a是不合法的。其实是左值引用）

- (6)使用sizeof看一个指针的大小是4，而引用则是被引用对象的大小；

- (7)指针和引用的自增(++)运算意义不一样；

- (8)如果返回动态内存分配的对象或者内存，必须使用指针，引用可能引起内存泄漏；

  - 如果返回引用，一个赋值操作就泄露了

  - ```c++
    比如string & fun()
    {
      string* str=new string("abcd");
      return *str;
    }
    如果你接收函数返回值时，不是使用引用 string & str=fun()  //正确
    而是用对象 string str=fun()，不会报错，但相当于赋值，调用的是重载的=运算符，在函数里new的内存就无法delete了。
    ```

- ```c++
  //这是类外函数
  const Rational& operator*(const Rational& lhs,const Rational& rhs)//返回创建的堆对象的引用
  {
      Rational* result=new Rational(lhs.n*rhs.n,lhs,d*rhs.d);
      return *result;
  }
  
  Rational w,x,y,z;
  w=x*y*z;//与operator*(operator*(x,y),z)相同
  //同一个语句调用了两次operator*，因而两次使用new，也就需要两次delete。两次new的对象都没有办法被捕获，没有合理的办法让我们取得operator*返回的references背后隐藏的那个指针，这绝对导致资源泄露。
  ```

  - ```c++
    class myclass {
    public:
    	myclass() {
    		cout << "构造" << endl;
    	}
    	~myclass(){
    		cout << "析构" << endl;
    	}
    };
    
    myclass& helper()
    {
    	myclass* temp = new myclass;
    	return *temp;
    }
    
    int main()
    {
    	myclass a;//myclass a=helper();这样将仅有一次构造，一次析构
    	a=helper();
    	return 0;
    }
    //构造
    //构造
    //析构
    helper()里构造的对象没有析构，资源泄露了
    ```

    (9)作为函数参数传递时，指针需要被解引用才可以对对象进行操作，而引用无需解引用，直接对引用的修改都会改变引用所指向的对象；

### 2.数组和指针的区别

#### 1.概念

- **数组**：数组是用于储存多个相同类型数据的集合。
- **指针**：指针相当于一个变量，但是它和不同变量不一样，它存放的是其它变量在内存中的地址。

#### 2.赋值

- 同类型指针变量可以相互赋值
- 数组不行，只能一个一个元素的赋值或拷贝.

#### 3.存储方式

- **数组**：数组在内存中是连续存放的，开辟一块连续的内存空间。数组是根据数组的下进行访问的，多维数组在内存中是按照一维数组存储的，只是在逻辑上是多维的。
  - 数组的存储空间，不是在静态区就是在栈上。
- **指针**：指针很灵活，它可以指向任意类型的数据。指针的类型说明了它所指向地址空间的内存。
  - 指针：由于指针本身就是一个变量，再加上它所存放的也是变量，所以指针的存储空间不能确定。

#### 4.求sizeof

- **数组**：
  - 数组所占存储空间的内存：sizeof（数组名）
    数组的大小：sizeof（数组名）/sizeof（数据类型）
- **指针**：
  - 在32位平台下，无论指针的类型是什么，sizeof（指针名）都是4，在64位平台下，无论指针的类型是什么，sizeof（指针名）都是8。

#### 5.初始化

- **数组**：

  - ```c++
    （1）char a[]={"Hello"};//按字符串初始化，大小为6.
    （2）char b[]={'H','e','l','l'};//按字符初始化（错误，输出时将会乱码，没有结束符）
    （3）char c[]={'H','e','l','l','o','\0'};//按字符初始化
    ```

  - ```c++
    这里补充一个大家的误区，就是关于数组的创建和销毁，尤其是多维数组的创建与销毁。
    （1）一维数组：
    int* arr = new int[n];//创建一维数组
    delete[] arr;//销毁
    （2）二维数组：
    int** arr = new int*[row];//这样相当于创建了数组有多少行
    for(int i=0;i<row;i++)
    {
    arr[i] = new int[col];//到这里才算创建好了
    }
    //释放
    for(int i=0;i<row;i++)
    {
    delete[] arr[i];
    }
    delete[] arr;
    ```

- **指针**：

  - ```c++
    //（1）指向对象的指针：（()里面的值是初始化值）
    int *p=new int(0) ;    delete p;
    //（2）指向数组的指针：(n表示数组的大小，值不必再编译时确定，可以在运行时确定)
    int *p=new int[n];    delete[] p;
    //（3）指向类的指针：(若构造函数有参数，则new Class后面有参数，否则调用默认构造函数，delete调用析构函数)
    Class *p=new Class;  delete p;
    //（4）指针的指针：（二级指针）
    int **pp=new (int*)[1]; 
    pp[0]=new int[6];
    delete[] pp[0];
    ```

- 指针数组与数组指针，它俩都是二级指针

  - [ ]的优先级比*高

  - **指针数组：它实际上是一个一维数组**，数组的每个元素存放的是一个指针类型的元素。

    - 

    ```c++
    int* arr[8];
    //优先级问题：[]的优先级比*高
    //说明arr是一个数组，而int*是数组里面的内容
    //这句话的意思就是：arr是一个含有8个int*的数组
    ```

    - ![image.png](https://i.loli.net/2020/08/27/w5Va6ZPu8C9Wh74.png)

  - **数组指针：它实际上是一个指针**，该指针指向一个数组。

    - ```c++
      int (*arr)[8];
      //由于[]的优先级比*高，因此在写数组指针的时候必须将*arr用括号括起来
      //arr先和*结合，说明p是一个指针变量
      //这句话的意思就是：指针arr指向一个大小为8个整型的数组。
      ```

    - ```c++
      int main()
      {
      	int arr[2][3] = {1,1,1,3,3,3};//二维数组
      	int(*arrr)[3];//数组指针，arrr指向一个包含3个int的数组名
      	arrr = &arr[0];
      	cout << (*arrr)[0] << endl;//1
      	cout<< (*arrr)[1] << endl;//1
      	cout << (*arrr)[2] << endl;//1
      	arrr = &arr[1];
      	cout << (*arrr)[0] << endl;//3
      	cout << (*arrr)[1] << endl;//3
      	cout << (*arrr)[2] << endl;//3
      	return 0;
      }
      ```

#### 6.传参

**数组传参时，会退化为指针，所以我们先来看看什么是退化！**
（1）退化的意义：C语言只会以值拷贝的方式传递参数，参数传递时，如果只拷贝整个数组，效率会大大降低，并且在参数位于栈上，太大的数组拷贝将会导致栈溢出。
（2）因此，C语言将数组的传参进行了**退化**。将整个数组拷贝一份传入函数时，**将数组名看做常量指针，传数组首元素的地址**。

**重要：**

一维数组才可以用一个指针代替，所以指针数组变为二级指针没有问题。

- 二级指针`int **p`做形参可以接收**指针数组**`int *arr[10]`实参。（数组指针做实参不行）

  - `p[0]`就是指针数组的第一个指针

- **数组指针**`int (*p)[3]`做形参可以接收二维数组`int arr[2][3]`实参  （数组指针做形参不行）

  - `*p`就相当于arr第一行的地址，`(*P)[0],(*P)[1],(*P)[2]`代表 1 2 3
  - `*(p+1)`就相当于第二行的地址，`(*(P+1))[0],(*(P+1))[1],(*(P+1))[2]`代表 4 5 6

  

- 1.一维数组的传参

  - 实参是一维数组\指针数组，传给形参

  - ```c++
    #include <stdio.h>
    //传参方式正确
    //用数组的形式传递参数，不需要指定参数的大小，因为在一维数组传参时，形参不会真实的创建数组，传的只是数组首元素的地址。（如果是变量的值传递，那么形参就是实参的一份拷贝）
    void test(int arr[])
    {}
    
    //传参方式正确
    //不传参数可以，传递参数当然也可以
    void test(int arr[10])
    {}
    
    //传参方式正确
    //一维数组传参退化，用指针进行接收，传的是数组首元素的地址
    void test(int *arr)
    {}
    
    //传参方式正确
    //*arr[20]是指针数组，传过去的是数组名,或者说二级指针
    void test2(int *arr[20])
    {}
    
    //传参方式正确
    //传过去是指针数组的数组名，代表首元素地址，首元素是个指针向数组的指针，再取地址，就表示二级指针，用二级指针接收
    void test2(int **arr)
    {}
    int main()
    {
    int arr[10] = {0};//一维数组
    int *arr2[20] = {0};//指针数组
    test(arr);
    test2(arr2);
    }
    ```

- 2.二维数组的传参

  - 实参是二维数组，传给形参

  - 传二维数组，一定要知道每行元素的个数，这样才能切割。

    - 所以对于`int arr[2][3]`，用`int** p`，`int arr[][],int arr[2][]`,`int* p[2]`接收都不行

      - 看似`int* p[2]`也对，因为`arr[2][3]`就可以看作2个一维数组，每个数组3个元素。所以`int* p[2]`代表p是一个数组，里边有两个指针，各自代表一个一维数组。但是实际上，由于传参仅传进去一个地址，不知道每行元素的个数，只知道有几行，这样也无法分割。因为元素是一行一行的串行存放的，传进来可不知道一共有6个元素

    - 只可以`int (*p)[3]`，它代表p是一个指针，指向一个含3个元素的一维数组

      - ```c++
        void fun(int(*a)[3])
        {
        	for (int i = 0; i < 3; i++)
        		cout << (*a)[i];//cout<<a[0][i];也行
        	for (int i = 0; i < 3; i++)
        		cout << (*(a+1))[i];
        }
        123456
        ```

        

  - ```c++
    //传参正确
    //表明二维数组的大小，三行五列
    void test(int arr[3][5])
    {}
    
    //传参不正确
    //二维数组的两个方括号，不能全部为空，也不能第二个为空，只能第一个为空
    void test(int arr[][])
    {}
    
    //传参正确
    //可以写成如下这样传参形式，但是不能写int arr[3][]
    void test(int arr[][5])
    {}
    
    //传参不正确
    //arr是一级指针，可以传给二维数组，但是不能正确读取
    void test(int *arr)
    {}
    
    //传参不正确
    //这里的形参是指针数组，是一维的，可以传参，但是读取的数据不正确
    void test(int* arr[3])
    {}
    
    //传参正确
    //传过去的是二维数组的数组名，即数组首元素的地址，也就是第一行的地址，第一行也是个数组，用一个数组指针接收
    void test(int (*arr)[5])
    {}
    
    //传参不正确
    //可以传参，但是在读取的时候会有级别不同的问题
    void test(int **arr)
    {}
    int main()
    {
    int arr[3][5] = {0};//二维数组
    test(arr);
    }
    ```

#### 7.函数指针

```c++
void test()
{
printf("hehe\n");
}
//pfun能存放test函数的地址
void (*pfun)();
```

函数指针的形式：`类型(*)( )，例如：int (*p)( ).`它可以存放函数的地址，在平时的应用中也很常见。

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



#### 8.函数指针数组

```c++
形式：例如int (*p[10])( );
因为p先和[ ]结合，说明p是数组，数组的内容是一个int (*)( )类型的指针
函数指针数组在转换表中应用广泛
```

#### 9.函数指针数组的指针（过于复杂）

指向函数指针数组的一个指针，也就是说，指针指向一个数组，数组的元素都是函数指针

```c++
void test(const char* str)
{
printf("%s\n", str);
}
int main()
{
//函数指针pfun
void (*pfun)(const char*) = test;
//函数指针的数组pfunArr
void (*pfunArr[5])(const char* str);
pfunArr[0] = test;
//指向函数指针数组pfunArr的指针ppfunArr
void (*(*ppfunArr)[10])(const char*) = &pfunArr;
return 0;
}
```

#### 指针和数组的主要区别如下：

| 指针                                                         | 数组                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| 保存数据的地址                                               | 保存数据                             |
| 间接访问数据，首先获得指针变量的内容，然后将其作为地址，从该地址中提取数据 | 直接访问数据，                       |
| 通常用于动态的数据结构                                       | 通常用于固定数目且数据类型相同的元素 |
| 通过Malloc分配内存，free释放内存                             | 隐式的分配和删除                     |
| 通常指向匿名数据，操作匿名函数                               | 自身即为数据名                       |

### 3.请你回答一下野指针是什么？

在这里注意了，free()释放的是指针指向的内存！注意！释放的是内存，不是指针！这点非常非常重 要！指针是一个变量，只有程序结束时才被销毁。释放了内存空间后，原来指向这块空间的指针还是存在！只不过现在指针指向的内容的垃圾，是未定义的，所以说是垃圾。因此，释放内存后应把把指针指向NULL，防止指针在后面不小心又被使用，造成无法估计的后果。

#### 野指针定义：

指向内存被释放的内存或者没有访问权限的内存的指针。野指针不能通过判断是否为NULL来避免

#### 野指针产生的原因：

- （1）指针变量未初始化：任何指针变量刚被创建时不会自动成为NULL指针，所以，指针变量在创建的同时应当被初始化，要么将指针设置为NULL，要么让它指向合法的内存。
- （2）指针释放之后未置空：有时指针在free或delete后未赋值 NULL，free或delete后它们只是把指针所指的内存给释放掉，但并没有把指针本身干掉。此时指针指向的就是“垃圾”内存。 （这个其实是悬挂指针）
  - 所以释放后的指针应立即将指针置为NULL，防止产生“野指针”。
- （3）指针操作超越变量作用域：不要返回指向栈内存的指针或引用，因为栈内存在函数结束时会被释放。

#### 其实跟悬挂指针区别不大，如果硬要区分悬挂指针跟野指针：

- 悬垂指针（dangling pointer）一般是说指向已经被释放的自由区内存（free store）的指针。悬挂指针曾经有效过
- 野指针（wild pointer）则一般是未经初始化的指针。野指针从未有效过。

#### 规避方法：

- 1.初始化指针的时候将其置为nullptr，之后对其操作。
- 2.释放指针的时候将其置为nullptr。

### 4.请你来说一下函数指针

#### 1、定义

函数指针是指向函数的指针变量。

函数指针本身首先是一个指针变量，该指针变量指向一个具体的函数。这正如用指针变量可指向整型变量、字符型、数组一样，这里是指向函数。

C在编译时，每一个函数都有一个入口地址，该入口地址就是函数指针所指向的地址。有了指向函数的指针变量后，可用该指针变量调用函数，就如同用指针变量可引用其他类型变量一样，在这些概念上是大体一致的。

#### 2、用途：

调用函数和做函数的参数，比如回调函数。

#### 3、示例：

```c++
char* fun(char * p)  {…}    // 函数fun

char* (*pf)(char * p);       // 函数指针pf

pf = &fun;            // 函数指针pf指向函数fun，
//其实加不加&都可以
//pf=fun;

char* temp=pf(p);            // 通过函数指针pf调用函数fun
//解不解引用也都可以，甚至都不用跟上边加没加&配套
//char* temp=(*pf)(p);
```

```c++
//函数回调
#include<iostream>
using namespace std;

int compute(int a,int b,int(*func)(int,int))
{	return func(a, b);}//指针解不解引用也都行,return (*func)(a, b);

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

### 5.以下四行代码的区别是什么？ const char * arr = "123"; char * brr = "123"; const char crr[] = "123"; char drr[] = "123";

#### 1.详细版答案

常量区就是内存里的数据区，静态区，或者说已初始化数据区

- **指向“常量”的指针**，“常量”在这里是真常量

  - ```c++
    const char * arr = "123";//这里没有const也效果一样
    // 可以修改指向: arr = "321";
    // 不可以修改值: arr[0]='2' //------error
    ```

  - 指针变量arr在栈区，字符串"123"保存在常量区，const本来是修饰arr指向的值不能通过arr去修改，但是字符串“123”在常量区，本来就不能改变，所以加不加const效果都一样

- **指向“常量”的指针**，“常量”可能是真常量，例如在常量区的字符串”123“；“常量”也可能是变量，例如Int num_1=1。

  - **指向“常量”的指针不代表它所指向的内容一定是常量**，只是代表不能通过解引用符（操作符*）来改变它所指向的内容。

  - ```c++
    const char *p;//上边的arr也是指向常量的指针
    ```

  - ```c++
    先看p,根据优先级它和*结合，是指针，指向char类型，但是char前面有一个const修饰，所以p所指向的内容为const类型不可修改。
    int num_a = 1;
    const int *p_a = &num_a; //底层const
    //*p_a = 2; //错误，指向“常量”的指针不能改变所指的对象
    num_a = 2;
    cout << *p_a << endl;//2
    ```

  - 注意：**指向“常量”的指针不代表它所指向的内容一定是常量**，只是代表不能通过解引用符（操作符*）来改变它所指向的内容。上例中指针p_a指向的内容就不是常量，可以通过赋值语句：num_a=2;  来改变它所指向的内容。

- **普通指针**，但是这里恰巧指向了常量区

  - ```c++
    //const char * arr = "123";//加不加const效果一样，因为指针的实际指向就是不能改变的，所以有没有const你都不能改变
    char * brr = "123";//加不加const效果一样
    ```

  - 字符串123保存在常量区，这个和arr指针指向的是同一个位置，同样不能通过brr去修改"123"的值。

- **常量数组**

  - ```c++
    const char crr[] = "123";
    ```

  - 这里123本来是在栈上的，但是编译器可能会做某些优化，将其放到常量区

- **普通数组**

  - ```c++
    char drr[] = "123";
    ```

  - 字符串123保存在栈区，可以通过drr去修改

- **指针常量**

  - ```c++
    char * const p;
    ```

  - const修饰的是p,p不能修改,p所指向的内容可以修改

  - ```c++
    int num_b = 2;
    int *const p_b = &num_b; //顶层const
    //p_b = &num_a; //错误，常量指针不能改变存储的地址值，或者说不能改变指向
    *P_b=100;//正确，p所指向的内容可以修改
    ```

#### 2.简洁版的答案

```c++
const char * arr = "123";
//指针变量arr在栈区，字符串123保存在常量区，const本来是修饰arr指向的值不能通过arr去修改，但是字符串“123”在常量区(text段)，本来就不能改变，所以加不加const效果都一样

char * brr = "123";//vs里提示错误，const char*不能初始化 char*。所以正确写法是，
char* brr = const_cast<char*>("123");//它依然是保存在常量区！你试图修改brr[0]会报错
//指针变量brr在栈区，字符串123保存在常量区，这个arr指针指向的是同一个位置，同样不能通过brr去修改"123"的值

const char crr[] = "123";
//字符数组变量crr在栈区，这里字符串123本来是在栈上的，但是编译器可能会做某些优化，将其放到常量区（我测试的还是在栈上）

char drr[] = "123";
//字符数组变量drr在栈区，字符串123也保存在栈区，可以通过drr去修改
```

### 6.

## 三、类、抽象、封装、继承、多态

### 1.为什么有的析构函数是虚函数？为什么C++默认的析构函数不是虚函数

#### 为什么有的析构函数是虚函数？

- 如果一个类打算作为基类被继承，那么一定要把这个基类声明virtual析构函数。
- 因为如果不这么做的话，一旦发生继承时，一旦发生这种情况，base class指针指向了一个derived class对象，并执行delete，而该base class带着一个non-virtual析构函数，通常发生的结果是只有base成分被销毁了，而derived成分没被销毁。这将造成严重的资源泄露。

另一种简洁的表述：将可能会被继承的父类（基类）的析构函数设置为虚函数，可以保证当我们new一个子类，然后使用基类指针指向该子类对象，释放基类指针时可以释放掉子类的空间，防止内存泄漏。

#### 为什么C++默认的析构函数不是虚函数

因为**每个多态类有一个虚表**（virtual table），虚表中有当前类的各个虚函数的入口地址，**每个对象有一个指向当前类的虚表的指针**（虚指针vptr）。**它们会占用额外的内存**。而对于不会被继承的类来说，其析构函数如果是虚函数，就会浪费内存。因此C++默认的析构函数不是虚函数，而是只有当需要当作父类时，设置为虚函数。

其次，将一个普通的类构造为多态类还将影响到代码向其他语言传递（移植）

### 2.C++中析构函数的作用

析构函数与构造函数对应，**当对象结束其生命周期**，如对象所在的函数已调用完毕时，**系统会自动执行析构函数**。

析构函数名也应与类名相同，只是在函数名前面加一个位取反符~，例如~stud( )，以区别于构造函数。它不能带任何参数，也没有返回值（包括void类型）。只能有一个析构函数，不能重载。

如果用户没有编写析构函数，编译系统会自动生成一个缺省的析构函数（即使自定义了析构函数，编译器也总是会为我们合成一个析构函数，并且如果自定义了析构函数，编译器在执行时会先调用自定义的析构函数再调用合成的析构函数），它也不进行任何操作。所以许多简单的类中没有用显式的析构函数。

如果一个类中有指针，且在使用的过程中动态的申请了内存，那么最好显示构造析构函数在销毁类之前，释放掉申请的内存空间，避免内存泄漏。

类析构顺序：1）派生类本身的析构函数；2）派生类中对象成员析构函数；3）基类析构函数。

### 3.请你来说一下静态函数和虚函数的区别

- 静态函数：
  - 在函数返回类型前加static，函数就定义为静态函数。函数的定义和声明在默认情况下都是extern的，但**静态函数只是在声明他的文件当中可见，不能被其他文件所用**。函数的实现使用static修饰，那么这个函数只可在本cpp内使用，不会同其他cpp中的同名函数引起冲突。
- 虚函数：
  - 在某基类中声明为 virtual 并在一个或多个派生类中被重新定义的成员函数。C++中的虚函数的作用主要是实现了多态的机制。
- 区别：
  - 静态函数（其实普通函数也是如此啊）在编译的时候就已经确定运行时机，虚函数在运行的时候动态绑定。虚函数因为用了虚函数表机制，调用的时候会增加一次内存开销。

### 4.你理解的虚函数和多态（对象中有虚指针，每个类有一个虚表，对象里的虚指针指向虚表，不同的类虚表中存有各个虚函数的入口指针）

#### 多态

多态的实现主要分为静态多态和动态多态，

- 静态多态主要是重载（函数重载，运算符重载），在编译的时候就已经确定；
- 动态多态是用虚函数机制实现的，在运行期间动态绑定。

举个例子：一个父类类型的指针指向一个子类对象时候，使用父类的指针去调用子类中重写了的父类中的虚函数的时候，会调用子类重写过后的函数，在父类中声明为加了virtual关键字的函数，在子类中重写时候不需要加virtual也是虚函数。

#### 虚函数

为什么运行时可以动态绑定呢？运行时没有编译环境了，只有操作系统，谁帮我们确定该执行哪个函数体呢？

答：其实包含在对象里！对象里的虚指针就决定了它真正应该调用的函数是哪个，而不取决于该对象被什么指针指向（管你是base指针还是derived指针，因为实际上是由对象里的虚指针决定的）。

虚函数的实现：在有虚函数的类中，编译器会为每个**有虚函数的类**创建一个**虚函数表**，该虚函数表将被该类的所有对象共享。虚表指针在类对象中，每个同类对象中都有个一个vptr，指向内存中的vtable，所有同类对象，共享一个vtable，但是**每个对象都自带一个vptr**指向这个vtable,虚表指针是对象的第一个数据成员，虚表指针的初始化发生在构造函数过程中。类的最开始部分是一个虚函数表的指针，这个指针指向一个虚函数表，表中放了虚函数的地址，实际的虚函数在代码段(.text)中。当子类继承了父类的时候也会继承其虚函数表，当子类重写父类中虚函数时候，会将其继承到的虚函数表中的地址替换为重新写的函数地址。使用了虚函数，会增加访问内存开销，降低效率。

#### 总结：

- 每个多态类（Based、derived）都有一个虚表，基类有基类的虚表，派生类有派生类的虚表，虚表里记录了应该调用的各个虚函数的地址。
- 每个多态类对象都有一个**虚指针，它指向应该调用的虚表的地址**，通过多态类型的指针或引用调用成员函数时，**通过虚指针找到虚表，进而找到所调用的虚函数的入口地址**



- 虚表

  - **每个多态类有一个虚表**（virtual table）
  - 也就是说Base和Derived类都将有自己的虚函数表
  - **虚表中有当前类的各个虚函数的入口地址**
  - **每个对象有一个指向当前类的虚表的指针（虚指针vptr）**
  - 在编译时，编译器做好虚表，等待运行时查找

- 动态绑定的实现

  - 构造函数中为对象的虚指针赋值
  - 通过多态类型的指针或引用调用成员函数时，通过虚指针找到虚表，进而找到所调用的虚函数的入口地址
  - 通过该入口地址调用虚函数。

![NSJHxO.png](https://s1.ax1x.com/2020/06/14/NSJHxO.png)

- Derived类的虚表会覆盖那些重新定义的虚函数，没有重新定义的，那么还是指向基类
- 所以，**虽然一个Derived对象被Base指针指向，但是该Derived实例（对象）里的虚指针VFT其实仍然指向Derived类的虚函数表，就可以实现运行时多态了。**

#### 别人的回答

子类若重写父类虚函数，虚函数表中，该函数的地址会被替换，对于存在虚函数的类的对象，在VS中，对象的对象模型的头部存放指向虚函数表的指针，通过该机制实现多态。

### 5.const修饰成员函数的目的是什么？

const修饰的成员函数表明该函数调用不会对对象做出任何更改，事实上，如果确认不会对对象做更改，就应该为函数加上const限定，这样无论const对象还是普通对象都可以调用该函数。

用const成员函数的两个好处

1. 可以从声明看出来，哪个函数可以改动对象内容，而哪个函数不行
2. const对象只能使用const成员函数。

### 6.如果同时定义了两个函数，一个带const，一个不带，会有问题吗？（本质上是形参为底层const可以重载，形参为顶层const不能重载。因为比如 char*  与 char* const，作为形参接受参数时，编译器完全看不出他俩有什么区别）

- 当这两个函数作为普通的函数时，编译会报错，因为无法仅按返回类型区分两个函数

  - 如果函数名相同，在相同的作用域内，其参数类型、参数个数，参数顺序不同等能构成函数重载。
  - 换句话说，各个重载函数的形参必须不同:即 类型不同 或 个数不同。

- 当这两个函数作为类的成员函数时，const关键字可以被用于参与对重载函数的区分

  - 如果同时在类中，对于函数名相同参数列表也相同的成员函数的const函数和非const函数能够构成重载。
  - 它们被调用的时机为：如果定义的对象是常对象，则调用的是const成员函数，如果定义的对象是非常对象，则调用重载的非const成员函数。
  - 如果是非常对象，某个成员函数仅有const版本，这也是可以调用的

- 其实本质上是，形参是底层const可以重载，形参为顶层const则不能重载。因为比如 char*  与 char* const，作为形参接受参数时，编译器完全看不出他俩有什么区别

  - 所以，两个函数作为类的成员函数时，const关键字修饰的其实是this指针，你无法通过这个指针来修改对象，也就意味着，它是一个底层const指针。所以是形参是底层const指针的函数，和形参是非const指针之间的重载。

  - 换句话说，两个函数，如果形参是顶层const指针和非const指针的区别，那么不能重载

  - 两个函数，如果形参是底层const指针和非const指针的区别，那么可以重载

  - ```c++
    void testfunc(const char* a) { cout << "底层const"<< endl; }
    void testfunc(char* a) { cout << "非const" << endl; }
    //void testfunc(char* const a) { cout << "顶层const" << endl; }//它与testfunc(char* a)冲突了
    //void testfunc(const char* const a) { cout << "底层and顶层const" << endl; }//它与testfunc(const char* a)冲突了，换句话说，char*与char* const毫无区分度
    int main()
    {
        testfunc("123");//输出：底层const
        char yyd[] = "123";
        testfunc(yyd);//输出：非const
        return 0;
    }
    ```

```c++
//这两个也是不能重载的，把你当做编译器，来判断一下，传入实参过来，两个形参有没有差别就知道了。
void fun(int a) {}
void fun(const int a){}//不能重载
```



### 7.C++中拷贝复制构造和拷贝赋值函数的形参能否进行值传递？

#### 1.对于拷贝复制构造函数(不能进行值传递，只能使用引用，const不const都行)

```c++
 // 拷贝构造函数原型
 Person( Person& );  // method 1
 Person( const Person& );  // method 2
 
 // 下面这种原型是错的
 Person( Person );  // 不能进行值传递，只能传递引用，这样写会报错
```

不能进行值传递的原因：如果自身参数不是引用，则永远不会调用成功------为了调用拷贝构造函数，我们必须拷贝它的实参，但为了拷贝实参，我们又必须调用拷贝构造函数，如此无限循环。

```c++
class Person{
public:
    Person(int i=0):test(i){};
    Person(Person x):test(x.test){};//它肯定不能通过编译，假若可以，我们思考一下
    int test;
}

int main(){
    
    Person temp(1);
    Person temp2(temp);//调用复制构造函数
    //你想要构造temp2，调用这个函数Person(Person x):test(x.test){}。
    //那么首先就得调用复制构造函数将实参temp复制构造到形参Person x吧，
    //那么这个复制构造形参的过程即 Person x(temp)，又要调用这个函数Person(Person x):test(x.test){}。
    //那你又得把实参temp复制构造到形参Person x吧，
    //那么这个复制构造形参的过程即 Person x(temp)，又要调用这个函数Person(Person x):test(x.test){}
    //.....循环往复，会形成递归调用栈溢出
    //问题就出在，你本想复制构造一个对象，然而实参到形参的过程也是一个复制构造，将无限循环下去
    return 0;
}
```

#### 2.对于拷贝赋值函数（能进行值传递，就只是多一个额外的实参到形参的一次复制拷贝）

```c++
// 一般的拷贝赋值函数原型
Person& operator=( const Person& x);

//但是值传递也没错
Person& operator=(Person x)//仅仅是多了实参到形参的一次复制拷贝
```

为了与内置类型的赋值保持一致，赋值运算符通常返回一个指向其左侧运算对象的引用，这样就可以实现连等的功能（a = b = c）

为了验证我们的猜测（能进行值传递），我们做了一下验证，代码如下：

```c++
class Person {
public:
	Person(int a = 0) :test(a) { cout << "普通构造" << endl; };
	Person(const Person& x) :test(x.test) { cout << "复制构造" << endl; };
	Person& operator=(Person x) { test = x.test; cout << "赋值函数" << endl;; return *this; }
	int test;
};

int main()
{
	Person a(100);//普通构造
	Person b;//普通构造
	b = a;//复制构造 (赋值函数的实参传形参中发生)
			//赋值函数
	cout << a.test << endl;//100
	cout << b.test << endl;//100
	return 0;
}
```

### 8.给你一个类，里面有static成员，virtual函数，虚函数表等，说说他们的内存位置

#### 0.类对象内存分布

如果一个类是局部变量则该类数据存储在栈区，如果一个类是通过new/malloc动态申请的，则该类数据存储在堆区。

#### 1.static修饰符

- static修饰成员变量
  - 对于非静态数据成员，每个类对象都有自己的拷贝。
  - 而静态数据成员被当做是类的成员，无论这个类被定义了多少个，静态数据成员都只有一份拷贝，为该类型的所有对象所共享(包括其派生类)。所以，静态数据成员的值对每个对象都是一样的，它的值可以更新。
  - **因为静态数据成员在全局数据区（静态区，bss段和data段）分配内存**，属于本类的所有对象共享，所以它不属于特定的类对象，在没有产生类对象前就可以使用。
  - **类的静态数据成员必须在类外定义和初始化**，用(::)来指明所属的类。
- static修饰成员函数
  - 静态成员函数和静态数据成员一样，它们都属于类的静态成员，它们都不是对象成员。因此，对静态成员的引用不需要用对象名。
  - 静态成员函数主要用于处理该类的静态数据成员。在静态成员函数的实现中不能直接引用类中说明的非静态成员，可以引用类中说明的静态成员（这点非常重要）。
  - 与普通的成员函数相比，静态成员函数由于不是与任何的对象相联系，因此它不具有this指针。从这个意义上来说，它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，只能调用其他的静态成员函数。
  - 如果静态成员函数中要引用非静态成员时，可通过参数传递的方式得到对象名，然后再通过对象名来引用。从中可看出，调用静态成员函数使用如下格式：<类名>::<静态成员函数名>(<参数表>);
  - **Static修饰的成员函数，在代码区分配内存。**

#### 2.virtual修饰符

- 如果一个类是局部变量则该类数据存储在栈区，如果一个类是通过new/malloc动态申请的，则该类数据存储在堆区。
- 如果该类是virutal继承而来的子类，则该类的虚函数表指针和该类其他成员一起存储。虚函数表指针指向只读数据段中的类虚函数表，虚函数表中存放着一个个函数指针，函数指针指向代码段中的具体函数。
- 如果类中成员是virtual属性，会隐藏父类对应的属性。

![image.png](https://i.loli.net/2020/09/21/RYCvAJWMU1tych8.png)

假如初始化方式是 A* obj=new A;

- 指针变量obj在栈
- 对象A的实例在堆
  - 对象A里有虚指针（存的地址指向虚表，即常量区地址，因为虚表存在roadata），各种成员变量等等（这些都存在堆里）
- 虚指针指向某个类（基类或者某派生类）虚表，**虚表存在c++中内存五区的常量存储区（对应rodata段）**
  - 虚表中存着各虚函数的入口指针（各指针地址指向代码区，因为各虚函数函数存在代码区text）
- 各虚函数存在代码区
  - text段

#### 3.特别说一下虚函数表的位置

- 虚函数表vtable在Linux/Unix中存放在可执行文件的只读数据段中(rodata)，对应于c++中内存五区的常量存储区（文字常量区）

### 8.请你来说一下C++中类成员的访问权限

**类的一个特征就是封装，public和private作用就是实现这一目的**。

**类的另一个特征就是继承，protected的作用就是实现这一目的**。

#### 1.类内和友元函数可以访问所有权限，类外只可以访问public权限的成员

C++通过 public、protected、private 三个关键字来控制成员变量和成员函数的访问权限，它们分别表示公有的、受保护的、私有的，被称为成员访问限定符。

- 在类的内部（定义类的代码内部），无论成员被声明为 public、protected 还是 private，都是可以互相访问的，没有访问权限的限制。

- 在类的外部（定义类的代码之外），只能通过对象访问成员，并且通过对象只能访问 public 属性的成员，不能访问 private、protected 属性的成员

- 举例：

  - 

  ```c++
  #include<iostream>
  #include<assert.h>
  using namespace std;
  class A{
  public:
    int a;
    A(){
      a1 = 1;
      a2 = 2;
      a3 = 3;
      a = 4;
    }
    void fun(){
      cout << a << endl;    //正确
      cout << a1 << endl;   //正确
      cout << a2 << endl;   //正确，类内访问
      cout << a3 << endl;   //正确，类内访问
    }
  public:
    int a1;
  protected:
    int a2;
  private:
    int a3;
  };
  int main(){
    A itema;
    itema.a = 10;    //正确
    itema.a1 = 20;    //正确
    itema.a2 = 30;    //错误，类外（即对象）不能访问protected成员
    itema.a3 = 40;    //错误，类外（即对象）不能访问private成员
    return 0;
  }
  ```

#### 2.具体地解析3个权限的区别（private仅限类内（或者友元）访问，public类内外都可以访问，protected与priavate的区别仅在于继承时子类可以访问protected但不能访问private）

- private：
  - 只能由该类中的函数、其友元函数访问，不能被任何其他访问，该类的对象（也就是类外）也不能访问。
  - **派生类（子类）也无法访问**
  - 完全是自己的，只能自己用，谁都不能用，好比一个人身体、四肢；
- protected： 可以被该类中的函数、其友元函数、其**派生类（子类）内部可以访问**，但不能被该类的对象（也就是类外）访问。
  - protected成员与private的区别仅限于继承时，子类能否使用
  - 仅限自己和自己的儿子以及儿子的儿子的儿子……使用的一些东西
- public: 可以被该类中的函数、其友元函数、派生类（子类）内部可以访问，也可以由该类的对象（也就是类外）访问。
  - 通常是功能，好比一个人的工作，比如程序员，老板让程序员写代码，写代码（）就是程序员的一个功能，并且是public 的，可以被老板调用，但是程序员写代码的时候要用到自己private的东西，比如两只手；

#### 3.三种继承方式的特点（3个权限和3个继承方式这是两码事）

先记住：不管是否继承，上面的规则（即类内和友元开放所有权限，类外只可以访问public权限的成员）永远适用！

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。

**继承之后的属性变换：**

**1.public继承：**基类public成员，protected成员，private成员的访问属性在派生类中分别变成：public, protected, private

**2.protected继承：**基类public成员，protected成员，private成员的访问属性在派生类中分别变成：protected, protected, private

**3.private继承：**基类public成员，protected成员，private成员的访问属性在派生类中分别变成：private, private, private



但无论哪种继承方式，上面两点都没有改变：

**1.private成员只能被本类成员（类内）和友元访问，不能被派生类内部访问；**

**2.protected、public成员（以继承前为准）可以被派生类内部访问。**

**3.至于类外能否访问，那么请自行判断继承以后的权限，然后继续按照类外只可以访问public权限的成员的规则即可**



##### 1.public继承（is-a关系，每一个derived对象同时也是一个base对象）

```c++
#include<iostream>
#include<assert.h>
using namespace std;

class A{
public:
  int a;
  A(){
    a1 = 1;
    a2 = 2;
    a3 = 3;
    a = 4;
  }
  void fun(){
    cout << a << endl;    //正确
    cout << a1 << endl;   //正确
    cout << a2 << endl;   //正确
    cout << a3 << endl;   //正确
  }
public:
  int a1;
protected:
  int a2;
private:
  int a3;
};
class B : public A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员
    cout << a1 << endl;       //正确，基类的public成员，在派生类中仍是public成员。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中仍是protected可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;
  cout << b.a1 << endl;   //正确
  cout << b.a2 << endl;   //错误，类外不能访问protected成员
  cout << b.a3 << endl;   //错误，类外不能访问private成员
  system("pause");
  return 0;
}
```

##### 2.protected继承：（关系不明确）

```c++
#include<iostream>
#include<assert.h>
using namespace std;
class A{
public:
  int a;
  A(){
    a1 = 1;
    a2 = 2;
    a3 = 3;
    a = 4;
  }
  void fun(){
    cout << a << endl;    //正确
    cout << a1 << endl;   //正确
    cout << a2 << endl;   //正确
    cout << a3 << endl;   //正确
  }
public:
  int a1;
protected:
  int a2;
private:
  int a3;
};
class B : protected A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员。
    cout << a1 << endl;       //正确，基类的public成员，在派生类中变成了protected，可以被派生类访问。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中还是protected，可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;       //正确。public成员
  cout << b.a1 << endl;      //错误，protected成员不能在类外访问。
  cout << b.a2 << endl;      //错误，protected成员不能在类外访问。
  cout << b.a3 << endl;      //错误，private成员不能在类外访问。
  system("pause");
  return 0;
}
```

##### 3.private继承：（含义：implemented-in-tems-of，derived对象根据base对象实现而得）

```c++
#include<iostream>
#include<assert.h>
using namespace std;
class A{
public:
  int a;
  A(){
    a1 = 1;
    a2 = 2;
    a3 = 3;
    a = 4;
  }
  void fun(){
    cout << a << endl;    //正确
    cout << a1 << endl;   //正确
    cout << a2 << endl;   //正确
    cout << a3 << endl;   //正确
  }
public:
  int a1;
protected:
  int a2;
private:
  int a3;
};
class B : private A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员。
    cout << a1 << endl;       //正确，基类public成员,在派生类中变成了private,可以被派生类访问。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中变成了private,可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;       //正确。public成员
  cout << b.a1 << endl;      //错误，private成员不能在类外访问。
  cout << b.a2 << endl;      //错误, private成员不能在类外访问。
  cout << b.a3 << endl;      //错误，private成员不能在类外访问。
  system("pause");
  return 0;
}
```



所以派生类包含了基类所有成员以及新增的成员，同名的成员被隐藏起来，调用的时候只会调用派生类中的成员。

如果要调用基类的同名成员，可以用以下方法：

```text
int main(){

  B b(10);
  cout << b.a << endl;
  cout << b.A::a << endl;

  system("pause");
  return 0;
}
```

### 9.C++类内可以定义引用数据成员吗？

可以，必须通过成员函数初始化列表初始化。

必须初始化列表初始化的原因是，因为引用在声明时必须同时初始化。你想在大括号里a=b，这是不行的。

C++类内的引用数据成员以及const常量都只能通过构造函数的初始化列表进行初始化，因为一旦进入构造函数函数体内部，就不再是初始化操作而是赋值操作，引用数据成员和const常量都是不能进行赋值操作的。

```c++
class A {
public:
    A(int& b) :a(b) {}
    int& a;
};
int main()
{
    int b = 10;
    A test(b);
    cout << test.a << endl;//10
    cout<<&test<<endl;//0x7ffffffedca0
    
    cout << &(test.a) << endl;//0x7ffffffedc9c
    cout << &b << endl;//0x7ffffffedc9c，两个变量地址一样
    return 0;
}
```

### 10.必须采用初始化列表的三种情况：

#### 1.总体概括

- 情况一、需要初始化的数据成员是**对象**的情况

  - 数据成员是对象，并且这个对象**只有含参数的构造函数，没有无参数的构造函数；**那么只能列表初始化

- 情况二、需要**初始化const**修饰的类成员**或初始化引用**成员数据；

  - 当类成员中含有一个const对象时，或者是一个引用时，他们也必须要通过成员初始化列表进行初始化，**因为这两种对象要在声明后马上初始化，而在构造函数中，做的是对他们的赋值，这样是不被允许的。**

- 情况三、**子类初始化父类的成员**，需要在(并且也**只能在)参数初始化列表中显示调用父类的构造函数**；

  - **不论父类的public还是protected、private成员，你在子类中想要初始化，只能通过父类的构造函数。而不能自行列表初始化**

    - 解释：因为初始化列表中，才进行初始化父类，子类可以理解为还没有继承到父类的成员。只有进入构造函数内部，才继承到了。继承到以后才可以访问

  - **父类的public和protected与private成员不同的是，虽然3种类型的成员都不能自行构造，但是父类的public和proteted成员可以在子类构造函数内部被访问赋值。**

  - ```c++
    //错误示范
    class Test {//父类
    public:
        Test() {};
        Test(int x) { data_private = x; }
        void show() { cout << data_private << endl; cout << data_protected << endl; }
        int data_public;
    protected:
        int data_protected;
    private:
        int data_private;
    };
    class Mytest :public Test {//子类
    public:
        Mytest() :Test(10),data_protected(100),data_public(1000) {//错误！！！报错说Mytest里没有成员data_protected、data_public
            
            //不论父类的public还是protected、private成员，你在子类中想要初始化，只能通过父类的构造函数。而不能自行列表初始化
    		//public和protected与private成员不同的是，虽然3种类型的成员都不能自行构造，但是public和proteted成员可以在构造函数内部被访问赋值。
        }
    };
    
    
    //正确示范
    class Test {//父类
    public:
        Test() {};
        Test(int x) { data_private = x; }
        void show() { cout << data_private << endl; cout << data_protected << endl; }
        int data_public;
    protected:
        int data_protected;
    private:
        int data_private;
    };
    class Mytest :public Test {//子类
    public:
        Mytest() :Test(10) {
            data_protected = 100;//data_protected其实在列表初始化阶段就构造了，这是赋值而已
            data_public = 1000;//data_public其实在列表初始化阶段就构造了，这是赋值而已
        }
    };
    int main()
    {
        Test* p = new Mytest();
        p->show();
        cout << p->data_public << endl;
        delete p;
        return 0;
    }
    //10
    //100
    //1000
    ```

    

#### 2.实际例子

- 情况一的例子

  - 数据成员是对象，并且这个对象只有含参数的构造函数，没有无参数的构造函数；

  - 如果我们有一个类成员（比如Mytest类里的private成员test），它本身是一个类（Test类）或者是一个结构，而且这个成员它只有一个带参数的构造函数，而**没有默认构造函数（就是不需要实参的构造函数）**，这时要对这个类成员进行初始化，就必须调用这个类成员的带参数的构造函数，如果没有初始化列表，那么他将无法完成第一步，就会报错。

  - ```c++
    #include "iostream"
    using namespace std;
    class Test
    {
     public:
        Test (int, int, int){
        cout <<"Test" << endl;
     };
     private:
        int x;
        int y;
        int z;
    };
    class Mytest 
    {
     public:
        Mytest():test(1,2,3){       //必须使用列表初始化！因为Test类没有默认构造函数，只能在这里进行构造。如果有默认构造函数的话，那么可以写在大括号里，此时将默认构造test，再在构造函数内部进行赋值操作
        cout << "Mytest" << endl;
        };
    private:
        Test test; //声明
    };
    int _tmain(int argc, _TCHAR* argv[])
    {
     Mytest test;
     return 0;
    }
    ```

- 情况二的例子

  - 需要**初始化const**修饰的类成员**或初始化引用**成员数据

  - 当类成员中含有一个const对象时，或者是一个引用时，他们也必须要通过成员初始化列表进行初始化，**因为这两种对象要在声明后马上初始化，而在构造函数中，做的是对他们的赋值，这样是不被允许的。**

  - ```c++
    //1.类成员包含const对象
    class Test
    {
     priate:
        const int a;             //const成员声明
     public:
        Test():a(10){}           //必须列表初始化
    };
    //2.类成员包含引用对象
    class Test
    {
     private:
         int &a;                        //声明
     public:
         Test(int a):a(a){}        //必须列表初始化
    }
    ```

- 情况三的例子

  - 子类初始化父类的私有成员，需要在(并且也只能在)参数初始化列表中显示调用父类的构造函数：

  - ```c++
    class Test{//父类
    public:
        Test(){};
        Test (int x){ int_x = x;}
        void show(){cout<< int_x << endl;}
    private:
        int int_x;
    };
    class Mytest:public Test{//子类
    public:
        Mytest() ：Test(110){//子类初始化父类的私有成员，只能在参数初始化列表中显示调用父类的构造函数
          //Test(110);            //  构造函数只能在初始化列表中被显示调用，不能在构造函数内部被显示调用
        }
    };
    int _tmain(int argc, _TCHAR* argv[])
    {
     Test *p = new Mytest();
     p->show();
     return 0;
    }
    //结果：如果在构造函数内部被显示调用，而没有初始化列表的话。输出结果是：-842150451（原因是这里调用了无参构造函数初始化了父对象，而内部那句Test(100)仅仅是一个临时对象而已）；
    //     如果在初始化列表中被显示调用输出结果是：110
    ```

    

### 11.C++中struct和class的区别

#### 1.struct与class区别

在C++中，结构体是一种特殊形态的类。

结构体和类的**唯一**区别就是：  结构体和类具有不同的默认访问控制属性。

- 　**类中，对于未指定访问控制属性的成员，其访问控制属性为私有类型（private）**
- 　**结构体中，对于未指定任何访问控制属性的成员，其访问控制属性为公有类型（public）**

```c++
struct Test1 {
    int a;//默认public
};

class Test2 {
    int b;//默认private
};

int main()
{
    Test1 yyd1;
    Test2 yyd2;
    yyd1.a = 1;
    yyd2.b = 2;//error！成员b不可访问
    return 0;
}
```

还有一个微小的区别：（这其实也可以从默认访问权限推断而知）

- struct继承其他类的时候，如果不写继承级别，则默认是public 继承。
- class则默认private 继承。只是一般情况下我们不会省略继承级别关键字。

但实际上，我们一般都会写上继承类型而不会默认（public、protected、private继承）

#### 2.注意

注意，在C++中，struct跟class其实没有太大区别，struct也有构造函数、析构函数。也可以自定义函数成员。也可以加上访问权限public、protected、private。也可以继承。

示例，把struct关键字换成class都没有问题（当然得记得加上public，因为class默认是private）

```c++
#include<iostream>
#include<string>
 
struct Person
{
  Person(std::string name);
  std::string greet(std::string other_name);
  std::string m_name;
};
 
Person::Person(std::string name)
{
    m_name = name;
}
 
std::string Person::greet(std::string other_name)
{
    return "Hi " + other_name + ", my name is " + m_name;
}
 
int main()
{
    Person m_person("JANE");
    std::string str = m_person.greet("JOE");
    std::cout<<str<<std::endl;
}
```

#### 3.C++与C中的struct区别

**C++中保留结构的原因：**

- C语言结构体的目的是使用结构体将不同类型数据组成整体，方便于保存数据。
- C++之所以要引入结构体，是为了保持和C程序的兼容性。
  - C++在struct之外引入了class关键字，但为了保持与C程序的兼容，C++保留了struct关键字，并规定结构体默认访问控制权限为公有类型。

**C++与C中的struct区别区别：**

- C++为C语言中的结构体引入了成员函数、访问控制权限、继承、包含多态等面向对象特性。

- 另外，C语言中，空结构体的大小为0，而C++中空结构体（属于空类）的大小为1。

  - C++中空类的大小为1的原因：

    - 空类也可以实例化，类实例化出的每个对象都需要有不同的内存地址，为使每个对象在内存中的地址不同，所以在类中会加入一个隐含的字节。

  - 实测：

    - ```c
      #include <stdio.h>
       
      struct A
      {
      }aa;
       
       
      int main()
      {
              printf("%ld\n", sizeof(struct A));
              printf("%ld\n", sizeof(aa));
              return 0;
      }
      ```

    - ```shell
      //用gcc编译并执行
      yangyadong@DESKTOP-UTDHAA6:~/myproject$ gcc yyd.c -o yyd && ./yyd
      0
      0
      //用g++编译并执行    
      yangyadong@DESKTOP-UTDHAA6:~/myproject$ g++ yyd.c -o yyd && ./yyd
      1
      1
      ```

      

## 四、linux

### 1.请你来说一下fork函数

Fork：创建一个和当前进程映像一样的进程可以通过fork( )系统调用：

成功调用fork( )会创建一个新的进程，它几乎与调用fork( )的进程一模一样，这两个进程都会继续运行。在子进程中，成功的fork( )调用会返回0。在父进程中fork( )返回子进程的pid。如果出现错误，fork( )返回一个负值。

最常见的fork( )用法是创建一个新的进程，然后使用exec( )载入二进制映像，替换当前进程的映像。这种情况下，派生（fork）了新的进程，而这个子进程会执行一个新的二进制可执行文件的映像。这种“派生加执行”的方式是很常见的。

在早期的Unix系统中，创建进程比较原始。当调用fork时，内核会把所有的内部数据结构复制一份，复制进程的页表项，然后把父进程的地址空间中的内容逐页的复制到子进程的地址空间中。但从内核角度来说，逐页的复制方式是十分耗时的。现代的Unix系统采取了更多的优化，例如Linux，采用了写时复制的方法，而不是对父进程空间进程整体复制。

```c++
int main(void)
{
    pid_t pid;
    signal(SIGCHLD, SIG_IGN);
    printf("before fork pid:%d\n", getpid());
    int abc = 10;
    pid = fork();//fork()一个进程
	if(pid==-1)//错误返回
    {
        perror("tile");
		return -1;
    }
    if(pid>0)    //父进程空间
    {
       abc++;
        printf("parent:pid:%d \n", getpid());
        printf("abc:%d \n", abc);
        sleep(20); 
    }
    else if(pid==0)//子进程空间
    {
        abc++;
		printf("child:%d,parent: %d\n", getpid(), getppid());
		printf("abc:%d", abc);	
    }
    
    printf("fork after...\n");
    return 0;
}
```

### 2.请你说一说select（还不会）

### 3.请你说说fork,wait,exec函数（）

#### 1.fork()

## 五、编译与底层

### 1.请你来说一下一个C++源文件从文本到可执行文件经历的过程？

#### 简略版

对于C++源文件，从文本到可执行文件一般需要四个过程：

- 预处理阶段：对源代码文件中文件包含关系（头文件）、预编译语句（宏定义）进行分析和替换，生成预编译文件。
- 编译阶段：将经过预处理后的预编译文件转换成特定汇编代码，生成汇编文件
  - c代码到汇编代码
- 汇编阶段：将编译阶段生成的汇编文件转化成机器码，生成可重定位目标文件
  - 汇编代码到机器码
- 链接阶段：将多个目标文件及所需要的库连接成最终的可执行目标文件

#### 详细版

**1）预编译**(.c到.i)

**主要处理源代码文件中的以“#”开头的预编译指令**。处理规则见下

1、删除所有的#define，展开所有的宏定义。

2、处理所有的条件预编译指令，如“#if”、“#endif”、“#ifdef”、“#elif”和“#else”。

3、处理“#include”预编译指令，将文件内容替换到它的位置，这个过程是递归进行的，文件中包含其他文件。

4、删除所有的注释，“//”和“/**/”。

5、保留所有的#pragma 编译器指令，编译器需要用到他们，如：#pragma once 是为了防止有文件被重复引用。

6、添加行号和文件标识，便于编译时编译器产生调试用的行号信息，和编译时产生编译错误或警告是能够显示行号。

7、预编译之后生成的xxx.i或xxx.ii文件

**2）编译**(.i到.s)

把预编译之后生成的xxx.i或xxx.ii文件，进行一系列词法分析、语法分析、语义分析及优化后，生成相应的汇编代码.s文件。

1、词法分析：利用类似于“有限状态机”的算法，将源代码程序输入到扫描机中，将其中的字符序列分割成一系列的记号。

2、语法分析：语法分析器对由扫描器产生的记号，进行语法分析，产生语法树。由语法分析器输出的语法树是一种以表达式为节点的树。

3、语义分析：语法分析器只是完成了对表达式语法层面的分析，语义分析器则对表达式是否有意义进行判断，其分析的语义是静态语义——在编译期能分期的语义，相对应的动态语义是在运行期才能确定的语义。

4、优化：源代码级别的一个优化过程。

5、目标代码生成：由代码生成器将中间代码转换成目标机器代码，生成一系列的代码序列——汇编语言表示。

6、目标代码优化：目标代码优化器对上述的目标机器代码进行优化：寻找合适的寻址方式、使用位移来替代乘法运算、删除多余的指令等。

**3）汇编**（.s到.o）

将汇编代码转变成机器可以执行的指令(机器码文件)。 汇编器的汇编过程相对于编译器来说更简单，没有复杂的语法，也没有语义，更不需要做指令优化，只是根据汇编指令和机器指令的对照表一一翻译过来，汇编过程有汇编器as完成。经汇编之后，产生目标文件(与可执行文件格式几乎一样)xxx.o(Windows下)、xxx.obj(Linux下)。

**4）链接**(.o +(其他.o)+ .lib  + .dll到.exe)

将不同的源文件产生的目标文件进行链接，从而形成一个可以执行的程序。链接分为静态链接和动态链接：

1、静态链接：.lib     (.a)

函数和数据被编译进一个二进制文件。在使用**静态库(.lib)**的情况下，在编译链接可执行文件时，**链接器从库中复制这些函数和数据**并把它们**和应用程序的其它模块组合起来创建最终的可执行文件。**

空间浪费：因为每个可执行程序中对所有需要的目标文件都要有一份副本，所以如果多个程序对同一个目标文件都有依赖，会出现同一个目标文件都在内存存在多个副本；

更新困难：每当库函数的代码修改了，这个时候就需要重新进行编译链接形成可执行程序。

运行速度快：但是**静态链接的优点就是，在可执行程序中已经具备了所有执行程序所需要的任何东西，在执行的时候运行速度快。**

2、动态链接：.dll    (.so)

动态链接的基本思想是把程序按照模块拆分成各个相对独立部分，**在程序运行时才将它们链接在一起形成一个完整的程序**，而不是像静态链接一样把所有程序模块都链接成一个单独的可执行文件。

共享库：就是即使需要每个程序都依赖同一个库，但是该库不会像静态链接那样在内存中存在多分，副本，而是这多个程序在执行时共享同一份副本；

更新方便：更新时只需要替换原来的目标文件，而无需将所有的程序再重新链接一遍。当程序下一次运行时，新版本的目标文件会被自动加载到内存并且链接起来，程序就完成了升级的目标。

性能损耗：因为把链接推迟到了程序运行时，所以每次执行程序都需要进行链接，所以性能会有一定损失。

### 2.请你来回答一下include头文件的顺序以及双引号””和尖括号<>的区别？

双引号与尖括号的区别：
编译器预处理阶段查找头文件的路径不一样。

对于使用**双引号**的头文件，查找头文件路径的顺序为：

- 1.当前头文件目录

- 2.编译器设置的头文件路径（编译器可使用-I显式指定搜索路径）

  - 如果想在指定路径下检索头文件，可加选项-I。如/home/Desktop目录下有个头文件local1.h，在编译包含local1.h的test.c文件时，可用：

  - ```shell
    gcc test.c -o test -I /root/Desktop
    ```

- 3.系统变量CPLUS_INCLUDE_PATH/C_INCLUDE_PATH指定的头文件路径

  - 即标准的系统头文件路径



对于使用**尖括号**包含的头文件，查找头文件的路径顺序为：（少了查找当前头文件目录这一步）

- 1.编译器设置的头文件路径（编译器可使用-I显式指定搜索路径）
- 2.系统变量CPLUS_INCLUDE_PATH/C_INCLUDE_PATH指定的头文件路径

### 3.请你回答一下malloc的原理，另外brk系统调用和mmap系统调用的作用分别是什么？

https://www.cnblogs.com/zpcoding/p/10808969.html

https://www.cnblogs.com/dongzhiquan/p/5621906.html

#### 1.Linux的虚拟内存管理有几个关键概念：

1、每个进程都有独立的虚拟地址空间，进程访问的虚拟地址并不是真正的物理地址；
2、虚拟地址可通过每个进程上的页表(在每个进程的内核虚拟地址空间)与物理地址进行映射，获得真正物理地址；
3、如果虚拟地址对应物理地址不在物理内存中，则产生缺页中断，真正分配物理地址，同时更新进程的页表；如果此时物理内存已耗尽，则根据内存替换算法淘汰部分页面至物理磁盘中。

#### 2.Malloc原理：

- 1）当开辟的空间**小于 128K** 时，调用 brk（）函数，malloc 的底层实现是系统调用函数 brk（），其主要移动指针 _enddata(此时的 _enddata 指的是 Linux 地址空间中堆段的末尾地址，不是数据段的末尾地址)
  - brk 是将数据段（.data）的最高地址指针 _edata 往高地址推
    - 也可以理解为在狭义heap上分配，狭义heap跟.data是紧挨着的。`_edata`指针一开始就指向了.date的结尾或者说heap的开始。之后往heap里加东西，`_edata`就代表了堆的顶部
  - 会产生内存碎片（即被释放的内存，是可利用内存空间），**内存碎片的虚拟地址与物理地址之间的映射是保留的，可以方便的重用，再使用时不会产生缺页中断了，因为映射保留**。当然也有弊端，碎片太多时，想分配大内存就没法了，因为可用空间（即各个碎片）不连续。
    - 举例：假设heap是10MB，我用new依次从这个堆里得到了3块内存块，分别是：1M、2M和7M。
      假设这三块内存是连续的。那么如果你把1M和7M的块delete掉，那么这个heap里就有了8M的剩余空间，但是这个8M是不连续的：1个1M的块和1个7M的块，两者被那个2M给分隔开了。
      这时候如果你要申请一个连续的8M内存区，虽然这个heap有8M的剩余空间，但却没法给你。原因就是这个碎片（其实碎片是1M和7M，因为它俩无法被使用）。**当然该例子不恰当，因为这种M量级的内存分配不可能是brk。brk只能分配128K以内的内存。**
- 2）当开辟的空间**大于 128K** 时，mmap（）系统调用函数来在虚拟地址空间中（堆和栈中间，称为**“文件映射区域”**的地方）找一块空间来开辟。
  - mmap 是在进程的虚拟地址空间中（堆和栈中间，称为“文件映射区域”的地方）找一块空闲的虚拟内存。
  - 没有内存碎片

**这两种方式分配的都是虚拟内存，没有分配物理内存。在第一次访问已分配的虚拟地址空间的时候，发生缺页中断，操作系统负责分配物理内存，然后建立虚拟内存和物理内存之间的映射关系。**

#### 3.实例：

跟我们常用的内存模型，刚好上下反了过来，不过高低地址没有错

![image.png](https://i.loli.net/2020/10/19/TPVexoE6Lahv4Uz.png)

- (1)进程启动的时候，其（虚拟）内存空间的初始布局如图1所示
- (2)进程调用A=malloc(30K)以后，内存空间如图2：
  - malloc函数会调用brk系统调用，将_edata指针往高地址推30K，就完成虚拟内存分配
  - 事实是：_edata+30K只是完成虚拟地址的分配，A这块内存现在还是没有物理页与之对应的，等到进程第一次读写A这块内存的时候，发生缺页中断，这个时候，内核才分配A这块内存对应的物理页。也就是说，如果用malloc分配了A这块内容，然后从来不访问它，那么，A对应的物理页是不会被分配的。
- (3)进程调用B=malloc(40K)以后，内存空间如图3
  - brk分配的内存需要等到高地址内存释放以后才能释放（例如，在B释放之前，A是不可能释放的，因为只有一个_edata 指针，这就是内存碎片产生的原因，什么时候紧缩看后文），而mmap分配的内存可以单独释放。

![image.png](https://i.loli.net/2020/10/19/dGPF9C6m1rsKXQw.png)

- (4)进程调用C=malloc(200K)以后，内存空间如图4
  - 默认情况下，malloc函数分配内存，如果请求内存大于128K（可由M_MMAP_THRESHOLD选项调节），那就不是去推_edata指针了，而是利用mmap系统调用，从堆和栈的中间（文件映射区域）分配一块虚拟内存
- (5)进程调用D=malloc(100K)以后，内存空间如图5
- (6)进程调用free(C)以后，**C对应的虚拟内存和物理内存一起释放**

![image.png](https://i.loli.net/2020/10/19/CVOXtH589Je3zix.png)

- (7)进程调用free(B)以后，如图7所示
  - B对应的虚拟内存和物理内存都没有释放，因为只有一个_edata指针，如果往回推，那么D这块内存怎么办呢？当然，B这块内存，是可以重用的，如果这个时候再来一个40K的请求，那么malloc很可能就把B这块内存返回回去了
- (8)进程调用free(D)以后，如图8所示
  - B和D连接起来，变成一块140K的空闲内存
- (9)默认情况下的内**存紧缩操作：**
  - **当最高地址空间的空闲内存超过128K（可由M_TRIM_THRESHOLD选项调节）时，执行内存紧缩操作（trim）**。在上一个步骤free(D)的时候，发现最高地址空闲内存超过128K，于是内存紧缩，变成图9所示

#### 4.既然堆内内存brk和sbrk不能直接释放，为什么不全部使用 mmap 来分配，munmap直接释放呢？

问题：既然堆内碎片不能直接释放，导致疑似“内存泄露”问题，为什么 malloc 不全部使用 mmap 来实现呢(mmap分配的内存可以会通过 munmap 进行 free ，实现真正释放)？而是仅仅对于大于 128k 的大块内存才使用 mmap ？

**答：**

其实，进程向 OS 申请和释放地址空间的接口 sbrk/mmap/munmap 都是系统调用，频繁调用系统调用都比较消耗系统资源的。并且， mmap 申请的内存被 munmap 后，重新申请会产生更多的缺页中断。例如使用 mmap 分配 1M 空间，第一次调用产生了大量缺页中断 (1M/4K 次 ) ，当munmap 后再次分配 1M 空间，会再次产生大量缺页中断。**缺页中断是内核行为，会导致内核态CPU消耗较大。**另外，如果使用 mmap 分配小内存，会导致地址空间的分片更多，内核的管理负担更大。

**同时堆是一个连续空间，并且堆内碎片由于没有归还 OS ，如果可重用碎片，再次访问该内存很可能不需产生任何系统调用和缺页中断，这将大大降低 CPU 的消耗**。 因此， glibc（GNU发布的libc库，即c运行库） 的 malloc 实现中，充分考虑了 sbrk 和 mmap 行为上的差异及优缺点，默认分配大块内存 (128k) 才使用 mmap 获得地址空间，也可通过 mallopt(M_MMAP_THRESHOLD, <SIZE>) 来修改这个临界值。

**两种方法的总结：**

- 1.为什么要用brk
  - 首先要知道无论是brk还是mmap，分配内存，在第一次使用时会有缺页中断的，缺页中断是很费CPU的。
  - **假如可以用mmap分配内存64K**，被munmap 后，虚拟内存和物理内存是真的被回收了。
    - 实际上mmap只能分配大于128K的，它没法分配这么小的内存
  - 如果回收后，此时，又有164K的内存分配，那么第一次使用时又得一遍即64K/4K=16次缺页中断，这成本好高的。
  - 所以呢，brk分配的64K内存，回收时没有真的还给系统。它的虚拟空间和物理空间还有映射关系都还是保留的。当再次有跟碎片的大小合适的内存分配时，可重用碎片，再次访问该内存很可能不需产生任何系统调用和缺页中断，这将大大降低 CPU 的消耗
  - 其次如果使用 mmap 分配小内存，会导致地址空间的分片更多，内核的管理负担更大。
  - 所以小内存分配，要用brk
- 2.为什么要用mmp
  - **假如使用brk可以分配1M内存**，那么被回收后，会直接产生1M的内存碎片，这没法容忍。
    - 实际上brk只能分配128K以内的内存
  - 所以大内存分配，要用mmp

### 4.请你说一说C++的内存管理是怎样的？



## 六、STL

## 七、C++11新特性