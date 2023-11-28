# &#x20;A collection of C++ interview questions.

## 信号和槽机制? <mark style="color:red;">提问次数: 6次</mark>

答:信号和槽机制是`Qt`中特有的一种机制,其本质是通过一种观察者设计模式的实现,其中`信号`通常是用于触发`槽函数`的一种特殊函数,不需要实现因为它是由底层的元对象系统管理的,而`槽函数`需要我们自己来实现,因为这是我们需要实现的功能,其中信号由`emit`触发,槽函数由信号的触发而调用,两者之间联系的建立是使用`connect`函数,它是源自于其父类`QObject`,使用它的时候需要在类的最开始位置声明`Q_OBJECT`宏,而且当前类还必须要继承`QObject`或者它子类,然后通过connect进行绑定,connect函数有五个参数,其中第一个参数为信号的发出者,第二个为发出的信号,第三个为信号的接收者,第四个为当信号触发时,需要执行的槽函数,第五个参数是关联的类型,其分类有:自动关联,直接关联,唯一关联,队列关联,阻塞队列关联.

## 对多态的理解 <mark style="color:red;">提问次数:5次</mark>

答:`多态`是面向对象的三大特征之一,其他两个分别是`封装`和`继承`,此处我们讨论一下多态,多态见名知义就是多种形态,在`c++`中其分为两种:

1.  ### 静态多态

    其本质是通过函数,以及运算符的重载来实现的,是在编译时期
2.  ### 动态多态

    其本质上是继承的前提下,通过在父类中声明`虚函数`,虚函数分为两种`普通虚函数`和`纯虚函数`的区别在于一个可以有函数体,另一个必须没有,且被替换为`=0;`如果是普通的`虚函数`,则重写与否都不影响正常使用,而对于`纯虚函数`的话必须重写,然后将父类指针或者引用指向子类对象来实现`动态多态`,其本质上是当我们在类中声明了一个`虚函数时`,编译器则会为父类和所有的子类都创建一个为指针数组的`虚函数表`,其中存储的是所有`虚函数`的`地址`,而当我们创建一个子类对象的时候,编译器会在子类的构造函数中为对象创建一个`虚指针`,并给其赋值指向`虚函数表`,当我们使用的时候我们就可以通过子类的`虚指针`找到`虚函数表`,然后再根据`虚函数`的地址进行调用,进而达到动态多态的目的,当然父类的析构函数必须设置为`虚析构函数`,这样可以避免子类开辟空间后没有正确释放.而且动态多态则是在运行时期的

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

1. ### `vector`的本质是一个单端数组,底层是一个动态数组,而`list`的本质是双向循环链表
2. ### 在访问方式方面,`vector`可以通过`迭代器`,以及`中括号`和`at()`来实现访问元素,但是`list`只能使用迭代器访问元素
3. ### 在`vector`中在头部以及中间增删元素的效率比较低,而尾部的增删效率比较高,容器本身查找效率比较高,`list`的话是增删效率比较高,但是查找和修改的效率比较低,因为都需要先遍历找到元素再操作
4. ### `vector`和`list`的元素顺序都是有序的不过都是按照插入的顺序,而且元素也可以重复,这是两者相同的地方

***

## `new`和`malloc`的区别? <mark style="color:red;">提问次数: 3次</mark>

1. ### 首先两者都是用来在堆上开辟内存空间进行分配的一种实现方式
2. ### `new`是一个关键字,而`malloc`则是一个第三方类库中的函数
3. ### `new`的话会调用类的构造函数,当`delete`的时候也会调用其对应的`析构函数`,而malloc的话则不会,只会开辟内存空间,以及释放内存空间
4. ### `new`开辟内存空间失败后会抛出 `std::bad_alloc` 异常，可以通过 `try-catch` 块来捕获异常并处理,而`malloc` 在分配内存失败时会返回空指针（`NULL`），我们可以通过检查返回值来判断是否分配成功
5. ### `new`是`c++`中提供的,而 `malloc` 是`c`标准库中的函数，位于`<cstdlib>` 头文件中,但是两者都可以在`c++`中使用
6. ### `new`可以被重载,允许自定义的内存管理策略.而`malloc`是第三方的不支持重载

## 为什么会发生内存泄漏? <mark style="color:red;">提问次数: 3次</mark>

1. ### 开辟的内存空间没有释放
2. ### 访问了未定义的空间
3. ### 对象之间循环引用
4. ### 以及程序出现了异常,导致内存泄漏

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

