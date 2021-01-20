## 1.项目文件

<img src="C:\Users\杨亚东\AppData\Roaming\Typora\typora-user-images\image-20200611150542812.png" alt="image-20200611150542812" style="zoom: 80%;" />

- **main.cpp：**主函数源文件。
- **myfirstwidget.h和myfirstwidget.cpp：**窗口类的头文件和源文件。
- **myfirstwidget.ui：**设计师界面类文件。
- **MyFirstWidget.pro.user：**用于记录打开工程的路径，所用的编译器、构建的工具链、生成目录、打开工程的qt-creator的版本等，可以打开此文件看一下，其实就是一个xml文件，一般情况，用户不需要理会。
- **MyFirstWidget.pro：**Qt工程文件，这个文件非常重要，本节中先简单介绍一下，随着我们学习的深入，我会对这个文件做详细介绍。（QTcreator打开项目就是打开这个）
- **MyFirstWidget.pro**

```c++
/*=号：你可以把等号左边的类型理解为变量，等号右边的理解为值。

+=号：你可以把左边的理解为变量列表，右边的为需要加到列表中的值。

\号：可以分行书写，但仍为一行。*/
    
#-------------------------------------------------
#
# Project created by QtCreator 2017-07-21T20:58:22
#
#-------------------------------------------------
QT       += core gui// 需要引用工程的模块，core表示核心模块，gui表示界面模块。Qt的代码都是模块化方式组织的，如果你想引入某方面的功能，就需要将对应模块引入到你的工程中。例如我想添加数据库模块，则可以写成QT += core gui sql。关于各模块的使用，我会在后面的分享中介绍。

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets//这是Qt5为了兼容Qt4而专门设计的，语句大意为如果Qt版本大于4，则将widgets模块加入到QT变量中，从这一点，我们可以推敲出Qt4的widgets模块是和gui模块为一体的，而Qt5已经将其分离了出来。
TARGET = MyFirstWidget
TEMPLATE = app//工程所使用的模版。app表示是一个窗口应用程序。如果是lib则表明是一个动态库模版。

# The following define makes your compiler emit warnings if you use
# any feature of Qt which as been marked as deprecated (the exact warnings
# depend on your compiler). Please consult the documentation of the
# deprecated API in order to know how to port your code away from it.
DEFINES += QT_DEPRECATED_WARNINGS//定义编译选项。QT_DEPRECATED_WARNINGS表示当Qt的某些功能被标记为过时的，那么编译器会发出警告。

# You can also make your code fail to compile if you use deprecated APIs.
# In order to do so, uncomment the following line.
# You can also select to disable deprecated APIs only up to a certain version of Qt.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0
SOURCES += main.cpp\
       myfirstwidget.cpp//源文件

HEADERS  += myfirstwidget.h//头文件

FORMS    += myfirstwidget.ui//设计师界面
```

## 2.

### main.cpp

```c++
#include "myfirstwidget.h"//包含myfirstwidget.h头文件，这是我们创建窗口类时，Qt自动为我们生成的头文件，且文件名都为小写。
#include <QApplication>//包含QApplication类。

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);//创建一个QApplication实例。

    MyfirstWidget w;//创建MyFirstWidget实例，并显示。
    w.show();

    return a.exec();
}

```

### myfirstwidget.h

```c++
#ifndef MYFIRSTWIDGET_H
#define MYFIRSTWIDGET_H

#include <QMainWindow>

namespace Ui {
class MyfirstWidget;
}

class MyfirstWidget : public QMainWindow//自定义类公有继承QMainWindow
{
    Q_OBJECT//Q_OBJECT宏，必须在类的私有声明中声明这个宏，这样就可以使用Qt的信号槽机制，元对象系统，对象树等Qt特有的功能，否则无法使用。所以这里推荐在创建类时，最好加上此声明，这样的代码会得到很多Qt提供的便利接口。

public:
    explicit MyfirstWidget(QWidget *parent = 0);//构造函数，禁止隐式转换
    ~MyfirstWidget();//析构函数

private:
    Ui::MyfirstWidget *ui;//新增成员
};

#endif // MYFIRSTWIDGET_H

```

###  myfirstwidget.cpp

```c++
#include "myfirstwidget.h"
#include "ui_myfirstwidget.h"

MyfirstWidget::MyfirstWidget(QWidget *parent) ://构造函数的实现
    QMainWindow(parent),//初始化基类。如果有参数parent不为空，则MyFirstWidget会成为该指针所指窗口的子窗口
    ui(new Ui::MyfirstWidget)//初始化新增成员ui。即创建Ui::MyFirstWidget界面对象，指针赋给ui
{
    ui->setupUi(this);//设置ui界面
}

MyfirstWidget::~MyfirstWidget()
{
    delete ui;
}

```



## 3.QT designer

- Edit

  - 放到前面：有控件重叠时，谁浮在前层
  - 编辑信号/槽：
    - 由一个控件拖动到另一个控件（代表了发送者，发送什么信号，接收者，槽函数），也可以拖动到空白（接收者即this）

  - 编辑伙伴：
    - 定义快捷键用的 比如 ALT +A  ，ALT+B等等
  - 编辑Tab顺序：
    - 即编辑应用程序里，各个可以互动的按钮、框等的Tab顺序

- 