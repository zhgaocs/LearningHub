# C++ 单例模式

## 1. 概念

单例模式确保一个类只有一个实例，并提供一个全局访问点

## 2. 实现

```C++
// 加入锁，确保线程安全
class Mutex
{
public:
    Mutex();
    ~Mutex();
    void request();
    void release();
    ...
};
```

```C++
// singleton.h
class Singleton
{
public:
    static Singleton *getInstance();
    static void destroyInstance();
	...
    
#if __cplusplus >= 201103L // C++11及以后
public:
    Singleton(const Singleton &) = delete;
    Singleton(Singleton &&) = delete;
    Singleton &operator=(const Singleton &) = delete;
    Singleton &operator=(Singleton &&) = delete;
#else
private:
    // 拷贝构造函数和拷贝赋值运算符声明在private段，不可定义
    Singleton(const Singleton &);
    Singleton &operator=(const Singleton &);
#endif

private:
    /**
     * 构造函数和析构函数必须声明在private段，且必须在源文件中实现
     * 构造函数和析构函数不能被定义为删除的函数，因为需要在类其他函数中new和delete指向本类对象的指针
     */
    Singleton();
    ~Singleton();

private:
    static Singleton *instance_ptr;
    static Mutex mutex;
};

// 静态非常量成员必须类外初始化
Singleton *Singleton::instance_ptr = nullptr;
Mutex Singleton::mutex;
```

```C++
// singleton.cpp
Singleton *Singleton::getInstance()
{
    if (nullptr == instance_ptr)
    {
        mutex.request();
        if (nullptr == instance_ptr) // 两个if语句实现双重检查锁定模式
            instance_ptr = new Singleton;
        mutex.release();
    }
    return instance_ptr;
}

void Singleton::destroyInstance()
{
    if (nullptr != instance_ptr)
    {
        mutex.request();
        if (nullptr != instance_ptr)
        {
            delete instance_ptr;
            instance_ptr = nullptr;
        }
        mutex.release();
    }
}
```

## 3. 使用

```C++
#include "singleton.h"

int main()
{
    Singleton *si_ptr = Singleton::getInstance();
    ...
    si_ptr->destroyInstance();
}
```