1.  ### 简介:

    静态这个关键字呢,是一个`存储类型说明符`,除了静态还有`auto`,`register`,`extern`,`mutable`,以及`thread_local`.谈论静态的话我们就可以将它和普通的成员函数以及成员变量来进行对比讨论了,比如
2.  ### 存储位置不同

    普通成员函数,以及成员变量都是存储在开辟出来的堆空间中的,而静态则是存储在静态存储区中
3.  ### 生命周期不同

    普通成员函数,成员变量的生命周期为对象的生命周期,随着对象的创建而创建随着对象的销毁而销毁;而静态则是随着类的加载而创建,随着类的销毁而销毁,是存在于整个程序的运行期间的
4.  ### 访问作用范围不同

    非静态可以调用静态,但是静态只能调用静态,因为静态存在的时候对象还不存在,而普通成员函数及变量的存在依赖于对象
5.  ### 使用方式不同

    普通成员函数通过对象的指针以及引用,也就是`->`以及`.`来访问,而静态则是通过双冒号来调用
6.  ### 能否使用`this`指针的不同

    普通成员函数以及变量依赖于对象,所以他们可以使用依赖于当前对象提供的`this`指针,而静态则是先于对象存在的,所以无法使用`this`指针
7.  ### 初始化的位置不同

    普通成员函数的初始化可以在类中的构造函数进行初始化,而静态的话则需要在类外进行初始化,比如举个简单的例子就是`数据类型 类名::变量名=初始化值;`
8.  ### 是否支持多态

    普通成员函数可以实现多态,因为它们的调用取决于实际的对象类型,而静态的话则是不参与多态,因为它是和类的类型绑定的

## `c++`的内存结构是什么样的? <mark style="color:red;">提问次数: 2次</mark>

c++的内存结构的话主要分为五个区:

1.  ### 栈

    栈是用来存储局部变量以及函数的调用信息的,它的创建和释放的速度比较快,一般不需要程序员手动管理
2.  ### 堆

    堆是用来存储我们程序员自己创建出来的对象,以及开辟出来的内存空间的,通常需要我们手动管理也就是开辟和释放,如果不释放的话会造成内存泄漏,堆的大小一般受限制与操作系统以及编译器
3.  ### 全局/静态存储区

    这里一般用来存储全局/静态的变量以及函数,在程序的运行期间是始终存在的,直到程序的结束才释放
4.  ### 常量存储区

    常量存储区的话是用于存储常量数据,比如字符串常量,该区域的数据在程序运行期间是不能修改的
5.  ### 代码区

    这里存储的是程序的执行代码,一般都是只读的不允许写入

## `Qt`中连接数据库的过程,以及如何显示数据? <mark style="color:red;">提问次数: 2次</mark>

1. ### 获取所有可以使用的驱动字符串列表
2. ### 声明一个`QSqlDatabase`对象
3. ### 遍历数据库驱动字符串列表,如果当前的字符串等于我们需要的驱动就使用当前的字符串来初始化`QSqlData`对象,使用`QSqlDatabase::addDatabase(驱动名称);`
4. ### 然后设置数据库的配置信息比如主机名,端口,用户,密码,数据库名等
5. ### 然后就可以使用QSqlDatabase对象的open()判断是否打开数据库
6. ### 如果打开就正常进行数据库操作,没有的话就调试是不是驱动,以及配置信息或者代码出错
7. ### 显示数据的话可以使用`QSqlQuery`对象的`exec(SQL字符串)`来执行基本的`SQL`语句,可以使用:
   1. ### QSqlTableModel ： 创建一个可编辑的表格式数据模型（注意：只能应用于单表）
      1. 使用步骤的话就是先创建模型关联模式数据表,比如`auto stuModel = new QSqlTableModel(this);`
      2. 然后设置使用的数据表,比如`stuModel->setTable("表名");`
      3. 设置好之后我们就可以执行查询操作,显示数据了,比如:`stuModel->select();`
      4. 然后也可以直接将查询出来的数据填充到表格视图中: `ui->stuTableView->setModel(stuModel);`
   2. ### QTableView ：常见一个表格视图，可以将 QSqlTableModel 创建的模型自动填充到表格中
   3. ### QSqlRelationalTableModel ：创建关联数据类型的数据模型
      1. 先创建关联表模型对象,比如:`auto stuTableModel = new QSqlRelationalTableModel(this);`
      2. 然后使用`setTable("表名");`来设置使用的第一张表
      3. 然后使用`setRelation(6, QSqlRelation("dept", "dno", "dname"));`来设置关联信息第一个参数是当前表中用来关联的列名,而第二个参数则使用过一个`QSqlRelation()`来创建一个新的`QSqlRealtion`来设置关联关系,第一个参数是表名,第二个是这个表用来关联的列名,第三个参数是要替换成的列名

