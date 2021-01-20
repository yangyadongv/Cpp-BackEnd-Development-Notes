## 0.导读

### 0.1术语

### 约定

- 声明式（declaration）

  - ```c++
    extern int x;//这是变量声明，x是外部文件的
    int x;//这是变量声明+定义
    
    std::size_t numDigits(int number);//函数声明
    
    class widgt;//类声明
    wight yyd;//这是对象声明+定义+初始化
    
    template<typename T>//模板声明
    class GraphNode;
    ```

    

  - 签名式：从函数的声明式中可以得知它的签名式。没啥用

- 定义式（definition）

- 初始化（initialization）

  - explicit关键字

  - ```c++
    //如果构造函数没有explicit关键字
    class YYD{
        public:
        int a;
        YYD(int a=0):a(a){};
    }
    //那么可以
    YYD test0;
    YYD test1(1);
    YYD test2=1;//有了explict关键字后，这个方式不可以了
    ```

  - ```c++
    //同理
    void dosomething(YYD obj){}
    //如果没有explicit关键字，将允许
    dosomething(1);//这个整数1会自动隐式转换构造为一个YYD对象
    ```

  - **写构造函数时，能加上explicit的就加上，用explicit可以禁止一些非预期的类型转换**

### 复制与赋值

- 复制构造函数：“以同型对象初始化自我对象”
- 赋值操作符：“从另一个同型对象中拷贝其值到自我对象”

**看到赋值符号“=”时要小心，可能是复制也可能是赋值**

```c++
class Widget{
    public:
    Widget();//默认构造函数
    Widget(const Widget& rhs);//复制构造函数
    Widget& operator=(const Widget&rhs);//赋值操作符
    ...
};
Widget w1;//默认构造函数
Widget w2(w1);//复制构造函数
w1=w2;//赋值操作符。没有新对象被定义，就不会有构造函数出现，那么肯定是赋值操作

Widget w3=w2;//复制构造函数。有新对象被定义，那么一定得是一个构造函数，所以是复制构造函数
```

###  不明确行为

```c++
//1
int* p=nullptr;
std::cout<<*p;//对一个nullptr取值会导致不明确行为

//2
char name[]="Darla";
char c=name[10];//访问一个无效的索引，会导致不明确行为
```

应该全力避开不明确行为

### 0.2命名习惯

通常将lhs和rhs作为二元操作符的两个参数名称

```c++
//例如将乘法运算符重载为非成员函数:
const Rational operator* (const Rational& lhs,const Rational &rhs);

//对于成员函数，左侧实参由this指针表现出来,所以有时单独使用rhs
//例如重载赋值操作符
Widget& operator=(cosnt Widget& rhs);

```

指向T型对象的指针命名为pt，意思是pointer to T

```c++
Widget* pw;//pw="ptr to Widget"

class Airplane;
Airplane* pa;//pa="ptr to Airplane"

class GameCharacter;
GameCharacter* pgc;//pgc="ptr to GameCharacter"
```

### 0.3关于线程

### 0.4 TR1和Boost

- TR1("Technical Report 1")
  - 是一份规范，描述加入C++标准程序库的诸多新机能
- Boost
  - Boost是一个组织/网站，提供可移植、同僚复审、源码开放的c++程序库

## 1.让自己习惯C++

### 条款1：视C++为一个语言联邦

**C++高效编程守则视状况而变化，取决于你使用C++的哪一部分**

4个次语言

- C	
  - 区块、语句、预处理器、内置数据类型、数组、指针等
- Object-Oriented C++
  - 类、构造函数、析构函数、抽象、封装、继承、多态、虚函数（动态绑定）
- Template C++
  - 模板相关
- STL
  - STL是个template程序库，包括 容器、迭代器、算法、函数对象等

### 条款2：尽量以const,enum,inline替换#define（没懂）

也就是尽量多使用编译器，而不是预处理器

```c++
#define ASPECT_RATIO 1.653//不要这样

const double AspectRatio=1.653//尽量这样
```

### 条款3：尽可能使用const

#### 1.const与指针

```c++
char greeting[]="Hello";
char* p=greeting;//non-const pointer,non-const data
const char* p=greeting;//non-const pointer,const data
char* const p=greeting;//const pointer,non-const data
const char* const p=greeting;//const pointer,const data
```

- **const出现在星号左边，表示被指物是常量**

  - ```c++
    const char* p;
    //或者
    char const* p;//两个效果一致，是一样的
    ```

    

  - 不可以通过指针修改指向的内容，但是指针可以指向其他地方

- **const出现在星号右边，表示指针自身时常量**

  - 可以通过指针修改指向的内容，但是指针不可以指向其他地方

- const出现在星号两边，表示被指物和指针两者都是常量

  - 不可以通过指针修改指向的内容，指针也不可以指向其他地方

**注意：**

**指向“常量”的指针不代表它所指向的内容一定是常量**，只是代表不能通过解引用符（操作符*）来改变它所指向的内容。

```c++
int num_a = 1;
const int* p_a = &num_a; //底层const
//*p_a = 2; //错误，指向“常量”的指针不能改变所指的对象
num_a = 2;
cout << *p_a << endl;//2
```



#### 2.STL迭代器与指针的关系

- 声明迭代器为const就像声明指针为const一样（即声明一个T* cosnt指针）。表示这个迭代器不得指向其他的东西，但它所指的东西的值是可以改动的

  - ```c++
    std::vector<int> vec;
    const std::vector<int>::iterator iter=vec.begin();
    *iter=10;//没问题，改变iter所指物
    iter++;//错误，iter是const
    ```

- 如果希望迭代器所指的东西不可被改动（即模拟const T*指针），需要的是cosnt_iterator

  - ```c++
    std::vector<int> vec;
    std::vector<int>::const_iterator cIter=vec.begin();
    *cIter=10;//错误! *CIter是const
    ++CIter;//没问题，改变cIter
    ```

- 如果想要模拟const T* cosnt指针

  - ```c++
    std::vector<int> vec;
    const std::vector<int>::const_iterator ccIter=vec.begin();
    *ccIter=10;//错误！
    ++ccIter;//错误！
    ```

#### 3.让函数返回const T类型比返回T类型好的多

```c++
class Rational{...};
Ration operator*(const Rational& lhs,const Rational& rhs);//不建议
const Rational operator* (const Rational& lhs,const Rational& rhs);//十分建议

//原因：
Rational a,b,c;
if(a*b=c){..}//程序员可能低级失误少打了一个等号
//程序员有可能实现这样的暴行，如果返回值是const，那么完全可以杜绝这种风险
```

**函数返回值可以不可以被修改**？

```c++
//关于这个返回值可不可以被修改，举两个例子
Rational func1(){
    return Rational();
}
int func2(){
    return 1;
}
int main(){
    Rational a;
    func1()=a;//可以通过编译
    func2()=2;//错误，提示表达式必须是可修改的左值
}
```

所以：**如果函数的返回类型是个内置类型，那么改动函数返回值从来就不合法**

#### 4.const成员函数

用const成员函数的两个好处

1. 可以从声明看出来，哪个函数可以改动对象内容，而哪个函数不行
2. const对象只能使用const成员函数

```c++
class TextBlock{
public:
    ...
    const char& operator[](std::size_t position) const{
        return text[position];
    }
    
    char& operator[](std::size_t position){
        return text[position];
    }
private:
    std::string text;
}

//使用示例
TextBlock tb("Hello");
std::cout<<tb[0];//调用non-const TextBlock::operator[]

const TextBlock ctb("World");
std::cout<<ctb[0];//调用const TextBlock::operator[]

void print(const TextBlock& ctb)//ctb是const
{
    std::cout<<ctb[0];//调用const TextBlock::operator[]
}

std::cout<<tb[0];//没问题，读一个non-const char&
tb[0]='x';//没问题，写一个non-const char&
std::cout<<ctb[0];//没问题，读一个const char&
tb[0]='x';//错误，写一个const char&
```

#### 5.mutable关键字（可变的）

对于const成员函数，所有编译器都认定const成员函数不能更改对象的任何成员变量（static成员除外）。

当数据成员加上mutable关键字后，意味着这些数据成员可能总是会被更改，即使在const成员函数内。也就是编译器将允许const成员函数修改mutalbe声明的变量。

```c++
class CTextBlock{
public:
	std::size_t length() const;//const成员函数    
private:
    char* pText;
    mutable std::size_t textLength;
    mutable bool lengthIsValid;
};

std::size_t CTextBlock::length()const{
    if(!lengthIsValid){
        textLength=std::strlen(pText);
        lengthIsValid=true;//要是textLength和lengthIsValid变量没加mutable关键字，编译器绝对不会允许const成员函数来修改数据成员
    }
}
```

#### 6.避免const和non-const重载的成员函数之间的代码冗余

例如：

```c++
class TextBlock{
public:
    const char& operator[](std::size_t position) const{//const
        ...
        ...
        return text[position]; 
    }
    
    char& operator[](std::size_t position){//non-const，两份代码高度相似，现在就出现了重载的代码冗余
        ...
        ...
        return text[position];
    }
private:
    std::string text;
}
```

该做的事是实现一个operator[]的机能一次并使用它两次。

**避免代码重复的两个方案：**

- 1.可以将相同的代码移到另一个成员函数（往往是priavate)，并令两个版本的operator[]调用它
- 2.常量性转除
  - 一般来说是不赞同这种做法的，但是在这种高度相似的情况下，仅仅是返回值多个const而已，这种做法是安全的

**实现常量性转除的两个方案**

- 1.用const版本调用non-const版本（非常不建议，或者说是错的）
  - 首先，const版本承诺不修改对象内的状态，但是在其中调用non-const函数就有了修改对象的风险，编译器不会允许，
  - 或者可以通过一些手段，比如使用const_cast将*this身上的const性质解放掉，就可以通过编译了，但是还是如之前所说的，在const成员函数中强行调用了non-const函数，这是个高风险的事情。
