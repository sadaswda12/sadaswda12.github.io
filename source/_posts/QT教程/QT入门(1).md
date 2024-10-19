---
title: QT入门教程 --- 基本框架
---

# 1.QT的基本框架

- main.cpp分析

打开sources里面的main.cpp文件可以看到下面代码：

```c++
//在代码中创建了一个QApplication对象并且调用其中的show函数显示
#include "mainwindow.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}

```

- 打开mainwindow.cpp文件和minwindow.h文件

```c++
#include "mainwindow.h"
#include "./ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
}

MainWindow::~MainWindow()
{
    delete ui;
}
```

```c++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow> //QT窗口类头文件

QT_BEGIN_NAMESPACE
namespace Ui {
class MainWindow;  //UI界面命名空间
}
QT_END_NAMESPACE

class MainWindow : public QMainWindow //自定义Qmaindow类
{
    Q_OBJECT //QT信号与槽的机制

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();
    
private slots:
    void on_open_clicked();
    
private:
    Ui::MainWindow *ui;
};
#endif // MAINWINDOW_H

```

- 最上面是Cmakelist.txt文件，存储项目设置

# 2. 简单使用

- 创建一个 button

1. 首先，在帮助中搜索 Qpushbutton，在Cmake中添加配置信息

![1](/images/QT教程/入门(1)/1.png)

2. 编写如下代码

```c++
  #include <QPushButton>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    //创建一个buton,并且将button显示在当前mainwindow中
    QPushButton *button5 = new QPushButton(this);

    //设置文字
    button5->setText("第一个btn");
    //设置按钮位置
    button5->move(100,100);

}

MainWindow::~MainWindow()
{
    delete ui;
}
```



从而就可以得到一个下图所示的界面

![1](/images/QT教程/入门(1)/2.png)

# 3.对象模型(对象树)

## 1.对象数

我们常常听到 QObject 会用对象树来组织管理自己，那什么是对象树？

这个概念非常好理解。因为 QObject 类就有一个私有变量 QList<QObject *>，专门存储这个类的子孙后代们。比如创建一个 QObject 对象并指定父对象时，就会把自己加入到父对象的 childre() 列表中，也就是 QList<QObject *> 变量中。

 父对象析构的时候，这个列表中的所有对象也会被析构。**（注意，这 里的父对象并不是继承意义上的父类！）**

举个例子，有一个窗口 Window，里面有 Label标签、TextEdit文本输入框、Button按钮这三个元素，并且都设置 Window 为它们的父对象。这时候我做了一个关闭窗口的操作，作为程序员的你是不是自然想到将所有和窗口相关的对象析构啊？古老的办法就是一个个手动 delete 呗。是不是很麻烦？Qt 运用对象树模式，当父对象被析构时，子对象自动就 delete 掉了，不用再写一大堆的代码了。

**QWidget 是能够在屏幕上显示的一切组件的父类（**QWidget 继承自 QObject，因此也继承了这种对象树关系。）

## 2.析构顺序问题

  正常情况下，最后被创建出来的会先被析构掉。就好比我有一个大桌子，上面先摆放一个盘子，再摆放一个碗。当我要把桌子撤掉的时候，会先撤掉碗，再撤掉盘子，最后撤掉桌子。

  用代码演示：

```c++
//后创建的 quit 对象指定了 window 为其父对象。那么关闭程序时，会先调用它的析构函数，然后调用 window 的析构函数。
int main()
{
     QWidget window;
     QPushButton quit("Quit", &window);
}
```

**这就有一个特殊情况**

```c++
//如果反过来，由于 window 后创建，程序关闭时先调用 window 的析构函数（此时 quit 被第一次析构）。接着调用 quit 的析构函数（此时 quit 被第二次析构），这时由于被两次析构，所以出问题了。

//这种特殊情况在编程中很隐蔽，不容易发现。因为编译的时候不会报错，只有运行时才会产生问题。

int main()
{
    QPushButton quit("Quit");
    QWidget window;
 
    quit.setParent(&window);
}
```

# 4.QT窗口坐标系

从左上向右向下为正方向！