## 什么情况下`vector`的迭代器会失效？ <mark style="color:red;">提问次数: 2次</mark>

我们在获取迭代器后,又对原来的动态数组进行了增删操作,而继续使用获取的迭代器的时候这是迭代器会失效,对于迭代器失效我们可以使用`insert(尾部迭代器,元素)` 来返回一个新的迭代器,以及使用`erase()`来擦除元素,返回一个新的迭代器,放置迭代器失效

## `Map`的底层,`Map`有无排列顺序,怎么让他的顺序变为降序? <mark style="color:red;">提问次数: 2次</mark>

有,是按照键的顺序升序排列的,可以使用反向迭代器,以及在map的泛型位置加入一个比较函数对象,设置一个类或者结构体,然后重写它的小括号,然后形式参数上写上前后的两个元素,返回第一个大于第二个就可以了

{% code title="" overflow="wrap" lineNumbers="true" fullWidth="false" %}
```cpp
#include <iostream>
#include <map>

struct Compare {
    bool operator()(const int& lhs, const int& rhs) const {
        return lhs > rhs; // 降序
    }
};

int main() {
    std::map<int, std::string, Compare> myMap;

    myMap[1] = "one";
    myMap[3] = "three";
    myMap[2] = "two";

    // 此时 myMap 中的元素按照键的降序排列
    for (const auto& pair : myMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    return 0;
}
```
{% endcode %}

***

## `Qt`动态展示怎么实现动态? <mark style="color:red;">提问次数: 1次</mark>

1. ### 使用信号和槽机制
   1. 设置一个数据变化的信号
   2. 当数据发生变化时触发这个信号
   3. 然后触发对应的槽函数重新设置数据或者效果
2. ### 使用定时器周期性刷新

{% code overflow="wrap" lineNumbers="true" %}
```cpp
// 创建一个定时器
QTimer* timer = new QTimer(this);

// 连接定时器的timeout信号到槽函数
connect(timer, &QTimer::timeout, this, &MyClass::updateDataAndRefreshView);

// 启动定时器
timer->start(1000); // 每隔1000毫秒（1秒）触发一次timeout信号
```
{% endcode %}

## 栈和队列的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

## 怎么实现上位机和终端的通讯? <mark style="color:red;">提问次数: 1次</mark>

## 讲一下`XML` <mark style="color:red;">提问次数: 1次</mark>

xml是可扩展标记语言,通常用来传输数据,其中的数据表现形式是使用标签进行存储,不过后来大部分改用JSON了

## 动态链接库的理解 <mark style="color:red;">提问次数: 1次</mark>

## 基类析构函数为什么要定义为虚函数 <mark style="color:red;">提问次数: 1次</mark>

因为当我们调用父类的析构函数的时候子类可能也开辟了新的内存空间,如果不将基类的析构函数设置为虚析构函数的话,子类开辟的内存空间就可能没有释放,导致内存泄漏,这样当释放父类指针的时候,编译器会自动调用子类的析构函数释放内存空间

## `map`和`unordered_map`的区别是什么? <mark style="color:red;">提问次数: 1次</mark>

1.  ### 底层实现不同

    `map`的底层是红黑树,而`unordered_map`的底层是哈希表
2.  ### 存储的元素的顺序不同

    `map`中的元素是按照键的元素升序排列,而`unordered_map`则没有固定的顺序,遍历时无法预测元素顺序
3.  ### 查找效率不同

    `unordered_map`的查询效率高于`map`,因为前者是基于哈希表,时间复杂度为`O(1)`,而红黑树则是`O(log_n)`
4.  ### 占用的内存大小不同

    map的底层是红黑树结构,因为维护这个树形结构所以它占用的内存会比`unordered_map`更大

## 移动构造和移动拷贝的理解 <mark style="color:red;">提问次数: 1次</mark>

移动构造的话,就是将传递进来的对象赋值给当前的这个新对象,而移动拷贝的话则是重写了等于号操作符,将右值拷贝给左值

## `Redis`的数据类型 <mark style="color:red;">提问次数: 1次</mark>

1. ### 字符串
2. ### 哈希表
3. ### 列表
4. ### 集合
5. ### 有序集合

## `静态常量`存放在`C++`哪个区? <mark style="color:red;">提问次数: 1次</mark>

存放在静态存储区,也叫做全局存储区,里边存储静态,以及全局函数和变量