- 2.用non-const版本调用const版本（完全建议）
  - non-const成员函数里，调用const成员函数完全没有数据被修改的风险。

```c++
class TextBlock{
public:
    const char& operator[](std::size_t position) const{
        ...
        return text[position]
    }
    
    char& operator[](std::size_t position){
        ...
        return const_cast<char&>(
        			static_cast<const TextBlock&>(*this)[position]
        )
    }
}
//解读一下代码
//第一次类型转换
static_cast用于强迫隐式转换，比如把non-const对象变成const对象
    
*this本身是TextBlock&型，static_cast<const TextBlock&>(*this)就强行把它变成了const TextBlock&型。//转换*this的类型，这使得接下来调用operator[]时调用的是const版本
//第二次类型转换
const_cast用于讲对象的const性质解除
    
static_cast<const TextBlock&>(*this)[position]将返回const char&类型的数据， const_cast<char&>再作用一下，使得const char&的返回数据变为char&型
```

#### 总结

- 将某些东西声明为const可帮助编译器侦测处错误的用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体
  - 对应1-3
- 编译器强制实施bitwise constness，编写代码时可以通过关键字mutable来实现概念上的const，也就是允许const成员函数修改某些成员变量
  - 对应4-5
- 当const和non-const成员函数有着实质等价的代码实现时，令non-const版本调用const版本可以避免代码重复。当然需要类型转换的帮助。
  - 对应6

### 条款4：确定对象被使用前已先被初始化

#### 1.内置类型一定要手工初始化

- 对于内置类型（int，int*...)等，必须手工完成初始化。
- 对于内置类型外的任何东西，初始化的责任落在构造函数身上。
  - 规则很简单，确保每一个构造函数都将对象的每一个成员初始化

#### 2.赋值（assignment)与初始化(initialization)

```c++
class PhoneNumber{...};
class ABEntry{
public:
    ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones);
private:
    std::string theName;
    std::string theAddress;
    std::list<PhoneNumber> thePhones;
    int numTimesConsulted;
};
ABEntry::ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones)
{
    theName=name;//这些都是赋值，而不是初始化
    theAddress=address;
    thePhones=phones;
    numTimesConsulted=0;
}
```

```c++
//更合适的写法应该是采用成员初值列。（初始化列表）
ABEntry::ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones)
:theName(name),//现在，这些都是初始化(initializations)
 theAddress(address),
 thePhones(phones),
 numTimesConsulted(0)
 {   }//现在构造函数本体不必有任何动作
```

- C++规定，对象的成员变量的初始化动作发生在进入构造函数本体之前。

- 在赋值的那个写法中，首先调用default构造函数为theName,theAddress,thePhones设初值，然后立刻再对它们赋予新值。default构造函数赋予的值都浪费了。

- 在初始化列表这种写法中，theName以name为初值进行复制构造，theAddress以address为初值进行复制构造，thePhones以phones为初值进行复制构造。

- 方法一（写在大括号里）：先default构造+再赋值

- 方法二（写在初始化列表里）：只有一次复制构造

  - 虽然对于基本内置类型而言，两种方法的成本相同。但是对于自定义的类，那方法二就高效的多了。

  - 甚至列表初始化可以这样写：

    - ```c++
      ABEntry::ABEntry()
      	:theName(),   //之前是用形参name复制构造数据成员theName，现在是调用string类的默认构造函数来构造数据成员theName
      	 theAddress(),
      	 thePhones(),
      	 numTimesConsulted(0)//它是基本类型int，显示初始化为0
           {}
      ```

  - 对于基本内置类型虽然在大括号里或者在初始化列表里成本相同，但是一旦该数据成员是const或者references，它们就一定需要初值，而不能被赋值。也就是这种情况只能在初始化列表里

- 想避免这些麻烦的最优做法就是：

  - **初始化列表里列出所有的成员变量，不让编译器自动默认构造**
  - **总是使用成员初值列，而不用大括号**

#### 3.成员初始化次序

两个规则：

- 1.base classes更早于其derived classes被初始化
- 2.class的成员变量总是以其声明次序被初始化

举个例子

```c++
class PhoneNumber{...};
class ABEntry{
public:
    ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones);
private:
    std::string theName;
    std::string theAddress;
    std::list<PhoneNumber> thePhones;
    int numTimesConsulted;
};

ABEntry::ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones)
:theName(name),//现在，这些都是初始化(initializations)
 theAddress(address),
 thePhones(phones),
 numTimesConsulted(0)
 {   }//现在构造函数本体不必有任何动作
```

- class的成员变量总是以其声明次序被初始化：
  - 也就是说顺序是theName、theAddress、thePhones、numTimesConsulted。不论他们在初始化列表里以怎样地次序被初始化，都是按照声明的次序来的。
- **所以建议在初始化列表中的初始化顺序与变量声明的次序保持一致**。以避免晦涩的错误，例如：初始化array时需要指定大小，因此代表大小的那个成语变量必须先有初值。

#### 4.“不同编译单元内定义之non-local static对象”的初始化次序

**static对象**

- 就是其寿命从被构造出来直到程序结束为止
- 因此，**在stack上的和在heap上的对象都不算static对象**
- **static对象包括：global对象，定义于namespace作用域内的对象，[在class内、在函数内（这种叫local static对象）、以及在file作用域内被声明为static的对象]**
- **local static对象：函数内的static对象就称为local static对象（因为它们对函数而言是local）**
- **non-local static对象：其余全部，包括：global对象，定义于namespace作用域内的对象，[在class内或者在file作用域内的被声明为static的对象]**
- 程序结束时static对象会被自动销毁，也就是它们的析构函数会在main()结束时被自动调用

**编译单元**

- 是指产出单一目标文件的那些源码。基本上它是**单一源码文件**加上**其所含入的头文件**

**现在的问题**

两个cpp文件，每个内都有一个non-local static对象。

例如:

```c++
//first.cpp
class FileSystem{
public:
    ...
    std::size_t numDisks() const;
    ...
};
extern FileSystem tfs;//这是个global对象，所以是non-local static对象

//second.cpp
class Directory{
public:
    Directory(params);
    ...
};
Directory::Directory(params){
    ...
    std::size_t disks=tfs.numDisks();//使用first.cpp中的tfs对象
}
Direcory tempDir(params);//这也是个global对象，所以是non-local static对象
```

- 现在初始化次序的重要性出现了，要保证tfs能在tempDir之前被初始化，然而c++做不到
- 解决方案是：
  - 将每个non-local static对象搬到自己的专属函数内（让对象在函数内声明为static），然后返回一个指向它的引用。
  - 其实也就是把两个non-local static对象，变为两个local static对象
- **因为C++保证，函数内的local static对象会在 “该函数被调用期间” “首次遇上该对象之定义式”时被初始化**

解决后的代码是：

```c++
//first.cpp
class FileSystem{
public:
    ...
    std::size_t numDisks() const;
    ...
};
FileSystem& tfs(){//之前是extern FileSystem tfs;
    static FileSystem fs;
    return fs;
}

//second.cpp
class Directory{
public:
    Directory(params);
    ...
};
Directory::Directory(params){
    ...
    std::size_t disks=tfs().numDisks();//之前是tfs.numDisks();
}
Directory& tempDir(params){//之前是Direcory tempDir(params);
    static Directory td(params);
    return td;
}
```

#### 5.总结

- 为内置型对象进行手工初始化，因为C++不保证初始化它们
  - 对应1
- 构造函数最好使用成员初值列（初始化列表），而不要在构造函数本题内使用赋值操作。初值列列出的成员变量，其排列次序应该和它们在class中的声明次序相同
  - 对应2-3
- 为免除“跨编译单元之初始化次序”问题，请以loacal static对象替换non-local static对象
  - 对应4

## 2.构造/析构/赋值运算

### 条款05：了解C++默默编写并调用哪些函数

#### 1.编译器做了哪些事

如果你没有定义，那么编译器将声明一个编译器版本的

- 复制构造函数
- 重载赋值操作符
- 析构函数（non-virtual的，除非这个class的基类自身声明有virtual析构函数）
- 默认构造函数（前提是你没有定义任何一个构造函数）
- **上边这些都是public且inline的**

因此，如果你写了一行

```c++
class Empty{};
```

相当于写了

```c++
class Empty{
public:
    Empty(){...}//默认构造函数
    Empty(const Empty& rhs){...}//复制构造函数
    ~Empty(){...}//析构函数
    Empty& operator=(const Empty& rhs);//赋值操作符
}
```

#### 2.编译器拒绝生成赋值操作符的情况

举个例子：

```c++
template<class T>
class NamedObject{
public:
    NamedObject(std::string& name,const T& value);
private:
    std::string& nameValue;//引用
    const T objectValue;//const
}
```

```c++
std::string newDog("Person");
std::string oldDog("Satch");

NamedObject<int> p(newDog,2);
NamedObject<int> s(oldDog,36);

p=s;//这个操作，第一修改引用，第二修改const，这是绝对不被允许的
```

- 所以在这种情况下（成员类型有引用或者const等)，编译器选择不生成默认的赋值操作符函数
  - 如果你需要一个赋值操作符，那么自己编写，并且需要合法（比如你也不可以去修改引用或者const）
- 另一种情况是，当base classes将它的赋值操作符声明为private时，编译器将拒绝为其derived classes生成一个默认的赋值操作符。
  - 因为编译器会假想derived classes所生的赋值操作符可以处理base class成分，然而现实不允许，所以编译器就不生成了

#### 3.总结

- 编译器暗自地为class创建: 默认构造函数、复制构造函数、赋值操作符、析构函数

### 条款06： 若不想使用编译器自动生成的函数，就该明确拒绝

如果你定义的类，完全就不该存在两份相同的对象，那么就应该明确的拒绝复制构造函数和赋值操作符。因为你无法避免别人使用它们。

