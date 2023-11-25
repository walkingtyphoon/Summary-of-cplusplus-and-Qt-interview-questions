# &#x20;A collection of C++ interview questions.

## 信号和槽机制? <mark style="color:red;">提问次数: 6次</mark>

答:信号和槽机制是`Qt`中特有的一种机制,其本质是通过一种观察者设计模式的实现,其中`信号`通常是用于触发`槽函数`的一种特殊函数,不需要实现因为它是由底层的元对象系统管理的,而`槽函数`需要我们自己来实现,因为这是我们需要实现的功能,其中信号由`emit`触发,槽函数由信号的触发而调用,两者之间联系的建立是使用`connect`函数,它是源自于其父类`QObject`,使用它的时候需要在类的最开始位置声明`Q_OBJECT`宏,而且当前类还必须要集成`QObject`或者它子类,然后通过connect进行绑定,connect函数有五个参数,其中第一个参数为信号的发出者,第二个为发出的信号,第三个为信号的接收者,第四个为当信号触发时,需要执行的槽函数,第五个参数是关联的类型,其分类有:自动关联,直接关联,唯一关联,队列关联,阻塞队列关联.

## 对多态的理解 <mark style="color:red;">提问次数:5次</mark>

答:`多态`是面向对象的三大特征之一,其他两个分别是`封装`和`继承`,此处我们讨论一下多态,多态见名知义就是多种形态,在`c++`中其分为两种:

1.  ### 静态多态

    其本质是通过函数的重载来实现的,是在编译时期
2.  ### 动态多态

    其本质上是继承的前提下,通过在父类中声明`虚函数`,虚函数分为两种`普通虚函数`和`纯虚函数`的区别在于一个可以有函数体,另一个必须没有,且被替换为`=0;`如果是普通的`虚函数`,则重写与否都不影响正常使用,而对于`纯虚函数`的话必须重写,然后将父类指针或者引用指向子类对象来实现`动态多态`,其本质上是当我们在类中声明了一个`虚函数时`,编译器则会为父类和所有的子类都创建一个为指针数组的`虚函数表`,其中存储的是所有`虚函数`的`地址`,而当我们创建一个子类对象的时候,编译器会在子类的构造函数中为对象创建一个`虚指针`,并给其赋值指向`虚函数表`,当我们使用的时候我们就可以通过子类的`虚指针`找到`虚函数表`,然后再根据`虚函数`的地址进行调用,进而达到动态多态的目的,当然父类的析构函数必须设置为`虚析构函数`,这样可以避免子类开辟空间后没有正确释放.而动态多态则是在运行时期的

## `C++11`新特性中的智能指针 <mark style="color:red;">提问次数: 5次</mark>

答:智能指针其本质通过模板类以及重载了`*`和`->`操作符来实现的,其本质是一种`RAII`技术,所谓的`RAII`的意思就是资源获取就是初始化,通过在类的构造函数中初始化数据,打开网络或者文件资源等,将其资源的释放和关闭放在析构函数中,当创建对象和释放对象的时候就会常见和释放资源.智能指针就是`RAII`技术的以及典型应用,智能指针主要分为三种:

1.  ### 共享智能指针(shared\_ptr)

    通过和多个智能指针共享开辟出来的同一块内存,不需要开发者管理内存的释放,常用的`API`有`get()`获取原始指针,`use_count()`获取指向这片内存区域的的指针个数,但是如果指向这片区域的指针是弱共享智能指针的话,计数器不会加一,其他的则会加一,不包括唯一智能指针.`reset()`是用来重置指针的指向的.
2.  ### 唯一智能指针(unique\_ptr)

    独占一片内存空间,不能使用赋值和拷贝,可以使用常用API包括重置,获取原始指针.
3.  ### 弱共享智能指针(weak\_ptr)

    主要用于解决共享智能指针之间的循环引用问题,指向共享指针的那片内存时不会导致计数器加一

***