## `vector`中插入一个元素,如果内存不够时会怎样? <mark style="color:red;">提问次数: 1次</mark>

当我们插入新元素的时候,如果内存不够,vector会自动扩容,如果是Windows系统的话是1.5倍向下取整,而Linux下是2.0倍向下取整,然后将所有元素拷贝到新申请的内存空间,然后释放原来的内存空间

## `Tcp`的三次握手对应`socket`中的哪个函数? <mark style="color:red;">提问次数: 1次</mark>

在TCP的三次握手过程中，对应到Socket编程中的函数通常是`connect`函数。在客户端使用`connect`函数时，会触发TCP的三次握手过程。

具体来说：

1. **第一次握手：**
   * 客户端调用`connect`函数向服务器发起连接请求。
2. **第二次握手：**
   * 服务器收到连接请求，通过`accept`函数接受连接，从而完成第二次握手。
3. **第三次握手：**
   * 客户端收到服务器的确认，`connect`函数返回，此时完成第三次握手。

所以，虽然`connect`函数在客户端中调用，但它实际上触发了TCP的三次握手过程。在服务器端，对应的是`accept`函数，它接受连接请求，完成握手过程。

## `Linux`下如何查看内存使用情况?查看`CPU`占用率 <mark style="color:red;">提问次数: 1次</mark>

1. ### `free -h` 显示系统内存的使用情况，包括空闲内存、已使用内存等
2. ### `top` 实时动态显示系统资源使用情况，包括内存使用情况

## 线程中的同步和异步 <mark style="color:red;">提问次数: 1次</mark>

1. 同步指的是在执行某个操作时，调用者需要等待这个操作完成，然后再继续执行下一步。同步操作是按顺序执行的，阻塞调用者直到操作完成。优点的话就是相对简单，易于理解和调试。缺点的话就是可能会导致程序的响应性降低，特别是在执行耗时操作时。
2. 异步指的是在执行某个操作时，调用者不需要等待操作完成，而是可以继续执行其他操作。异步操作通常使用回调函数或者事件来处理完成后的逻辑。优点的话是提高程序的响应性，允许同时执行多个任务。缺点的话是编码和调试相对复杂，因为需要处理异步操作的回调和事件。

## 你对`const`的理解 <mark style="color:red;">提问次数: 1次</mark>

`const`的话是`c++`中的一个关键字,可以用来修饰变量,和常量以及函数,表示其值在程序的运行过程中不能修改,修饰成员函数的话，表示该成员函数不会修改类的成员变量。总的来说，const 在 C++ 中用于声明常量、修饰变量、指针和成员函数，是一种强大的约束和保护机制，有助于编写更安全、可靠的代码。

## `Qt`中事件处理机制有哪些?都有什么用? <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中动态展示有哪些实现思路? <mark style="color:red;">提问次数: 1次</mark>

## `Qt`中的预编译都做了什么,`MOC`的作用是什么? <mark style="color:red;">提问次数: 1次</mark>

## `http`和`https`的区别? <mark style="color:red;">提问次数: 1次</mark>

1. **安全性：**
   * **HTTP：** 是一种明文传输协议，数据在传输过程中未经加密，容易被窃听和篡改。
   * **HTTPS：** 使用了 SSL/TLS 协议进行加密，数据在传输过程中经过加密处理，更安全，难以被窃听和篡改。
2. **端口：**
   * **HTTP：** 默认使用端口80。
   * **HTTPS：** 默认使用端口443。
3. **协议：**
   * **HTTP：** 使用标准的 HTTP 协议。
   * **HTTPS：** 在 HTTP 的基础上加入了 SSL/TLS 协议，通过 URL 的开头 "https://" 表示。
4. **证书：**
   * **HTTP：** 不需要使用证书。
   * **HTTPS：** 需要使用 SSL 证书，用于建立安全连接。SSL 证书通常由权威的证书颁发机构（CA）颁发，用于验证网站的身份。
5. **加密算法：**
   * **HTTP：** 传输的数据是明文的，不经过加密。
   * **HTTPS：** 采用 SSL/TLS 协议，使用非对称加密、对称加密和哈希算法，确保数据在传输过程中的机密性和完整性。
6. **搜索引擎排名：**
   * **HTTPS：** 对网站使用 HTTPS 加密有利于搜索引擎优化（SEO），一些搜索引擎可能更倾向于显示使用 HTTPS 的网站。
7. **使用场景：**
   * **HTTP：** 适用于一些不涉及敏感信息传输的场景，如普通网页浏览。
   * **HTTPS：** 适用于需要保护用户敏感信息的场景，如在线支付、登录等。

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