- 做法1：**将复制构造函数和赋值操作符声明为private，并且只有声明，而没有实现部分。**

  - 编译器一看你有了这两个函数声明，那么就不生成默认的了。

  - ```c++
    class HomeForSale{
    public:
        ...
    private:
        ...
        HomeForsale(const HomeForSale&);//只有声明，故意不去实现它
        HomeForsale& operator=(const HomeForSale&);//只有声明，故意不去实现它
    }
    ```

  - 有了上述class定义，当客户企图拷贝HomeForSale对象，编译器会报错；如果不慎在成员函数或者friend函数中使用了这两个，那么连接器将会报错。

- 做法2：其实跟上边是一回事，只是更加先进

  - ```c++
    class Uncopyable{
    protected:
        Uncopyable(){};
        ~Uncopyable(){};
    private:
        Uncopyable(const Uncoyable&);//基类的复制构造函数
        Uncopyable& operator=(const Uncopyalbe&);//基类的赋值操作符
    };
    
    class HomeForSale: private Uncopyalbe{//它不再需要声明private的复制构造和赋值操作符了
        
    }
    ```

  - 这次不论是客户还是在成员函数或者友元中意图拷贝一份HomeForSale对象，编译器将尝试生成一个赋值构造和赋值操作符，但是**base classe Uncopyalbe将它的复制构造和赋值操作符声明为private了**，编译器将**不再为该派生类HomeForSale生成默认复制或者赋值操作符函数**。

#### 总结

为驳回编译器自动（暗自）提供的机能，可将相应的成员函数声明为priavate并且不予实现。使用像Uncopyable这样的base class也是一种做法。

### 条款07： 为多态基类声明virtual 析构函数（否则当基类指针指向派生类对象时，会仅仅析构基类成员，导致派生类成员资源没被析构，资源泄露）

#### 1.一般来说有继承的基类都应该声明virtual析构函数

当不使用virtual析构函数时的后果

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

当使用virtual析构函数后

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

**C++明确指出，当derived class对象经由一个base class指针被删除，而该base class带着一个non-virtual析构函数，那么结果是未有定义的，通常发生的结果是只有base成分被销毁了，而derived成分没被销毁。这将造成严重的资源泄露**

规则：

- 任何class，只要带有virtual函数，那么它99%的情况下应该有一个virtual的析构函数
- 如果class不含virtual函数，通常表示它并不意图被用作一个base class

#### 2.当class不意图做base class时，一般就不要让它的析构函数是virtual了

举例：

```c++
class Point{//一个二维空间点（2D point）
public:
    Point(int xCoord,int yCoord);
    ~Point();
private:
    int x,y;
}
```

- 假如一个int 32bit，那么一个Point对象就占64bit。
- 一旦虚构函数被声明为virtual，那么每个Point对象将额外包含一个指向当前类的虚表的指针，用以实现运行时多态。因此一个Point对象所占据的空间变大了。
  - 其次，还将影响到代码向其他语言传递（移植）

结论：

- 当class不做base class时，请不要无端将其析构函数（或者其中任何函数）声明为virtual。
- 心得是：**只有当class内至少含有一个virtual函数时，才将该类内析构函数声明为virtual。**

#### 3.不要继承带有“non-virtual析构函数”的class

例如系统自带的：string类、所有STL容器比如vector、list、set、tr1::unordered_map等等。

举例：

string类不含任何virtual函数，但有时候程序员会错误的把它当做base class，这将存在很多潜在的风险

```c++
class SpecialString:public std::string{//馊主意！std::string有个non-virtual析构函数
    ...
};
```

如果程序无意间将一个pointer-to-SpecialString（例如底下的pss）转换为一个pointer-to-string（例如底下的ps），然后将pointer-to-string   delete掉，这就是之前说的derived class对象经由一个base class指针被删除，而该base class带着一个non-virtual析构函数，那么结果是未有定义的。

```c++
SpecialString* pss=new SpecialString("Impending Doom");//一个指向派生类的指针
std::string* ps;//基类指针
ps=pss;//派生类的指针可以隐含转换为基类的指针
delete ps;//该行为未有定义！实际上*ps的SpecialString资源会泄露，因为SpecialString析构函数没被调用
```

很不幸C++没有类似Java中的final classes或者C#中的 sealed classes的禁止派生的机制，所以你只能自行避免这个大坑。

#### 4.有时候令class带一个纯虚（pure virtual）析构函数可能颇为便利

纯虚（pure virtual)函数可以导致一个类为抽象类（abstract classes)——也就是不能被实体化的class。也就是说不能为这种类型创建对象。

例如：

```c++
class AWOV{//AWOV="Abstract w/o Virtuals"
public://带有至少一个纯虚函数的类称为抽象类:（可以有其他的成员函数，也可以有函数实现，但是一旦有一个=0这种形式的纯虚函数，那它就是抽象类）
    virtual ~AWOV()=0;//声明pure virtual析构函数
}
```

注意：

- 纯虚函数，抽象类自己可以实现，也可以不实现。虽然声明里有=0，依然可以有函数实现部分

  - 例如

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

- 抽象类里，可以有非纯虚函数

- 抽象类里，可以有好多纯虚函数，至少有一个纯虚函数。（也就是说至少有一个=0的函数声明）

- 对于一般的纯虚函数，抽象类自己可以实现或者不实现。**但是对于纯虚析构函数，抽象类必须自己附带有函数实现。**

  - ```c++
    class AWOV{//AWOV="Abstract w/o Virtuals"
    public:
        virtual ~AWOV()=0;//声明pure virtual析构函数
    }
    AWOV::~AWOV(){}//pure virtual析构函数的定义
    ```

给base class一个virtual的析构函数这个规则只适用于polymorphic（带多态性质的） base classes身上

#### 5.总结

- polymorphic（带多态性质的）base classes应该声明一个virtual析构函数。如果class带有任何virtual 函数，那它就应该拥有一个virtual析构函数
  - 对应1、3、4
- class的设计目的如果不是作为base classes使用，或不是为了具备多态性，就不应该声明virtual析构函数
  - 对应2

### 条款08：别让异常逃离析构函数（如果析构函数中调用的函数可能产生异常，那么一定要捕获然后选择忽略异常或者结束程序）

- 1.可以在析构函数中抛异常吗？
  - ​    不可以！
    ​    虽然语法上并没错，但会对整体系统带来重大隐患！！
-  2： 那么如何保证析构不抛异常呢？
      2.1）析构里如同构造函数一样，做一些简单的操作。
      2.2）如果异常不可避免，那么直接在析构里捕获异常，不要让异常逃离析构函数！
-  3： 析构里抛异常有什么危害呢？
      阻止异常传递到析构函数外有两个原因，第一能够在异常转递的堆栈辗转开解（stack-unwinding）的过程中，防止terminate被调用。第二它能帮助确保析构函数总能完成我们希望它做的所有事情。

C++并不禁止析构函数吐出异常，但它并不鼓励你这样做。

#### 1.析构函数自己绝对不要吐出异常（但是它里边调用的函数可以吐出异常，并一定要直接在析构里捕获）

```c++
class Widget{
public:
    ...
    ~Widget(){...}//假设这个可能吐出一个异常。这就是析构函数吐出异常
};

void doSomething(){
    std::vector<Widget> v;
    ...
}//v在这里被自动销毁
```

当vector v被销毁时，将会销毁其内含的所有Widget对象。假设v里含有十个Widgets，在析构第一个元素期间，有个异常被抛出，那也应该继续析构其他的九个Widget对象。假设执行第二个Widget的析构函数时，又抛出一个异常。那么此时有两个同时作用的异常，这对C++而言太多了。在两个异常同时存在的情况下，程序若不是结束执行就是导致不明确行为。所以C++不喜欢析构函数吐出异常。

#### 2.如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们或者结束程序

```c++
class DBconnection{
public:
    ...
    static DBconnection create();//这个函数返回DBconnection对象
    
    void close();//失败则抛出异常
}
```

为确保客户不忘记在DBconnection对象上调用close，一个合理的想法是创建一个用来管理DBConnection资源的class，并在其析构函数中调用close函数

```c++
class DBconn{//用来管理DBConnection资源的class，确保close被调用
public:
    ...
    ~DBconn(){
        db.close();//确保DBconnection类的对象db一定会在结束时调用close成员函数
    }
private:
    DBconnection db;
}
```

那么客户就可以这样简单的使用

```c++
{//局部块
    DBConn dbc(DBconnection::create());//建立一个DBconnection对象并交给DBconn对象以便管理
}
//由于dbc是一个局部变量，出这个大括号时自动消亡，将调用DBConn的析构函数，所以一定会调用DBconnection类的close函数
```



只要close成功，一切美好。一旦close失败，抛出异常，那么这种情况下的DBConn析构函数将会传播该异常，也就是将允许异常离开这个析构函数（**异常从析构函数传播出去将导致不明确行为**），这是绝对的大隐患，应该杜绝。**不应该让任何异常离开析构函数**。

#### 3.普通的解决方案（都不能对异常做出反应，只能终止程序或者忽略异常）

- 1.在析构函数里捕获异常，并调用abort结束程序

  - ```c++
    DBConn::~DBConn(){
    	try{
            db.close();
        }
        catch(...){
            制作运转记录，记下对close的调用失败；
                std::abort();//结束程序
        }
            
    }
    ```

    

- 2.在析构函数里捕获异常，并吞下它们而不做处理

  - ```c++
    DBConn::~DBConn(){
        try{db.close();}
        catch(...){
            制作运转记录，记下对close的调用失败；
            //然后什么都不做，吞下异常当做无事发生
        }
    }
    ```

#### 4.如果客户需要对某个操作函数（比如DBconnection::close）运行期间抛出的异常做出反应，那么该函数应该在一个普通函数（比如DBConn::close）中执行，而非在析构函数中执行该操作

也就是说，客户应该手动的调用DBConn::close函数，且在这里边处理DBconnection::close可能产生的异常。而不应该寄希望于析构函数帮他实现。当然这里的析构函数也确实帮他实现了，也就是起了二次保险的作用。但是一旦close真的抛出了异常，他没有自己处理，而是让我们的析构函数选择了结束程序或者吞下异常，那么客户不应该抱怨我们的代码有问题，是他没有自己处理。