## `c++`中的多线程的理解 <mark style="color:red;">提问次数: 4次</mark>

答:常用的类有`Thread`类比如可以使用在构造函数中传入一个函数指针来执行线程任务,不过通常使用`lambda`表达式,其它参数可以写参数,然后使用`join()`阻塞主进程等待子进程结束,也可以使用`detach()`将两个线程分离,不过主线程结束时子线程可能还没执行,所以可能需要一些演示操作.关于多线程的话我们可以使用锁机制来保证线程同步,比如`互斥锁`,`唯一锁`以及`条件变量`,`原子操作`.关于线程间的通信的话主要是互斥锁,条件变量,以及共享内存来实现的也可以通过`thread::get_id()`获取线程的`ID`,也可以通过`this_thread::sleep_for()`来进行线程睡眠,其中的参数为`chrono::`常用的方法有`seconds(int)`以及`millseconds(int)`来达到线程睡眠的目的

## 线程同步的方式有哪些? <mark style="color:red;">提问次数: 4次</mark>

1.  ### 互斥锁

    用于防止多个线程同时访问共享资源，保证数据的一致性。主要就是`mutex`,`lock_guard`,以及`unique_lock`

    {% code title="lock_guard" overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>
    #include <thread>
    #include <mutex>

    std::mutex mtx;
    int sharedData = 0;

    void updateSharedData() {
        std::lock_guard<std::mutex> lock(mtx);
        sharedData++;
    }

    void printSharedData() {
        std::lock_guard<std::mutex> lock(mtx);
        std::cout << "Shared Data: " << sharedData << std::endl;
    }

    int main() {
        std::thread thread1(updateSharedData);
        std::thread thread2(printSharedData);

        thread1.join();
        thread2.join();

        return 0;
    }
    ```
    {% endcode %}
2.  ### 条件变量

    就是`Condition Variable`用于在线程之间传递关于共享资源状态的信息，以便实现线程同步。

    {% code title="condition variable " overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>
    #include <thread>
    #include <mutex>
    #include <condition_variable>

    std::mutex mtx;
    std::condition_variable cv;
    int sharedData = 0;
    bool dataReady = false;

    void updateSharedData() {
        std::lock_guard<std::mutex> lock(mtx);
        sharedData++;
        dataReady = true;
        cv.notify_one();
    }

    void printSharedData() {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return dataReady; });
        std::cout << "Shared Data: " << sharedData << std::endl;
        dataReady = false;
    }

    int main() {
        std::thread t1(updateSharedData);
        std::thread t2(printSharedData);

        t1.join();
        t2.join();

        return 0;
    }

    ```
    {% endcode %}
3.  ### 原子操作

    就是`atomic`提供无锁的原子操作，可以确保对共享变量的安全访问。

    {% code title="" overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>
    #include <thread>
    #include <atomic>

    std::atomic<int> sharedData(0);

    void updateSharedData() {
        sharedData++;
    }

    void printSharedData() {
        std::cout << "Shared Data: " << sharedData.load() << std::endl;
    }

    int main() {
        std::thread t1(updateSharedData);
        std::thread t2(printSharedData);

        t1.join();
        t2.join();

        return 0;
    }
    ```
    {% endcode %}

## `STL`的常用容器,以及使用场景 <mark style="color:red;">提问次数: 4次</mark>

1. ### vector
   1.  ### 容器介绍:&#x20;

       `vector`底层是基于一个数组实现的动态数组,其本质为一个单端数组只能在尾部操作数据,默认是有序的是按照插入元素的顺序,而且允许重复
   2.  ### 访问方式:

       `vector`可以使用随机访问,也支持使用迭代器访问
   3.  ### 增删元素的特点:

       在尾部增加元素的时候效率比较高,但是在头部或者中间插入元素的时候效率比较低,因为涉及到移位操作,所以在头部和中间增删元素的时候效率比较低
2. ### List
   1.  ### 容器介绍:

       `List`的底层是双向循环链表来实现的,它的元素是有序的按照插入的顺序,可以重复
   2.  ### 访问方式:

       只能通迭代器访问,不能随机访问,只能使用`++`/`--`,不能使用`+5`/`+6`这种操作
   3.  ### 增删元素的特点

       增删元素效率比较,只需要前后元素进行连接,然后将当前节点空间释放即可,但是查询指点元素时效率比较低,简单地说就是增删效率比较高,查询元素以及进行修改时效率比较低,因为要先找到它,才能进行修改
3. ### queue
   1.  ### 容器介绍:

       `queue`是受限容器中的一种,其本质是一种单端队列,队列的特点的话就是先进先出,后进后出,队尾进队头出.元素是有序的,按照入队的顺序,元素可以重复
   2.  ### 访问方式:

       只能在队头出队,在队尾入队,没有迭代器,不能使用随机访问元素,只能使用`pop()`来实现出队效果,以及`push()`来实现入队效果
   3.  ### 增删元素的特点:

       单端队列的操作元素方式单一,在队头查询删除效率比较高,队尾增加效率比较高


4. ### stack
   1.  ### 容器介绍

       也是一种受限容器,它的特点是先进后出,元素是从栈顶使用栈帧压入栈底,取元素的时候,只能从栈顶逐个拿出,栈中的元素的顺序是按照插入的元素排列的
   2.  ### 访问方式

       栈的访问方式是只能从栈顶取出元素,使用`pop()`,使用`top()`来查看栈顶元素,以及使用`push()`来将元素压入栈底,没有迭代器也不支持随机访问
   3.  ### 增删元素的特点

       栈只能在栈顶弹出,以及压入元素到栈底所以在栈顶的增加弹出以及访问元素的效率比较高,栈底和栈中的元素访问效率就很差
5. ### map
   1.  ### 容器介绍

       `map`的底层是基于红黑树实现的,其中存储的是一个对组,对组中包含的是`key,value`形式,元素顺序的话是按照键的顺序排列的,有序,而且元素不允许重复,每个键是唯一,如果插入一个已经存在的键，它将会更新该键对应的值，而不是保留旧值。
   2.  ### 访问方式

       `map`的访问方式是通过迭代器访问遍历对组,通过对组的`first`和`second`来获取键和值
   3.  ### 增删元素的特点

       查找效率比较高,但是增删改的效率比较低
6. ## set
   1.  ### 容器介绍

       `set`的底层也是红黑树实现的,是一种特殊二叉排序树,元素是`有序的(按照键的)而且不重复,`如果插入一个已经存在的键，它将会更新该键对应的值，而不是保留旧值。
   2.  ### 访问方式

       只能通过迭代器访问,不能随机访问,添加的话使用`insert()`
   3.  ### 增删元素的特点

       它添加,查询,删除元素的效率比较高,但是修改的效率不太高

## `vector`和`list`的区别是什么? <mark style="color:red;">提问次数4次</mark>

1. `vector`的本质是一个单端数组,底层是一个动态数组,而`list`的本质是双向循环链表
2. 在访问方式方面,`vector`可以通过`迭代器`,以及`中括号`和`at()`来实现访问元素,但是`list`只能使用迭代器访问元素
3. 在`vector`中在头部以及中间增删元素的效率比较低,而尾部的增删效率比较高,容器本身查找效率比较高,`list`的话是增删效率比较高,但是查找和修改的效率比较低,因为都需要先遍历找到元素再操作
4. `vector`和`list`的元素顺序都是有序的不过都是按照插入的顺序,而且元素也可以重复,这是两者相同的地方

***

## `new`和`malloc`的区别? <mark style="color:red;">提问次数: 3次</mark>

1. 首先两者都是用来在堆上开辟内存空间进行分配的一种实现方式
2. `new`是一个关键字,而`malloc`则是一个第三方类库中的函数
3. `new`的话会调用类的构造函数,当`delete`的时候也会调用其对应的`析构函数`,而malloc的话则不会,只会开辟内存空间,以及释放内存空间
4. `new`开辟内存空间失败后会抛出 `std::bad_alloc` 异常，可以通过 `try-catch` 块来捕获异常并处理,而`malloc` 在分配内存失败时会返回空指针（`NULL`），我们可以通过检查返回值来判断是否分配成功
5. `new`是`c++`中提供的,而 `malloc` 是`c`标准库中的函数，位于`<cstdlib>` 头文件中,但是两者都可以在`c++`中使用
6. `new`可以被重载,允许自定义的内存管理策略.而`malloc`是第三方的不支持重载

## 为什么会发生内存泄漏? <mark style="color:red;">提问次数: 3次</mark>

1. 开辟的内存空间没有释放
2. 访问了未定义的空间
3. 对象之间循环引用
4. 以及程序出现了异常,导致内存泄漏

## 如何实现线程间的通信? <mark style="color:red;">提问次数: 3次</mark>

1.  ### 使用互斥锁

    使用互斥锁的话就需要先创建一个`std::mutex`,然后使用`std::lock_guard`来给他加上互斥锁,在需要加锁的位置,当超出作用域就会自动释放
2.  ### 条件变量

    条件变量的话就实现创建一个`std::mutex`,然后使用创建一个互斥锁或者唯一锁,然后创建一个条件变量的对象,将它添加到等待的方法中,然后再写上需要执行的函数也可以使用`lambda`表达式来替换,然后等待释放锁,以及被唤醒
3.  ### 信号量

    没怎么用过,记不清楚了

## 单例模式的运用和实现 <mark style="color:red;">提问次数: 3次</mark>

单例模式的话是一种创建型设计模式,他分为三种:饿汉式,懒汉式,以及局部静态变量,然后饿汉式的话就是直接创建一个私有的全局对象,然后再创建一个静态成员函数,然后通过这个方法就可以获取当前类的一个唯一实例对象,懒汉式的话,则是在调用的时候才创建对象并返回对象的实例,而局部静态变量的话,则是创建一个局部静态变量的对象,然后进行返回,单例模式本质不是线程安全的,我们需要通过使用双重判断并加锁来达到线程安全的目的

1.  ### 饿汉式

    {% code title="" overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>

    class SingletonHungry {
    private:
        static SingletonHungry instance;

        SingletonHungry() {
            std::cout << "SingletonHungry instance created." << std::endl;
        }

    public:
        static SingletonHungry& getInstance() {
            return instance;
        }

        // Other member functions...
    };

    SingletonHungry SingletonHungry::instance;  // Initialize the static member

    int main() {
        SingletonHungry& obj1 = SingletonHungry::getInstance();
        SingletonHungry& obj2 = SingletonHungry::getInstance();

        // obj1 and obj2 refer to the same instance
        return 0;
    }
    ```
    {% endcode %}
2.  ### 懒汉式

    {% code title="" overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>
    #include <mutex>

    class SingletonLazy {
    private:
        static SingletonLazy* instance;
        static std::mutex mtx;

        SingletonLazy() {
            std::cout << "SingletonLazy instance created." << std::endl;
        }

    public:
        static SingletonLazy& getInstance() {
            std::lock_guard<std::mutex> lock(mtx);
            if (instance == nullptr) {
                instance = new SingletonLazy();
            }
            return *instance;
        }

        // Other member functions...

        ~SingletonLazy() {
            delete instance;
        }
    };

    SingletonLazy* SingletonLazy::instance = nullptr;
    std::mutex SingletonLazy::mtx;

    int main() {
        SingletonLazy& obj1 = SingletonLazy::getInstance();
        SingletonLazy& obj2 = SingletonLazy::getInstance();

        // obj1 and obj2 refer to the same instance
        return 0;
    }
    ```
    {% endcode %}
3.  ### 局部静态变量

    {% code title="" overflow="wrap" lineNumbers="true" fullWidth="true" %}
    ```cpp
    #include <iostream>

    class SingletonLocalStatic {
    private:
        SingletonLocalStatic() {
            std::cout << "SingletonLocalStatic instance created." << std::endl;
        }

    public:
        static SingletonLocalStatic& getInstance() {
            static SingletonLocalStatic instance;  // Local static variable
            return instance;
        }

        // Other member functions...
    };

    int main() {
        SingletonLocalStatic& obj1 = SingletonLocalStatic::getInstance();
        SingletonLocalStatic& obj2 = SingletonLocalStatic::getInstance();

        // obj1 and obj2 refer to the same instance
        return 0;
    }

    ```
    {% endcode %}

## 信号和槽机制类似于哪种设计模式? <mark style="color:red;">提问次数: 3次</mark>

观察者模式

## 具体说说观察者模式? <mark style="color:red;">提问次数: 3次</mark>

我不常用,所以记不清了

## 多态,虚函数,纯虚函数,虚函数指针,虚函数表之间的联系  <mark style="color:red;">提问次数:3次</mark>

***

## 智能指针的实质是什么? 底层是如何实现的 <mark style="color:red;">提问次数: 2次</mark>

## 普通成员函数和静态成员函数的区别是什么? <mark style="color:red;">提问次数: 2次</mark>

1.  ### 调用方式不同:

    普通成员函数通过对象或者调用,而静态成员函数通过类名调用,普通函数使用`.`或者`->`,而静态成员函数则是通过`::`来调用
2.  ### 生命周期不同

    普通成员函数和对象的生命周期相同,当对象被销毁后,它也就不存在了,而静态的话存在于程序运行的整个过程中
3.  ### 关于`this`指针

    普通成员函数可以使用`this`指针,而静态成员函数则不可以使用`this`指针

## Qt中你在哪些地方使用过多线程? <mark style="color:red;">提问次数: 2次</mark>

比如当我们制作一些游戏的时候比如声音和动画特效同步的时候,以及使用定时器执行定时任务

## 指针和引用的区别 <mark style="color:red;">提问次数: 2次</mark>

1.  ### 指针本身是一个变量名,而引用则是一个别名机制,它在内存中不占用内存空间


2.  ### 指针初始化以后可以被修改,但是引用初始化以后就不可以修改


3.  ### 指针使用的领域比较广泛,比如全局,局部变量,以及函数的形式参数上,而引用通常是使用在函数的形式参数上


4.  ### 而且指针可以为空,但是引用不可以为空


5.  ### 指针的话可以指向数组的首地址,甚至是函数,而引用的话在声明是必须初始化,所以不能像指针那样灵活可以指向数组和函数


6. ### 关于操作方式的话.指针可以使用`&`获取变量或者对象的地址,使用`*`进行解引用获取其中的值,而引用则不需要可以直接操作

## 你对`static`的理解 <mark style="color:red;">提问次数: 2次</mark>

## `Qt`中连接数据库的过程,以及如何显示数据? <mark style="color:red;">提问次数: 2次</mark>

## `c++`的内存结构是什么样的? <mark style="color:red;">提问次数: 2次</mark>

## 什么情况下`vector`的迭代器会失效？ <mark style="color:red;">提问次数: 2次</mark>

## `Map`的底层,`Map`有无排列顺序,怎么让他的顺序变为降序? <mark style="color:red;">提问次数: 2次</mark>

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

## 动态链接，有没有自己写过动态链接库. <mark style="color:red;">提问次数: 1次</mark>

## 平衡二叉树的特点是什么? <mark style="color:red;">提问次数: 1次</mark>

## map和unorderedmap的区别 <mark style="color:red;">提问次数: 1次</mark>

## 数据库主键可以重复吗，有什么用 <mark style="color:red;">提问次数: 1次</mark>

## 视图的特点 <mark style="color:red;">提问次数: 1次</mark>
