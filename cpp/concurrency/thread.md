# C++线程

## 1. `std::thread::id`类

### 1.1 作用

+ `std::thread`和`std::jthread`对象的唯一标识符
+ 作为有序和无序关联容器的键

### 1.2 构造函数

默认构造函数构造**不表示任何线程的 ID**，原型如下：

```C++
id() noexcept;
```

### 1.3 非成员函数

+ 比较运算符：`operator==`  `operator<`等
+ 输出运算符

```C++
template <class CharT, class Traits>
std::basic_ostream<CharT, Traits> &
operator<<(std::basic_ostream<CharT, Traits> &ost, std::thread::id id);
```

### 1.4 注意

+ `std::thread::id`的实例可以不表示任何线程
+ 线程结束，`std::thread::id`的值可为另一线程复用

## 2. `std::thread`类

### 2.1 概述

`std::thread`类表示单个执行线程，定义在头文件`<thread>`中*（C++11起）*

### 2.2 成员函数

#### 2.2.1 构造函数

```C++
thread() noexcept;
thread(const thread &) = delete; // std::thread对象不能拷贝
thread(thread &&other) noexcept;

template <class Function, class... Args>
explicit thread(Function &&f, Args &...args);
```

构造函数的参数是任何可调用对象，可以为函数名（函数指针）、仿函数（重载`operator()`的类）、*lambda*表达式

+ 函数名（函数指针）

```C++
void func(int number)
{
    std::cout << number << std::endl;
}

std::thread t(func, 100);   // OK
std::thread t1(&func, 100); // OK
```

+ 仿函数

```C++
class background_task
{
public:
    void operator()() const
    {
        std::cout << std::this_thread::get_id() << std::endl;
    }
};

std::thread t(background_task{});  // OK
std::thread t1{background_task()}; // OK
std::thread t2{background_task{}}; // OK
std::thread t3(background_task()); // error
```

+ *lambda*表达式

```C++
std::thread t([]() -> void { std::cout << std::this_thread::get_id() << std::endl; });
```

#### 2.2.2 析构函数

```C++
~thread();
```

+ 若`*this`拥有关联线程（`joinable() == true `），则调用`std::terminate()`
+ 在下列操作后`std::thread`对象无关联的线程（可安全销毁）
    + 被默认构造
    + 被移动
    + 已调用`join()`
    + 已调用`detach()`

#### 2.2.3 赋值运算符

```C++
thread &operator=(thread &&other) noexcept; // std::thread类拷贝赋值运算符被定义为删除的函数
```

+ 若`*this`仍拥有关联的运行中进程（即`joinable() == true`），则调用`std::terminate()` ；否则，赋值`other`的状态给`*this`并设置`other`为默认构造的状态

+ 此调用后，`this->get_id() `等于`other.get_id()`在`operator=`调用前的值，而`other`不再表示执行的线程

#### 2.2.4 观察器

+ `joinable`：检查`std::thread`对象是否标识**活跃**的执行线程。具体而言，若`get_id() != std::thread::id()`则返回`true`

```C++
bool joinable() const noexcept;
```

```C++
#include <iostream>
#include <thread>

void foo()
{
}

int main()
{
    std::cout << std::boolalpha;

    std::thread t;
    std::cout << "before starting, joinable: " << t.joinable() << '\n';

    t = std::thread{foo};
    std::cout << "after starting, joinable: " << t.joinable() << '\n';

    t.join();
    std::cout << "after joining, joinable: " << t.joinable() << '\n';

    t = std::thread{foo};
    t.detach();
    std::cout << "after detaching, joinable: " << t.joinable() << '\n';
}
```

输出：

```TEXT
before starting, joinable: false
after starting, joinable: true
after joining, joinable: false
after detaching, joinable: false
```

+ `get_id`：返回线程ID。若无关联的线程，则返回默认构造的`std::thread::id`

```C++
std::thread::id get_id() const noexcept;
```

```C++
#include <chrono>
#include <iostream>
#include <thread>

void foo()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    std::thread t1(foo);
    std::thread::id t1_id = t1.get_id();

    std::thread t2(foo);
    std::thread::id t2_id = t2.get_id();

    std::cout << "t1's id: " << t1_id << '\n';
    std::cout << "t2's id: " << t2_id << '\n';

    t1.join();
    t2.join();
}
```

可能的输出：

```TEXT
t1's id: 2
t2's id: 3
```

+ `native_handle`：返回实现定义的底层线程柄

```++
native_handle_type native_handle();
```

+ `hardware_concurrency`：返回实现支持的并发线程数

```C++
static unsigned int hardware_concurrency() noexcept;
```

#### 2.2.5 操作

+ `join`：阻塞当前线程直至 `*this`所标识的线程结束其执行

```C++
void join();
```

+ `detach`：使`thread`对象的执行线程独立运行，并在结束时自动释放资源。调用后，`*this`不再关联任何线程。

```C++
void detach();
```

```C++
#include <chrono>
#include <iostream>
#include <thread>
 
void independentThread() 
{
    std::cout << "Starting concurrent thread.\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::cout << "Exiting concurrent thread.\n";
}
 
void threadCaller() 
{
    std::cout << "Starting thread caller.\n";
    std::thread t(independentThread);
    t.detach();
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Exiting thread caller.\n";
}
 
int main() 
{
    threadCaller();
    std::this_thread::sleep_for(std::chrono::seconds(5));
}
```

可能的输出：

```TEXT
Starting thread caller.
Starting concurrent thread.
Exiting thread caller.
Exiting concurrent thread.
```

+ `swap`：交换两个`thread`对象的底层柄

```C++
void swap(std::thread &other) noexcept;
```

### 2.3 非成员函数

+ `std::swap`：交换`lhs`与`rhs`的状态，等效调用`lhs.swap(rhs)`

```C++
void swap(std::thread &lhs, std::thread &rhs) noexcept;
```