```c++
class DBConn{
public:
    ...
    void close(){//在这里边处理异常，而非析构函数中（就算析构函数处理了，也是选择忽略异常或者结束程序）
        db.close();
        closed=true;
    }
    
    ~DBConn(){
        if(!closed){
            try{
                db.close();//客户应该手动的调用close，这里只是双保险的作用
            }
            catch(...){//如果关闭动作失败
                制作运转记录，记下对close的调用失败//记录下来并结束程序或吞下异常。
            }
        }
    }
}
```

#### 5.总结

- 析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉所有类型的异常，然后选择吞下它们（不处理，不传播）或者结束程序
  - 对应1-3
- 如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class应该提供一个普通函数（而非在析构函数中）执行该操作
  - 对应4

### 条款09：绝不在构造和析构过程中调用virtual函数（因为容易造成歧义，此时的virtual函数不会表现virtual特性）

#### 1.构造过程中调用virtual函数的后果

```c++
class Transaction{//所有交易的base class
public:
    Transaction();
    virtual void logTransaction() const;
};

Transaction::Transaction(){
    ...
    logTransaction();//在构造函数内部使用了virtual函数，这将造成很大的误解
}
```

```c++
class BuyTransaction: public Transaction{//derived class
public:
    virtual void logTransaction() const;
    ...
};

class SellTransaction: public Transaction{//derived class
public:
    virtual void logTransaction() const;
    ...
}
```

当执行下边这句时

```c++
BuyTransaction b;
```

由于derived classs对象内的base class成分会在derived class自身成分被构造之前先构造妥当，所以一定是先执行基类的构造函数，再执行派生类构造函数。

所以当执行基类构造函数最后一句logTransaction();时，这个函数调用的是谁的？答案是调用的是基类的，尽管你构造的对象是派生类。base class构造期间virtual函数绝不会下降到derived classes阶层。换句话说**，在base class构造期间，virtual函数不是virtual函数，就是普通的函数。**对象在derived class构造函数开始执行前不会成为一个derived class对象。

#### 2.析构过程中调用virtual函数的后果

相同道理也适用于析构函数。一旦derived class析构函数开始执行，对象内的derived class成员变量便呈现未定义值，所以C++视它们仿佛不再存在。**进入base class析构函数后对象就成为一个base class对象**，而C++的任何部分包括virtual函数、dynamic_casts等等也就同样的看待它。

#### 3.总结

在构造和析构期间不要调用virtual函数，因为这类调用从不下降至derived class（比起当前执行构造函数和析构函数的那层）。换句话说，在base class构造期间，virtual函数不是virtual函数，就是普通的函数。也就是对象在derived class构造函数开始执行前不会成为一个derived class对象，只是一个base class对象。

### 条款10：令operator=返回一个reference to *this

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



关于赋值，你可以把它们写成连锁形式：

```c++
int x,y,z;
x=y=z=15;//赋值连锁形式
```

赋值采用右结合律，所以上述连锁赋值被解析为：

```c++
x=(y=(z=15));
```

15先被赋值给z，然后其结果（更新后的z）再被赋值给y，然后其结果（更新后的y）再被赋值给x。

**应该遵循的协议：**

为了实现上述的这种“连锁赋值”，**赋值操作符必须返回一个reference指向操作符的左侧实参。**

```c++
class Widget{
public:
    ...
    Widget& operator=(const Widget& rhs){//标准赋值形式
        ...
        return *this;//返回左侧对象
    }
}
```

这个协议不仅适用于以上的标准赋值形式，也适用于所有赋值相关运算

```c++
class Widget{
public:
    ...
    Widget& operator+=(const Widget& rhs){//这个协议也适用于+=,-+,*=等运算符
        ...
        return *this;
    }
    
    Widget& operator=(int rhs){//非标准赋值形式，相当于是operator=有两个函数重载
        ...
        return *this
    }
}
```

### 条款11：在operator=中处理“自我赋值”

#### 1.可能发生自我赋值的情形

```c++
//情形1
class Widget{...}
Widget w;
w=w;//明显的赋值给自己
//情形2
a[i]=a[j]//假如i=j，这就是潜在的自我赋值
//情形3
*px=*py;//假如指针指向同一个地方，这也是潜在的自我赋值
//情形4
class Base{...};
class Derived:public Base{...};
void doSomething(const Base& rb,Derived*pd);//rb和*pd可能其实是同一对象，毕竟一个base class的reference或者pointer可以指向同一个derived class对象
```

一旦在赋值时涉及到资源管理的问题，不考虑“自我赋值”就很容易陷入“在停止使用资源之前意外释放了它”的陷阱

#### 2.危险版本（自我赋值时会出问题，且不具备异常安全性）

假设建立一个class用来保存一个指针指向一块动态分配的位图（bitmap）

```c++
class Bitmap{...};
class Widget{
    ...
private:
    Bitmap* pb;//指针，指向一个从heap分配而得的对象
}
```

```c++
//Widget类的operator=实现代码
Widget& Widget::operator=(const Widget& rhs){
    delete pb;//假若是自我赋值，上来就把自己的数据给删了
    pb=new Bitmap(*rhs);//*rhs过去发现数据已经被删了，会出现大问题
    return *this;
}
```

也就是说*this和rhs是同一个对象时，这样的操作会出现极大的问题

#### 3.解决方案1：证同测试（解决了自我赋值的问题，但仍不具备异常安全性）

```c++
Widget& Widget::operator=(const Widget& rhs){
    if(this==&rhs) return *this;//证同测试，如果是自我赋值，将不做任何事
    delete pb;
    pb=new Bitmap(*rhs.pb);
    return *this;
}
```

这个方案可以解决自我赋值时“在停止使用资源之前意外释放了它”的陷阱问题，但仍不具备异常安全性

**所谓异常安全性是指，此时确实确凿了ths和*this不是同一个对象。但此时如果“new Bitmap”导致异常（不论是因为分配时内存不足或者是因为Bitmap的copy构造函数抛出异常），Widget最终会持有一个指针（this->pb指针）指向一块被删除的Bitmap。这样的指针（野指针）非常有害，你无法安全地删除他们，甚至无法安全地读取他们。**

#### 4.解决方案2：调换代码语句顺序，将delete放最后（既解决自我赋值，又具备异常安全）

```c++
Widget& Widget::operator=(const Widget& rhs){
    Bitmap* pOrig=pb;//先记住之前的pb
    pb=new Bitmap(*rhs.pb);
    delete pOrig;//删除原先的pb
    return *this;
}
```

怎么解决的自我赋值就不说了，通俗易懂。看看它怎么解决的异常安全

现在，如果“new Bitmap”抛出异常，pb（及其栖身的那个widget）保持原状。因为会直接抛出异常，pb还是之前的pb，new不会返回nullptr。pb的原来指向的空间也没有被delete，因为new异常了，底下的代码没执行。

这种方案可行，除了效率有点低。

#### 5.解决方案3：使用copy and swap技术

```c++
class Widget{
    ...
    void swap(Widget& rhs);//交换*this和rhs的数据
    ...
};

Widget& Widget::operator=(const Widget& rhs){
    Widget temp(rhs);//copy复制一份rhs，为rhs数据制作一份副本
    swap(temp);//将*this与副本进行数据交换
    return *this;
}
```

或者是

```c++
Widget& Widget::operator=(Widget rhs){//传进来的时候就已经是副本了，直接进行交换。（非传统的operator=的形参写法）
    swap(rhs);
    return *this;
}
```

这俩方案里，更推荐上边那个

#### 6.总结

- 确保当对象自我赋值时operator=有良好行为。其中技术包括比较“来源对象“和”目标对象“的地址、精心周到的语句顺序、以及copy-and-swap
  - 3个技术对应3、4、5
- 确定任何函数如果操作一个以上的对象，而其中对个对象是同一个对象时，其行为依然正确。
  - 主要是指可能发生自我赋值的情形里的情形4

### 条款12：复制对象时勿忘其每个成分(copying函数)

#### 1.自己写复制构造函数或者赋值运算符时，如果类里新加了成员变量，那么要自己记得手动修改复制构造函数或者赋值运算符

考虑一个class用来表现顾客，其中手工写出**copying函数（即复制构造函数和赋值操作符**），使得外界对它们的调用会被志记（logged）下来

```c++
void logCall(const std::string& funcName);//log entry
class Customer{
public:
	...
    Customer(const Customer& rhs);//复制构造函数
    Customer& operator=(const Customer& rhs);//赋值操作符
    ...
private:
    std::string name;
};

Customer::customer(const Customer& rhs):name(rhs.name)//复制rhs的数据成员name
{
    logCall("Customer copy constructor");
}
Customer& Customer::operator=(const Customer& rhs){
    logCall("customer copy assignment operator");
    name=rhs.name;//复制rhs的数据
    return *this;
}
```

此时呢一切都没问题，实际上也确实没问题。但是假如新加入一个成员变量Data lastTransaction

```c++
class Date{...};//日期
class Customer{
public:
	...
    Customer(const Customer& rhs);//复制构造函数
    Customer& operator=(const Customer& rhs);//赋值操作符
    ...
private:
    std::string name;
    Date lastTransaction;
};
```

如果你为类添加了新的成员变量，但是没有在复制构造函数和赋值操作符里同步修改，那么编译器也不会告诉你你漏了一些成分忘记复制了。

**结论很明显**：**如果你为class添加一个成员变量，你必须同时修改复制构造函数和赋值操作符函数（当然也需要修改class的所有构造函数；以及任何非标准形式的operator=，例如其中一个非标准形式operator=(int rhs)，而标准形式是operator=(const Customer& rhs)）**

#### 2.一旦发生继承，会发生最暗中肆虐的潜藏危机

试着考虑

