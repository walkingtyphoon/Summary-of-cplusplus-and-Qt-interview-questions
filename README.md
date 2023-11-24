# &#x20;A collection of C++ interview questions.

## 信号和槽机制? <mark style="color:red;">提问次数: 5次</mark>

答:信号和槽机制是`Qt`中特有的一种机制,其本质是通过一种观察者设计模式的实现,其中`信号`通常是用于触发`槽函数`的一种特殊函数,不需要实现因为它是由底层的元对象系统管理的,而`槽函数`需要我们自己来实现,因为这是我们需要实现的功能,其中信号由`emit`触发,槽函数由信号的触发而调用,两者之间联系的建立是使用`connect`函数,它是源自于其父类`QObject`,使用它的时候需要在类的最开始位置声明`Q_OBJECT`宏,而且当前类还必须要集成`QObject`或者它子类,然后通过connect进行绑定,connect函数有五个参数,其中第一个参数为信号的发出者,第二个为发出的信号,第三个为信号的接收者,第四个为当信号触发时,需要执行的槽函数,第五个参数是关联的类型,其分类有:自动关联,直接关联,唯一关联,队列关联,阻塞队列关联.

## 对多态的理解 <mark style="color:red;">提问次数:5次</mark>

`多态`是面向对象的三大特征之一,其他两个分别是`封装`和`继承`,此处我们讨论一下多态,多态见名知义就是多种形态,在`c++`中其分为两种:

1.  ### 静态多态

    其本质是通过函数的重载来实现的,是在编译时期
2.  ### 动态多态

    其本质上是继承的前提下,通过在父类中声明`虚函数`,虚函数分为两种`普通虚函数`和`纯虚函数`的区别在于一个可以有函数体,另一个必须没有,且被替换为`=0;`如果是普通的`虚函数`,则重写与否都不影响正常使用,而对于`纯虚函数`的话必须重写,然后将父类指针或者引用指向子类对象来实现`动态多态`,其本质上是当我们在类中声明了一个`虚函数时`,编译器则会为父类和所有的子类都创建一个为指针数组的`虚函数表`,其中存储的是所有`虚函数`的`地址`,而当我们创建一个子类对象的时候,编译器会在子类的构造函数中为对象创建一个`虚指针`,并给其赋值指向`虚函数表`,当我们使用的时候我们就可以通过子类的`虚指针`找到`虚函数表`,然后再根据`虚函数`的地址进行调用,进而达到动态多态的目的,当然父类的析构函数必须设置为`虚析构函数`,这样可以避免子类开辟空间后没有正确释放.

## `C++11`新特性中的智能指针 <mark style="color:red;">提问次数: 5次</mark>

***

## `c++`中的多线程的理解 <mark style="color:red;">提问次数: 4次</mark>

## 线程同步的方式有哪些? <mark style="color:red;">提问次数: 4次</mark>

***

## `new`和`malloc`的区别? <mark style="color:red;">提问次数: 3次</mark>

## 为什么会发生内存泄漏? <mark style="color:red;">提问次数: 3次</mark>

## 单例模式的运用和实现 <mark style="color:red;">提问次数: 3次</mark>

## `STL`的常用容器,以及使用场景 <mark style="color:red;">提问次数: 3次</mark>

## 信号和槽机制类似于哪种设计模式? <mark style="color:red;">提问次数: 3次</mark>

***

## 智能指针的实质是什么? 底层是如何实现的 <mark style="color:red;">提问次数: 2次</mark>

## 普通成员函数和静态成员函数的区别是什么? <mark style="color:red;">提问次数: 2次</mark>

## Qt中你在哪些地方使用过多线程? <mark style="color:red;">提问次数: 2次</mark>

## 指针和引用的区别 <mark style="color:red;">提问次数: 2次</mark>

## 如何实现线程间的通信? <mark style="color:red;">提问次数: 2次</mark>

## 你对`static`的理解 <mark style="color:red;">提问次数: 2次</mark>

## `Qt`中连接数据库的过程,以及如何显示数据? <mark style="color:red;">提问次数: 2次</mark>

## `c++`的内存结构是什么样的? <mark style="color:red;">提问次数: 2次</mark>

## 多态,虚函数,纯虚函数,虚函数指针,虚函数表之间的联系  <mark style="color:red;">提问次数:2次</mark>

## 什么情况下`vector`的迭代器会失效？ <mark style="color:red;">提问次数: 2次</mark>

## `Map`的底层,`Map`有无排列顺序,怎么让他的顺序变为降序? <mark style="color:red;">提问次数: 2次</mark>

## 具体说说观察者模式? <mark style="color:red;">提问次数: 2次</mark>

***

## `Qt`动态展示怎么实现动态? <mark style="color:red;">提问次数: 1次</mark>

## 栈和队列的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

## 怎么实现上位机和终端的通讯? <mark style="color:red;">提问次数: 1次</mark>

## 讲一下`XML` <mark style="color:red;">提问次数: 1次</mark>

## 动态链接库的理解 <mark style="color:red;">提问次数: 1次</mark>

## 基类析构函数为什么要定义为虚函数 <mark style="color:red;">提问次数: 1次</mark>

## `map`和`unordered_map`的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

## 移动构造和移动拷贝的理解 <mark style="color:red;">提问次数: 1次</mark>

## `new`和`malloc`的区别 <mark style="color:red;">提问次数: 1次</mark>

## `Redis`的数据类型 <mark style="color:red;">提问次数: 1次</mark>

## `静态常量`存放在`C++`哪个区? <mark style="color:red;">提问次数: 1次</mark>

## `vector`中插入一个元素,如果内存不够时会怎样? <mark style="color:red;">提问次数: 1次</mark>