```c++
class PriorityCustomer:public Customer{//优先顾客
public:
    ...
    PriorityCustomer(const PriorityCustomer& rhs);
    PriorityCustomer& operator=(const PriorityCustomer& rhs);
    ...
private:
    int priority;
};
PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs)//派生类复制构造函数实现。没有复制base class成分
     :priority(rhs.priority)
    {
      logcall("PriorityCustomer copy constructor");
    }

PriorityCustomer& PriorityCustomer::operator=(const PriorityCustomer& rhs)//派生类赋值运算符实现.没有复制base class成分
{
    logcall("PriorityCustomer copy assignment operaotr");
    priority=rhs.priority;
    return *this;
}
```

看上去复制构造函数和赋值运算符复制了PriorityCustomer声明的成员变量priority，没什么问题。但仔细想想，每个PriorityCustomer对象内所继承的Customer成员变量（std::string name）却没有被复制。因此，PriorityCustomer对象的Customer成分会被不带实参的Customer构造函数（即default构造函数——必定有一个否则无法通过编译）初始化。default构造函数将针对std::string name和Date lastTransaction执行缺省的初始化动作。

**重点：任何时候只要你承担起为派生类撰写copying函数的任务，你必须记得也要复制其base class成分。那些成分往往是private，你无法直接访问他们，你应该让派生类的copying函数调用相应的base class函数。**

```c++
PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs)//派生类复制构造函数实现
     :priority(rhs.priority),Customer(rhs)      //调用了base class的复制构造函数
    {
      logcall("PriorityCustomer copy constructor");
    }

PriorityCustomer& PriorityCustomer::operator=(const PriorityCustomer& rhs)//派生类赋值运算符实现
{
    logcall("PriorityCustomer copy assignment operaotr");
    Customer::operator=(rhs);//对base class成分进行赋值动作
    priority=rhs.priority;
    return *this;
}
```

#### 3.不要试图通过赋值构造函数来实现赋值操作符，也不要试图通过赋值操作符来实现复制构造函数

如果你发现你的copy构造函数和assignment操作符有大篇幅相近的代码，消除重复代码的做法是，建立一个新的成员函数给两者调用。这样的函数往往是private而且常被命名为init。这个策略可以安全消除copy构造函数和copy assignment操作符之间的代码重复。

#### 4.总结

- copying函数应该确保复制“对象内的所有成员变量”及“所有base class成分”

  - 即（1）复制所有的local成员变量 (2)调用所有base class内的适当的copying 函数。
  - 对应1-2

- 不要尝试以某个copying函数实现另一个copying函数。应该将共同机能放进第三个函数中，并由两个copying函数共同调用。

  - 即不要试图通过赋值构造函数来实现赋值操作符，也不要试图通过赋值操作符来实现复制构造函数。
  - 对应3

  

## 3.资源管理

所谓资源就是，一旦用了它，将来必须还给系统。C++程序中常用的资源有：动态分配内存、文件描述器（file descriptors）、互斥锁（mutex locks）、图形界面中的字型和笔刷、数据库连接、网络sockets。

### 条款13：以对象管理资源（主要是指用智能指针来管理heap-based资源）

#### 1.问题示例

假设使用一个用来模拟投资行为（例如股票、债券等等）的程序库，其中各式各样的投资类型继承自一个root class Investment：

```c++
class Investment{...};//“投资类型”继承体系中的root class
class gupiao: public Investment{...};//派生类
class zhaiquan: public Investment{...};//派生类
```

进一步假设该程序库通过一个工厂函数（factory function),供应我们某特定的Investment对象（股票、债券等等）：

```c++
Investment* createInvestment();//返回指针，指向Investment继承体系内的动态分配对象。调用者有责任删除它，其实应该有参数，才能返回不同类型的对象。这里为了简化，刻意不写参数。
```

createInvestment的调用端使用了函数返回的对象后，有责任删除它。现在考虑某函数f履行该责任

```c++
void f(){
    Investment* pInv=createInvestment();//调用factory函数，获得某特定的Investment对象
    ...
    delete pInv;//释放pInv所指对象
}
```

f函数的做法，看似没问题。但是仔细考虑。

- **假如“..."代码中，有一个过早的return 语句，或者类似的循环中有个continue等。这将跳过delete函数**
- **或者是"..."代码中语句抛出异常，那么将不会执行delete**

**无论delete如何被略过去，那么将泄漏大量内存，不只是内含投资对象的那块内存，还包括那些投资对象内所保存的任何资源。**

#### 2.解决方案

当然你可以谨慎的写代码，来保证“f函数总是会执行其delete语句”，但是一旦代码被别人维护或者修改，那么别人可能无法意识到这个问题，他们可能又会加入过早的return语句或者引发异常。

**因此，单纯依赖“f函数总是会执行其delete语句”是行不通的**

为确保createInvestment返回的资源总是被释放，我们需要将资源放进对象内，当控制流离开f，该对象的析构函数会自动释放那些资源。实际上正是本条款的想法：把资源放进对象内，我们便可依赖c++的“析构函数自动调用机制”确保资源被释放。

许多资源被动态分配于heap内而后被用于单一区块或者函数内，它们应该在控制流离开那个区块或函数时被释放（比如f函数内的delete pInv）。标准程序库提供的auto_ptr正是针对这种形势而设计的特制产品。**auto_ptr是个“类指针（pointer-like）对象”，也就是所谓的“智能指针”，其析构函数自动对其所指对象调用delete。**

下面示范如何使用auto_ptr以避免f函数潜在的资源泄露可能性：

```c++
void f(){
    std::auto_ptr<Investment> pInv(createInvestment());//调用factory函数createInvestment。经由auto_ptr的析构函数自动删除pInv
    ...   //之前是Investment* pInv=createInvestment();
        //之前最后还要 delete pInv;现在不用了。因为智能指针其实是个对象，大括号完了会自动析构。
}
```

这个简单的例子示范”以对象管理资源“的两个关键想法：

- 获得资源后立刻放进管理对象（managing object）内。
  - 以上代码中createInvestment()返回的资源被当做其管理者auto_ptr的初值。实际上“以对象管理资源”的观念常被称为“资源取得时机便是初始化时机”。（Resource Acquisition Is Initialization；RAII）。每一笔资源都在获得的同时立刻被放进管理对象中。（不论是初始化构造一个管理对象或者是先构造好管理对象再赋值）
- 管理对象（managing object）运用析构函数确保资源被释放。
  - 不论控制流如何离开区块，一旦对象被销毁（例如当对象离开作用域）其析构函数自然会被自动调用，于是资源被释放。如果资源释放动作可能导致抛出异常，事情会变得棘手，但运用条款8（别让异常逃离析构函数）可以解决这个问题。

#### 3.关于智能指针auto_ptr（智能指针其实是个对象）

由于auto_ptr被销毁时会自动删除它所指之物，所以一定要注意别让多个auto_ptr同时指向同一对象。如果真的是那样，对象会被删除一次以上，这是一种“未定义行为”。为了预防这个问题，auto_ptrs有一个不寻常的性质：若通过copy构造函数或者assignment操作符复制它们，它们会变成null，而**复制所得的指针将取得资源的唯一拥有权！**

```c++
std::auto_ptr<Investment> pInv1(createInvestment());//pInv1指向createInvestment返回物

std::auto_ptr<Investment> pInv2(pInv1);//现在pInv2指向对象，pInv1被设为null 

pInv1=pInv2;//现在pInv1指向对象，pInv2被设为null
```

这一诡异的复制行为，复加上其底层条件：“受**auto_ptrs”管理的资源必须绝对没有一个以上的auto_ptr同时指向它“**，意味auto_ptrs并非管理动态分配资源的身边利器。举个例子，STL容器要求其元素发挥”正常的“复制行为，因此这些容器容不得auto_ptr；

#### 4.智能指针RCSP

auto_ptr的替代方案是**“引用计数型智慧指针”(reference-counting smart pointer;RCSP)**。所谓RCSP也是个智能指针，持续追踪共有多少对象指向某笔资源，并在无人指向它时自动删除该资源。RCSPs提供的行为类似垃圾回收，不同的是RCSPs无法打破环状引用（例如两个其实已经没被使用的对象彼此互指，因为好像还处在“被使用”状态）。

tr1::shared_ptr就是个RCSP；例如可以这样改进f函数

```c++
void f(){
    ...
    std::tr1::shared_ptr<Investment>  pInv(createInvestment());//调用factory函数createInvestment。
     //之前是std::auto_ptr<Investment> pInv(createInvestment());这是auto_ptr，现在换成shared_ptr了
    ...//一如以往使用pInv，经由shared_ptr析构函数自动删除pInv
}
```

这段代码看似和使用auto_ptr哪个版本相同，但shared_ptrs的复制行为正常多了。

```c++
{
    ...
    std::tr1::shared_ptr<Investment>  pInv1(createInvestment());//pInv1指向createInvestment返回物
    std::tr1::shared_ptr<Investment>  pInv2(pInv1);//pInv2 和 pInv1指向同一个对象
    pInv1=pInv2;//同上，无任何改变
}//pInv1和pInv2被销毁，他们所指的对象也就被自动销毁

/*与auto_ptr的诡异复制行为对比来看，shared_ptr的行为正常多了
std::auto_ptr<Investment> pInv1(createInvestment());//pInv1指向createInvestment返回物

std::auto_ptr<Investment> pInv2(pInv1);//现在pInv2指向对象，pInv1被设为null 

pInv1=pInv2;//现在pInv1指向对象，pInv2被设为null*/
```

由于tr1::shared_ptrs的复制行为“一如预期”，他们可被用于STL容器里做元素，以及其他的auto_ptr不适用的语境上。

#### 5.其他细节

“以对象管理资源”很重要，而auto_ptr和tr1::shared_ptr只是两个例子。

auto_ptr和tr1::shared_ptr两者都在析构函数中做的是delete而不是delete[]动作。那意味在动态分配而得的数组array身上适用auto_ptr或tr1::shared_ptr是个馊主意。（但是它仍然能通过编译）。

```c++
std::auto_ptr<std::string> aps(new std::string[10]);//馊主意!!!

std::tr1::shared_ptr<int> spi(new int[1024]);//馊主意！！！
```

注意：C++和TR1中并没有针对new std::string[10]或者new int[1024]这种动态分配的数组而设计的智能指针。如果你非要用的话，请去看boost中的boost::scoped_arrayhe boost::shared_array classes.

最后一点：尽管适用对象管理资源很好，但是可以在更根源上解决这个问题。因为一切都出于那个工厂函数，`Investment* createInvestment();//返回指针`。这个接口设计的很不好，将在条款18阐述。因为createInvestment返回的“未加工指针”（raw pointer)简直就是资源泄露的死亡邀约，不该如此返回指针。

#### 6.总结

- 为防止资源泄露，请使用RAII对象。（“资源取得时机便是初始化时机”（Resource Acquisition Is Initialization；RAII））他们在构造函数中获得资源并在析构函数中释放资源。
- 两个常被使用的RAII classed分别是tr1::shared_ptr和auto_ptr。前者通常是最佳选择，因为auto_ptr有个很诡异的copying行为。若选择auto_ptr，复制动作会使它（被复制物）指向null。

### 条款14：在自己建立的资源管理类中小心copying行为（自己写资源管理类，而不使用智能指针）

在上一个条款上提出了这样的观念“资源取得时机便是初始化时机”（RAII），并以此作为“资源管理类”的脊柱，也描述了auto_ptr和tr1::shared_ptr如何将这个观念表现在heap-based资源上。然而，并非所有资源都是heap-based的，对于这种资源，auto_ptr和tr1::shared_ptr智能指针往往不适合作为资源掌管者。所以有时需要自己建立资源管理类。

RAII class即可以理解为资源管理类,因为资源管理类都具备这个特性（**“资源在构造期间获得，在析构期间释放”**）

#### 1.自己建立资源管理类示例

```c++
void lock(Mutex* pm);//锁定pm所指的互斥器
void unlock(Mutex* pm);//将互斥器解除锁定
```

为确保绝不会忘记将一个被锁住的Mutex解锁，你可能会希望建立一个class用来管理锁。这样的class的基本结构由**RAII守则**支配，也就是**“资源在构造期间获得，在析构期间释放”**：

```c++
class Lock{
public:
    explicit Lock(Mutex* pm):mutexPtr(pm)
    {
        lock(mutexPtr);//构造期间获得资源。类外函数lock
    }
    
    ~Lock(){
        unlock(mutexPtr);//析构期间释放资源
    }
private:
    Mutex* mutexPtr;
}
```

客户对Lock的用法符合RAII方式:

```c++
Mutex m;//定义你需要的互斥器
...
{//建立一个区块来表示临界区
    Lock ml(&m);
    ...//执行临界区操作
}//区块末尾，自动解除互斥器锁定
```

#### 2.改进版本

上边这么建立似乎没有问题，试考虑如果Lock对象被复制，会发生什么

```c++
Lock ml1(&m);//锁定m
Lock ml2(ml1);//将ml1复制到ml2身上，这会怎样？
```

当一个RAII对象被复制，会发生什么事？大多时候可能有多个选择，常用的是前两个

- **1.禁止复制。（常用）**

  - 很多时候允许RAII对象被复制并不合理。比如这个Lock类被复制就不合理，应该禁用复制动作。

  - 例如：采用条款06的先进方案

    - ```c++
      class Lock:private Uncopyable{//禁止复制
      public:
          ...//还是上边的构造函数、析构函数等
      };
      ```

      

- **2.对底层资源祭出“引用计数法”（reference-count）（常用）**。

  - 有时候我们希望保有资源，直到它的最后一个使用者（某对象）被销毁。这种情况下复制RAII对象时，应该将资源的“被引用数”递增。

  - tr1::shared_ptr便是如此

  - 通常只要内含一个tr1::shared_ptr成员变量，RAII classes便可实现出reference-counting copying行为。如果前述的Lock打算使用reference counting，可以将成员变量`Mutex* mutexPtr`改为 `tr1::shared_ptr<Mutex>`。但是tr1::shared_ptr的默认行为是“当引用次数为0时，删除其所指物”，而我们想要的行为是当引用次数为0时，执行unlock解锁。

  - tr1::shared_ptr允许指定“63

  - ”（deleter），是一个函数或者函数对象，当引用次数为0时调用（auto_ptr不允许自己指定删除器）

  - ```c++
    class Lock{
    public:
        explicit Lock(Mutex* pm):mutexPtr(pm，unlock)//以mutex初始化shared_ptr，并以unlock函数为删除器
        {
            lock(mutexPtr.get());//下一个条款讲get
        }
    private:
        std::tr1::shared_ptr<Mutex> mutexPtr;//使用shared_ptr替换raw pointer
    }
    ```

  - 本例中不再声明析构函数，因为没有必要。**Lock class析构函数**（无论是编译器生成的，还是用户定义的）会**自动调用**non-static成员变量（即本例中的shared_ptr<Mutex> **mutexPtr）的析构函数**。**而mutexPtr的析构函数**会在互斥器mutexPtr的引用次数为0时自动调用tr1::shared_ptr的删除器（本例为unlock）。

  - 解锁这个事不再依赖于Lock对象的析构函数手动释放（手动unlock），而是依赖Lock对象的析构函数自动调用mutexPtr的析构函数。mutexPtr的析构函数会`shared_ptr<Mutex> mutexPtr`当这个成员的计数为0时，自动调用我们指定的删除器，也就是unlock函数。

- **3.复制底部资源（不常用）**

  - 也就是深度拷贝

- **4.转移底部资源的拥有权(不常用）**

  - 某些罕见场合下希望确保永远只有一个RAII对象指向一个未加工资源（raw resource)。即使RAII对象被复制也依然如此。
  - 例如auto_ptr的思想

#### 3.总结

- 复制RAII对象必须一并复制它所管理的资源，所以资源的copying行为决定RAII对象的copying行为
- 普遍而常见的RAII class copying行为是：抑制copying、施行引用计数法（reference counting）。不过其他行为也都可能被实现。



### 条款15：在资源管理类中提供对原始资源的访问

例如:原始资源一般就是资源管理类里的private数据成员。资源管理类是一种封装，避免原始资源资源泄露的方式

```c++
class Font{//(资源管理类)
public:
    explicit Font(FontHandle fh):f(fh){}
    ~Font(){releaseFont(f);}
private:
    FontHandle f;//原始资源(raw resource)
}
```

资源管理类（resource-managing classes）是对抗资源泄露的堡垒。在一个理想的完美世界中，我们可以依赖这样的资源管理类来处理和资源之间的所有互动，而不用玷污双手直接处理原始资源（raw resources）。但是这个世界并不完美，许多API都会直接指涉原始资源。

举个例子，底下的API（daysHeld（）)只接受原始资源。

条款13中的例子，用智能指针auto_ptr或者tr1::shared_ptr保存工厂函数如createInvestment的调用结果。

```c++
std::tr1::shared_ptr<Investment> pInv(createInvestment());//条款13的例子
```

假设希望以某个函数处理Investment对象，例如

```c++
int daysHeld(const Investment* pi);//返回投资天数
```

如果你这样调用它

```c++
int days=daysHeld(pInv);//错误！
```

无法通过编译，因为API `int daysHeld(const Investment* pi);`需要的参数类型是个`Investment*`指针，而你传入的是`shared_ptr<Investment>`对象。所以需要一个函数来将RAII对象（本例中即`shared_ptr<Investment>`对象pInv）转换成其所含之原始资源（即本例中的`Investment*`）

可以通过显式转换或者隐式转换来取得原始资源：

#### 1.显示转换（直接转换成原始指针，或者直接转换成原始资源）

auto_ptr和tr1::shared_ptr都提供了一个get成员函数，用来执行显式转换，也就是它会返回智能指针内部的原始指针（的复件）：

显式转换例子1

```c++
Investment* pi=pInv.get();//显式转换，将`shared_ptr<Investment>`对象pInv内的原始指针返回
int days=daysHeld(pi);
```

显式转换例子2

```c++
class Font{//Fonthandle是原始资源类型
public:
   explicit Font(FontHandle fh):f(fh){}
    ~Font(){releaseFont(f);}
    Fonthandle get() const{return f};//提供资源管理类Font类型显式转换成原始资源FontHanlde类型的接口
private:
    FontHandle f;//原始资源(raw resource)    
}

Font f(getFont());//构建一个资源管理类
Fonthand temp=f.get();//显式转换
```



#### 2.隐式转换（像指针一样访问资源）

几乎所有的智能指针，都重载了指针取值(pointer dereferencing)操作符（**operateor->和operator***)，它们允许隐式转换至原始指针

隐式转换例子1

```c++
class Investment{//Investment继承体系的基类
public:
    bool isTaxFree() const;
    ....
};

Investment* createInvestment();//factory函数，所谓工厂函数，就是用来产生我们需要的资源，放在heap上，并返回一个指针
std::tr1::shraed_ptr<Investment> pil(createInvestment());//令tr1::shared_ptr 管理一笔资源

bool taxable1=!(pil->isTaxFree());//经由operateor->访问资源
...
std::auto_ptr<Investment> pi2(createInvestment());//令auto_ptr管理一笔资源
bool taxable2=!((*pi2).isTaxFree());//经由operator* 访问资源
```

隐式转换例子2

```c++
class Font{
public:
    ...
    explicit Font(FontHandle fh):f(fh){}
    ~Font(){releaseFont(f);}
    operator FontHandle() const {return f;}//隐式转换函数，将资源管理类Font类型隐式转换成原始资源FontHanlde类型
private:
     FontHandle f;//原始资源(raw resource)
}

//某api void changeFontSize(FontHandle f,int newSize);
Font temp(getFont());//构建一个资源管理类对象
int newSize;
//用显式转换的话就得changeFontSize(temp.get(),newSize);
//用隐式转换的话
changeFontSize(temp,newSize);//资源管理类对象temp可隐式转换为原始资源FontHandle
```