## `Tcp`的三次握手对应`socket`中的哪个函数? <mark style="color:red;">提问次数: 1次</mark>

## `Linux`下如何查看内存使用情况?查看`CPU`占用率 <mark style="color:red;">提问次数: 1次</mark>

## 线程中的同步和异步 <mark style="color:red;">提问次数: 1次</mark>

## 你对`const`的理解 <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中事件处理机制有哪些?都有什么用? <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中动态展示有哪些实现思路? <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中的预编译都做了什么,`MOC`的作用是什么? <mark style="color:red;">提问次数: 1次</mark>

## `http`和`https`的区别? <mark style="color:red;">提问次数: 1次</mark>

## `Tcp`的常用函数 <mark style="color:red;">提问次数: 1次</mark>

## 虚函数的性能为什么差? <mark style="color:red;">提问次数: 1次</mark>

## `Https`怎么实现安全的? <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中的`Qmake`有了解过吗? <mark style="color:red;">提问次数: 1次</mark>

## 说一下`Qt`中的委托? <mark style="color:red;">提问次数: 1次</mark>

## 怎么阻止迭代器失效？ <mark style="color:red;">提问次数: 1次</mark>

## 什么是泛型编程?什么是仿函数? <mark style="color:red;">提问次数: 1次</mark>

## 仿函数解决的问题是什么? <mark style="color:red;">提问次数: 1次</mark>

## 视图的特性是什么? <mark style="color:red;">提问次数: 1次</mark>

## ab通信 a每秒生成10种数据每个数据 100数据 实时发送 b实时接数据 并解析出正确的数据类型,说一下B 的接收?B端的通信.如何保持数据一致? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈你对`Tcp`的了解? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈你对`udp`的了解? <mark style="color:red;">提问次数: 1次</mark>

## `udp`能保证不丢包吗?如何解决呢? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈`Tcp`的三次握手和四次挥手,为什么握手和挥手不是两次或者四次? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈如何实现socket通信? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈你常用的`Linux`命令 <mark style="color:red;">提问次数: 1次</mark>

## `MySQL`中`SQL`语句的执行顺序? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈多线程中的同步与互斥? <mark style="color:red;">提问次数: 1次</mark>

## 两个线程拿两个锁有哪些注意事项?对于可能出现的死锁问题,你有什么解决方案? <mark style="color:red;">提问次数: 1次</mark>

## 谈谈你对工厂模式和单例模式的理解 <mark style="color:red;">提问次数: 1次</mark>

## `vector`的默认容量是多少? <mark style="color:red;">提问次数:1次</mark>

## `vector`的扩容机制是怎么样的? <mark style="color:red;">提问次数:1次</mark>

## `lambda`表达式替换函数指针和函数对象时应该如何替换 <mark style="color:red;">提问次数: 1次</mark>

## `STL`和`Qt`的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

## C++语法中函数一定要实现，在Qt中的信号为什么不需要定义 <mark style="color:red;">提问次数: 1次</mark>

## 在项目中具体怎么使用`QChart`绘制图表 <mark style="color:red;">提问次数: 1次</mark>

## 服务器最多可以接受多少台客户端同时连接? <mark style="color:red;">提问次数: 1次</mark>

## `c`和`c++`如何开辟内存? <mark style="color:red;">提问次数: 1次</mark>

## `Linux`下的内存拷贝 <mark style="color:red;">提问次数: 1次</mark>

## 进程和线程的区别 <mark style="color:red;">提问次数: 1次</mark>

## 怎么理解进程是资源调度的最小单位这句话? <mark style="color:red;">提问次数: 1次</mark>

## `Linux`的内存管理? <mark style="color:red;">提问次数: 1次</mark>

## `Linux`下的多线程 <mark style="color:red;">提问次数: 1次</mark>

## `new`的底层是如何实现的? <mark style="color:red;">提问次数: 1次</mark>

## 在`Qt`中使用过多线程了吗? <mark style="color:red;">提问次数:1次</mark>

## 在`Qt`中 如何绘制三维图? <mark style="color:red;">提问次数:1次</mark>

## 对象树是什么? <mark style="color:red;">提问次数:1次</mark>

## 空类的大小? <mark style="color:red;">提问次数1次</mark>

## 继承中的构造和析构顺序是什么? <mark style="color:red;">提问次数1次</mark>

## 如何创建一个进程,并获取它的`id?` <mark style="color:red;">提问次数1次</mark>

## 讲一讲动态内存分配? <mark style="color:red;">提问次数1次</mark>

## 构造函数里能放虚函数吗? <mark style="color:red;">提问次数1次</mark>

## `vector`和`list`的区别是什么? <mark style="color:red;">提问次数1次</mark>

## `set`和`map`的区别是什么? <mark style="color:red;">提问次数1次</mark>

## 类型转换的分类? <mark style="color:red;">提问次数1次</mark>

## `git`中怎么解决代码冲突 <mark style="color:red;">提问次数1次</mark>

## 重载与重写的区别? <mark style="color:red;">提问次数1次</mark>

## 谈谈你对`lambda`表达式的理解,以及注意事项  <mark style="color:red;">提问次数1次</mark>

## 堆中变量和栈中变量的区别? <mark style="color:red;">提问次数: 1次</mark>

## 能不能手动在栈上申请空间? <mark style="color:red;">提问次数: 1次</mark>

## 快速排序的思想是什么? <mark style="color:red;">提问次数: 1次</mark>

## 平衡二叉树和普通二叉树的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

## 懒汉式和饿汉式的区别是什么?哪个更好?为什么? <mark style="color:red;">提问次数: 1次</mark>

## `Linux`下如何创建文件 <mark style="color:red;">提问次数: 1次</mark>

##

##