```c++
//隐式转换同时存在一些风险
//比如可能不小心在想要拷贝一个Font对象时，不小心创建成了FontHandle
Font f1(getFont());
...
FontHandle f2=f1;//本来想创建一个Font f2，这里手误写错了，会让资源管理类f1隐式转换为原始资源FontHanlde类型，再复制给f2
```

#### 3.总结

- APIs往往要求访问原始资源(raw resources)，所以每一个RAII class应该提供一个“取得其所管理之资源”的办法
- 对原始资源的访问可能经由显式转换或隐式转换。一般而言显式转换比较安全，但隐式转换对客户比较方便。

### 条款16：成对使用new和delete时要采取相同形式（new的是单个对象就用delete，new的是对象数组就得用delete[])

#### 1.new和delete

```c++
std::string* stringArray=new std::string[100];
...
delete stringArray;
```

以上动作有什么错？你的程序行为不明确（未有定义）。stringArray中所含的**100个string对象中的99个可能未被适当的删除，因为它们的析构函数很可能没被调用。**



使用new（通过new动态生成一个对象）时，有两件事情发生：

- 1.内存被分配出来（通过名为operator new的函数）
- 2.针对此内存会有一个（或更多）构造函数被调用

使用delete时，也有两件事情发生：

- 1.针对此内存会有一个（或更多）析构函数被调用
- 2.然后内存被释放（通过名为operator delete的函数）

delete的最大问题就是：

**被删除的内存之内，究竟有多少对象？这个答案决定了有多少个析构函数必须被调用。**

#### 2.delete和delete[]

这个问题可以更简单些：你要删除的那个指针，它指向的究竟是单一对象还是对象数组？这是必不可缺的条件，因为单一对象的内存布局一般情况下与对象数组的内存布局不同。数组所用的内存通常还包括“数组大小”这个记录，以便delete知道需要调用多少次析构函数，而单一对象的内存则没有这笔记录。

所以对着一个指针使用delete，唯一能够让delete知道内存中是否存在“数组大小”这个记录项的方法就是，你来告诉他。

- 使用delete,则认定指针指向单一对象
- 使用delete[],则认定指针指向数组。然后查找“数组大小这个记录项，以确定调用多少次析构函数

```c++
std::string* stringPtr1=new std::string;
std::string* stringPtr2=new std::string[100];
...
delete stirngPtr1;//删除一个对象
delete[] stringPtr2;//删除一个由对象组成的数组
```

对stirngPtr1使用delete[]或者对stringPtr2使用delete，结果都是未有定义。应该完全避免这种情况的出现。

游戏规则很简单：

- 如果你调用new时使用[]，你必须在对应调用delete时也使用[]。
- 如果你调用new时没有使用[]，那么也不该在对应调用delete时使用[]。

#### 3.应该注意的几个常见地方

①

当撰写的class中数据成员有一个指针指向动态内存分配，并提供多个构造函数时，你必须保证在所有构造函数中使用相同形式的new。（要么都加[]，要么都不加）。因为析构函数是惟一的，它不能使用不同形式的delete，它只能使用一种delete。（加[]或者不加）

②

```c++
typedef std::string AddressLines[4];//AddressLines是一个string数组，4个string。

std::string *pal1 = new std::string[4];
std::string *pal2 = new AddressLines;//pal1和pal2是一个意思


delete [] pal1;
//delete pal2; 错误！！！
delete [] pal2;//正确

```

为避免此种错误，尽量不要对数组形式做typedef动作。完全可以将AddressLines定义为

```
typedef vector<string> AddressLines;
```

用STL可以将数组的需求几乎降为0。

#### 4.总结

- 如果你在new表达式中使用[]，必须在相应的delete表达式中也使用[]。
- 如果你在new表达式中不使用[]，一定不要在相应的delete表达式中使用[]。

### 条款17：以独立语句将newed对象置入智能指针（std::tr1::shared_ptr<Widget> pw(new Widget);//单独语句内以智能指针存储newed所得对象）

```c++
int priority();
void processWidget(std::tr1::shared_ptr<Widget> pw,int priority);
```

现在考虑调用processWidget函数，

```c++
//processWidget(new Widget,priority());//该调用方式无法通过编译，因为tr1::shared_ptr的构造函数是explicit的，所以无法隐式的将new的Widget指针隐式转换为一个std::tr1::shared_ptr<Widget>对象。需要手动的进行显式转换。

processWidget(std::tr1::shared_ptr<Widget>(new Widget)),priority());//显式转换即没有问题
```

但是上述调用，将new语句与置入智能指针语句合二为一，将有可能发生资源泄露的情况。



编译器在产出一个processWidget函数调用码之前，需要核算即将被传递的各个实参。`std::tr1::shared_ptr<Widget>(new Widget))`分为两个部分，执行“new Widget"表达式 和 调用tr1::shared_ptr析构函数。第二个部分就是prioriry()函数调用即可。

所以在执行`processWidget(std::tr1::shared_ptr<Widget>(new Widget)),priority());`之前，编译器必须创建代码，做三件事:

- 调用priority()函数
- 执行“new Widget"
- 调用tr1::shared_ptr构造函数

C++编译器以怎样地次序做以上三件事呢？只能确定“new Widget"肯定在调用tr1::shared_ptr构造函数之前，因为这是new Widget的结果是它的实参。但是对于priority()函数什么时候执行，无法得知。priority()函数可能在第一、第二、第三位执行，都有可能。



假如获得以下操作顺序：

- 1.执行“new Widget"
- 2.调用priority()函数
- 3.调用tr1::shared_ptr构造函数

万一对priority()函数调用产生异常，那么“new Widget"返回的指针将会遗失，导致资源泄露，因为它尚未被置入tr1::shared_ptr内，而tr1::shared_ptr使我们防止资源泄露的武器。

所以，在对`processWidget(std::tr1::shared_ptr<Widget>(new Widget)),priority());`调用过程中可能发生资源泄露，因为在“资源被创建”（经由“new Widget"和”资源被转换为资源管理对象“（这里指tr1::shared_ptr）这两个时间点之内可能发生异常干扰。（这里指priority()函数可能异常）



避免这类问题的办法很简单，分两步写就行了

```c++
std::tr1::shared_ptr<Widget> pw(new Widget);//单独语句内以智能指针存储newed所得对象
processwidget(pw,prioriry());//这个调用动作绝不至于造成资源泄露
```

**总结：**

以独立语句将newed对象存储于（置入）智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄露。

## 4.设计与声明

### 条款18：让接口容易被正确使用，不易被误用

### 条款19：设计class犹如设计type

- **新type的对象如果被passed by value（以值传递），意味着什么？**

  - 记住，copy构造函数用来定义一个type的pass-by-value该如何实现

- **你的新type需要什么样的转换？**

  - 如果你希望允许类型T1之物被隐式转换为类型T2之物，就必须在class T1内写一个类型转换函数（operator T2）或者在class T2内写一个non-explicit-one-argument（可被单一实参调用）的构造函数。

    

### 条款20：宁以pass-by-reference-to-const替换pass-by-value （除非是内置类型或者STL的迭代器和函数对象）

#### 1.优点1：高效

缺省情况下，c++以传值的方式传递对象至（或来自）函数。除非你另外指定，否则函数参数都是以实际实参的复件（副本）为初值，而调用端所获得的的亦是函数返回值的一个复件。这些复件（副本）是由对象的copy构造函数产出，这可能使得pass-by-value成为费时的操作。

举例：

```c++
class Person{
public:
    Person();
    virtual ~Person();
    ...
private:
    std::string name;
    std::string address;
};

class Student:public Person{
public:
    Student();
    ~Student();
    ...
private:
    std::string schoolName;
    std::string schoolAddress;
};
```

假设有个函数：

```c++
bool validateStudent(Student s);
```

当以下代码发生时，会有多少的复杂度

```c++
Student plato;
bool platoIsok = validateStudent(plato);
```

Student 的复制构造函数会被调用，以plato为蓝本将形参s初始化。当函数validateStudent执行结束时，形参s的析构函数又会被调用。所以参数传递的成本有“一次Student的复制构造函数，加上一次Student析构函数调用”。更甚者分析，Student对象里有两个string对象，并且Student继承自Person，Person内也有两个String对象，所以这次的pass-by-value的后果是，调用一次Student复制构造函数，一次Person复制构造函数，四次String复制构造函数，当函数返回要析构局部对象时，又会一次Student析构函数，一次Person析构函数，4次string析构函数。所以总体成本是“6次构造函数，6次析构函数”

所以当函数声明为：

```
bool validateStudent(const Student& s);
```

将变得高效的多，没有任何构造函数或析构函数被调用。

#### 2.优点2：避免对象切割问题（类似于基类指针指向派生类对象的意思）

```c++
class Window{
public:
    ...
    std::string name() const;
    virtual void display() const;
};

class WindowWithScrollBars:public Window{
public:
    ...
    virtual void display() const;
};
```

假如有个函数，是pass-by-value

```c++
void printNameAndDisplay(Window w)//对象很可能被切割
{
    std::cout<<w.name);
    w.display();
}
```

当我们调用时，会发生什么问题

```c++
WindowWithScrollBars wwsb;//派生类对象
printNameAndDisplay(wwsb);
```

派生类对象wwsb被传值给形参w时，由于形参w类型是基类而非派生类，实参wwsb是个“派生类对象”的所有特化信息将会被切除。这就是对象切割。形参就完全表现的是个基类了。所以w.display();将显示基类的display函数而非派生类的display函数。



如果采用pass by reference to const的方式传递给形参w

```c++
void printNameAndDisplay(const Window& w)
{
    std::cout<< w.name();
    w.display();
}
```

现在传进来的是派生类，那么就会表现出派生类的特性。（类似于基类指针指向派生类对象）

#### 3.可以使用pass-by-value的情况

一般而言，你可以合理假设“pass-by-value并不昂贵”的唯一对象就是**内置类型**和**STL的迭代器和函数对象**。至于其他任何东西，尽量以pass-by-reference-to-const替换pass-by-value

#### 4.总结

- 尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题
  - 对应1-2
- 以上规则不适用于内置类型，以及STL的迭代器和函数对象。对这二者而言，pass-by-value往往比较适当
  - 对应3

### 条款21：必须返回对象时，别妄想返回其reference

```c++
class Rational{
public:
    Rational(int numerator=0,int denominator=1);
private:
    int n,d;//分子和分母
    friend const Rational operator*(const Rational& lhs,const Rational& rhs);
}
```

operator*返回的是一个对象，如果想返回成引用呢？

#### 1.返回stack空间的对象（错误）

```c++
const Rational& operator*(const Rational&lhs, cosnt Rational&rhs)
{
	Rational result(lhs.n * rhs.n,lhs.d * rhs.d);
    return result;
}
```

这个函数返回一个reference指向result，但result是个local对象，而local对象在函数退出前被销毁了。所以真相是，**任何函数返回一个reference指向某个local对象，都将一败涂地（如果函数返回一个指针指向一个local对象，也是一样）**

#### 2.返回heap空间的对象

```c++
const Rational& operator*(const Rational&lhs, cosnt Rational&rhs)
{
	Rational* result=new Rational(lhs.n * rhs.n,lhs.d * rhs.d);
    return *result;
}
```

这将有个更大的问题，谁该对着被你new出来的对象实施delele。这是一种非常容易导致资源泄露的写法，因为没有人觉得归自己来delelte这个资源。

#### 3.正确写法

一个“必须返回新对象”的函数的正确写法是：就让那个函数返回一个新对象。

```c++
const Rational operator*(const Rational& lhs,const Rational& rhs)
{
    return Rational(lhs.n*rhs.n,lhs.d*rhs.d);
}
```

当然必须得承受operator*返回值的构造成本和析构成本。

#### 4.总结

绝不要返回pointer或reference指向一个local stack对象，或返回reference指向一个heap-allocated对象，或返回一个local static对象而有可能同时需要多个这样的对象。

### 条款22：将成员变量（也就是所有数据成员和尽量多的函数成员）声明为private

将成员变量声明为private，那么只能通过Public函数接口来访问它。

**好处：**

- 1.接口的一致性。客户在使用时，不用再纠结要不要加小括号。现在都要加，因为你所使用的接口都是函数。
- 2.封装。我可以修改计算过程，来得到返回值。我的代码版本更替，而用户并不用因此作出改变。

**从封装的角度来看：**

其实只有两种访问权限：private(提供封装) 和 public、protected（不提供封装）

解释：

- 假设我们有一个pulic成员变量，而我们最终取消了它。所有使用它的客户代码都会被破坏，这是个不可知的大量。因此Public成员变量完全没有封装性。
- 假设我们有一个protected成员变量，而我们最终取消了它。所有使用它的派生类都会被破坏，这也是一个不可知的大量。因此protected成员变量就像public成员变量一样缺乏封装性，因为在这两种情况下，如果成员变量被改变。都会有不可预知的大量代码受到破坏。

总结：

- 切记将成员变量声明为private。这可赋予客户访问数据的一致性、可细微划分访问控制、允诺约束条件获得保证，并提供class作者以充分的实现弹性。
- protected并不比public更具封装性。

### 条款23：宁以non-member、non-friend替换member函数（能写成非成员函数的，就尽量写成非成员且非友元函数）

```c++
class WebBrowser{
public:
    ...
    void clearCache();
    void clearHistory();
    void removeCookies();
    ...
}
```

```c++
class WebBrowser{
public:
    ...
    void clearEverything();//member函数,调用clearCache,clearHistory和removeCookies
}
```

```c++
void clearBrowser(WebBrowser& wb){//non-member、non-friend函数
    wb.clearCache();
    wb.clearHistory();
    wb.removeCookies();
}
```

宁可拿non-member non-friend函数替换member函数。这样做可以增加封装性、包裹弹性和机能扩充性

### 条款24：若所有参数皆需类型转换，请为此采用non-member函数

```c++
class Rational{
public:
    Rational(int numerator=0,int denominator=1);//构造函数是non-explicit的，允许int-to-Rational隐式转换
    int numerator() const;//分子(numerator)和分母(denominator)
    int denominator() const;
private:
    ...
};
```

假如将operator*写成Rational成员函数：

```c++
class Rational{
public:
	...
    const Rational operator*(const Rational& rhs) const;
};
```

```c++
Rational oneEighth(1,8);
Rational oneHalf(1,2);
Rational result=oneHalf*oneEighth;//正确
result=result*oneEighth;//正确

result=oneHalf*2;//在Raitonal构造函数声明为non-explicit时，正确
//即result=oneHalf.operator*（2）;//正确，2被隐式转换了

result=2*oneHalf;//错误!
//即result=2.operator*(oneHalf);//错误，因为this对象那个位置的隐喻参数2是不可以被隐式转换的。
```

因为，只有当参数被列于参数列内，这个参数才是隐式类型转换的合格参与者。但是this对象那个位置的隐喻参数，是不可以被隐式转换的。也就是`result=2*oneHalf;//错误!`里边这个2这个位置。

```c++
result=oneHalf*2;//在Raitonal构造函数声明为non-explicit时，正确;
//相当于
const Rational temp(2);
result = oneHalf*temp;
```



所以解决这种情况（lhs和rhs都需要隐式转换）的最佳手段就是，让operator*成为一个non-member函数，即允许编译器在每一个实参身上执行隐式类型转换：

```c++
class Rational{
...
};

const Rational operator*(const Rational& lhs,const Rational& rhs){
    return Rational(lhs.numerator()*rhs.numerator(),lhs.denominator()*rhs.denominator());
}

Rational oneFourth(1,4);
Rational result;
result = oneFourth*2;//正确
result=2*oneFourth;//正确
```

因为本例所有的对类内成员变量的访问都有public接口，所以operator*可以声明为 non-friend的（无需直接访问private，通过public接口即可）。因此尽量的，能用non-friend、non-member函数去替代其他情况。

**总结：**

如果你需要为某个函数的所有参数（包括被this指针所指的那个隐喻参数）进行类型转换，那么这个函数必须是个non-member

### 条款25：考虑写出一个不抛异常的swap函数（没学会）

## 5.实现

#### 条款26：尽可能延后变量定义式的出现时间

## 6.继承与面向对象设计

C++的OOP：

- 继承可以是：单一继承或者多重继承
- 每一个继承连接可以是：public、protected、private，也可以是virtual或者non-virtual
- 成员函数的各个选项：virtual？non-virtual？ pure virtual?

### 条款32：确定你的public继承塑模出is-a关系

如果你令class D("Derived")以**public形式继承**class B("Base")，你便是告诉C++编译器（以及代码读者）说，每一个类型为D的对象同时也是一个类型为B的对象，反之不成立。你的意思是B比D表现出更一般化的概念，而D比B表现出更特殊化的概念。你主张“B对象可派上用场的任何地方，D对象一样可以派上用场“，因为每一个D对象都是一种（是一个）B对象。反之，如果你需要一个D对象，B对象无法效劳，因为虽然每个D对象都是一个B对象，反之并不成立。

**总结**

“public继承”意味is-a。适用于base classes身上的每一件事情一定也适用于derived classes身上，因为每一个derived class对象也都是一个base class对象。

### 条款33：避免遮掩继承而来的名称

- derived class内的名称会遮掩base class内的名称。在public继承下从来没有人希望如此
- 为了让被遮掩的名称再见天日，可使用using声明式或转交函数

### 条款34：区分接口继承和实现继承

- 希望derived class只继承成员函数的接口（也就是声明）
  - 基类声明一个纯虚函数，只具体指定接口继承
- 希望derived class同时继承函数的接口和实现，但又希望能够覆写它们所继承的实现
  - 基类的普通虚函数:具体指定接口继承及缺省实现继承
- 希望derived class同时继承函数的接口和实现，并且不允许覆写任何东西
  - 非虚函数具体指定:接口继承，以及强制性实现继承。它绝不该在derived class中被重新定义

```c++
class Shape{
public:
    virtual void draw() const=0;//pure virtual函数；纯虚函数
    virtual void error(const std::string& msg);//impure virtual函数；普通的虚函数
    int objectID() const;//non-virtual函数；非虚函数
    ...
};

class Rectangle:public Shape{...};
class Ellipse:public Shape{...};
```

### 条款35：考虑virtual函数以外的其他选择（没学会）

### 条款36：绝不重新定义继承而来的non-virtual函数

### 条款37：绝不重新定义继承而来的缺省参数值

“继承一个带有缺省参数值的virtual函数”

```c++
//基类
class Shape{
public:
    enum ShapeColor{Red, Green, Blue};
    virtual void draw(ShapeColor color=Red) const=0;
}
```

```c++
//派生类1
class Rectangle:public Shape{
public:
    virtual void draw(ShapeColor color=Green) const;
    ...
}

//派生类2
class Circle:public Shape{
public:
    virtual void draw(ShapeColor color) const;
}
```

```c++
Shape* pr=new Rectangle;
Shape* pc=new Circle;

pc->draw(Shape::Red);//调用Circle::draw(Shape::Red);
pr->draw(Shape::Red);//调用Rectangle::draw(Shape::Red);
pr->draw();//调用Rectangle::draw(Shape::Red);！ 注意在Rectangle派生类里，虽然缺省参数值指定为了Green，但是你默认使用的时候，还是使用的基类的默认值。
```

### 条款38：通过符合塑模出has-a或“根据某物实现出”（没学会）

### 条款39：明智而审慎地使用private继承

- private继承意味implemented-in-terms-of(根据某物实现出)。
- 如果你让class D以private形式继承class B，你的用意是为了采用class B内已经备妥的某些特性，不是因为B对象和D对象存在有任何观念上的关系。
- private继承意味只有实现部分被继承，接口部分应略去
- 如果你让class D以private形式继承class B，意思是D对象根据B对象实现而得，在没有其他意涵了

### 条款40：明智而审慎地使用多重继承



