# C++互斥

## 1. `std::mutex`类

### 1.1 作用

`std::mutex`用于保护共享数据免受多个线程同时访问的同步原语，定义在头文件`<mutex>`中*（C++11起）*

### 1.2 构造函数和析构函数

```C++
mutex();
~mutex();
```

### 1.3 锁定

+ `lock`：锁定互斥。若另一线程已锁定互斥，则到`lock`的调用将阻塞执行，直至获得锁

```C++
void lock();
```

+ `try_lock`：尝试锁定互斥，立即返回。成功获得锁时返回`true`，否则返回`false`

```C++
bool try_lock();
```

+ `unlock`：解锁互斥*（互斥必须为当前执行线程所锁定，否则行为未定义）*

```C++
void unlock();
```

### 1.4 `mutex`所有权

+ 一个线程在成功调用`lock`或`try_lock`后，直到调用`unlock`之前，都被视为拥有`mutex`
+ 当一个线程拥有`mutex`时，任何其他试图获取`mutex`所有权的线程都将被阻塞，即`lock`调用将被阻塞，或者`try_lock`将返回`false`
+ 在调用`lock`或`try_lock`之前，调用方线程必须确保自己不拥有`mutex`

### 1.5 注意

+ 通常不直接使用`std::mutex`，`std::unique_lock`、`std::lock_guard`或`std::scoped_lock`*（C++17起）*以更加异常安全的方式管理锁定
+ `std::mutex`既**不可拷贝**也**不可移动**

```C++
mutex(const mutex &) = delete;
mutex &operator=(const mutex &) = delete;
```

## 2. 互斥包装器概览

### 2.1 锁定策略

#### 2.1.1 锁定策略的标记类型

```C++
#include <mutex>

struct defer_lock_t
{
    explicit defer_lock_t() = default;
};
struct try_to_lock_t
{
    explicit try_to_lock_t() = default;
};
struct adopt_lock_t
{
    explicit adopt_lock_t() = default;
};
```

#### 2.1.2 标记类型的含义

|      类型       |               效果               |
| :-------------: | :------------------------------: |
| `defer_lock_t`  |        不获得互斥的所有权        |
| `try_to_lock_t` |   尝试获得互斥的所有权而不阻塞   |
| `adopt_lock_t`  | 假设调用方线程已拥有互斥的所有权 |

### 2.2 锁定概念的比较

|    概念    |                         成员函数要求                         |
| :--------: | :----------------------------------------------------------: |
| 基本可锁定 |                       `lock`、`unlock`                       |
|   可锁定   |                 `lock`、`unlock`、`try_lock`                 |
| 可定时锁定 | `lock`、`unlock`、`try_lock`、`try_lock_for`、`try_lock_until` |

### 2.3 互斥包装器类模板声明

```C++
#include <mutex>

template <class Mutex>
class unique_lock;

template <class Mutex>
class lock_guard;

template <class... MutexTypes>
class scoped_lock;
```

类模板参数要求

|       包装器       |             模板形参（锁）的要求              |
| :----------------: | :-------------------------------------------: |
| `std::unique_lock` |                  基本可锁定                   |
| `std::lock_guard`  |                  基本可锁定                   |
| `std::scoped_lock` | 可锁定（模板参数个数为1时只需满足基本可锁定） |

### 2.4 成员类型

|                           成员类型                           |  定义   |
| :----------------------------------------------------------: | :-----: |
| `mutex_type`（仅当`std::scoped_lock`的`sizeof...(MutexTypes) == 1`时有效） | `Mutex` |

### 2.5 互斥包装器对比分析

|           特性           |           `std::unique_lock`           |   `std::lock_guard`    |   `std::scoped_lock`   |
| :----------------------: | :------------------------------------: | :--------------------: | :--------------------: |
|           标准           |                 C++11                  |         C++11          |         C++17          |
|         锁定策略         | 立即锁定、延迟锁定、尝试锁定和采用锁定 |     只支持立即锁定     |     只支持立即锁定     |
|  是否支持手动锁定和解锁  |                  支持                  |         不支持         |         不支持         |
| 是否能与条件变量配合使用 |                   能                   |          不能          |          不能          |
|  是否支持锁定多个互斥量  |                 不支持                 |         不支持         |          支持          |
|   包装器对象是否可拷贝   |                不可拷贝                |        不可拷贝        |        不可拷贝        |
|   包装器对象是否可移动   |                 可移动                 |           /            |           /            |
|      效率和内存占用      |         效率较低，内存占用稍高         | 效率较高，内存占用较低 | 效率较高，内存占用较低 |

## 3. `std::unique_lock`类

### 3.1 构造函数

```C++
unique_lock() noexcept; // 构造无关联互斥的unique_lock
unique_lock(unique_lock &&other) noexcept;
explicit unique_lock(mutex_type &m);
```

```C++
unique_lock(mutex_type &m, std::defer_lock_t t) noexcept;
unique_lock(mutex_type &m, std::try_to_lock_t t);
unique_lock(mutex_type &m, std::adopt_lock_t t);
```

```C++
template <class Rep, class Period>
unique_lock(mutex_type &m, const std::chrono::duration<Rep, Period> &timeout_duration);
template <class Clock, class Duration>
unique_lock(mutex_type &m, const std::chrono::time_point<Clock, Duration> &timeout_time);
```

### 3.2 析构函数

```C++
~unique_lock(); // 销毁锁。若*this拥有关联互斥且获得了其所有权，则解锁互斥
```

### 3.3 移动赋值运算符

```C++
/**
 * 用移动语义以other的内容替换*this内容。
 * 若调用前*this拥有关联互斥并获得其所有权，则解锁互斥
 */
unique_lock &operator=(unique_lock &&other);
```

### 3.4 锁定

+ `lock`：锁定关联互斥。等效调用`mutex()->lock()`

```C++
void lock();
```

+ `try_lock`：尝试锁定关联互斥而不阻塞。等效调用`mutex()->try_lock()` 

```C++
bool try_lock();
```

+ `try_lock_for`：在指定的时间段内尝试获取互斥

```C++
template <class Rep, class Period>
bool try_lock_for(const std::chrono::duration<Rep, Period> &timeout_duration);
```

```C++
std::timed_mutex tmtx;
std::unique_lock<std::timed_mutex> lck(tmtx, std::defer_lock);

lck.try_lock_for(std::chrono::milliseconds(500));
```

+ `try_lock_until`：在指定的时间点之前尝试获取互斥

```C++
template <class Clock, class Duration>
bool try_lock_until(const std::chrono::time_point<Clock, Duration> &timeout_time);
```

```C++
std::timed_mutex tmtx;
std::unique_lock<std::timed_mutex> lck(tmtx, std::defer_lock);

auto now = std::chrono::steady_clock::now();
lck.try_lock_until(now + std::chrono::seconds(1));
```

+ `unlock`：解锁关联互斥并释放所有权

```C++
void unlock();
```

### 3.5 修改器

+ `swap`：与另一`std::unique_lock` 交换状态

```C++
void swap(unique_lock &other) noexcept;
```

+ `release`：将关联互斥解关联而不解锁它

```C++
mutex_type *release() noexcept;
```

### 3.6 观察器

+ `mutex`：返回指向关联互斥的指针，或若无关联互斥则返回空指针

```C++
mutex_type *mutex() const noexcept;
```

+ `owns_lock`：测试锁是否占有其关联互斥

```C++
bool owns_lock() const noexcept;
```

+ `operator bool`：测试锁是否占有其关联互斥。等效调用`owns_lock()`

```C++
explicit operator bool() const noexcept;
```

```C++
std::mutex mtx;
std::unique_lock<std::mutex> lck(mtx, std::defer_lock);

if (lck)
    std::cout << "Mutex is locked." << std::endl;
else
    std::cout << "Mutex is not locked." << std::endl;
```

## 4. `std::lock_guard`类


### 4.1 构造函数

```C++
explicit lock_guard(mutex_type &m);             // 等效调用m.lock()
lock_guard(mutex_type &m, std::adopt_lock_t t); // 假定当前线程已拥有互斥m的所有权
```

### 4.2 析构函数

```C++
~lock_guard(); // 释放所占有的互斥，等效调用m.unlock()，其中m是传递给lock_guard的构造函数的互斥
```

## 5. `std::scoped_lock`类

### 5.1 构造函数

```C++
explicit scoped_lock(MutexTypes &...m);
scoped_lock(std::adopt_lock_t, MutexTypes &...m); // 假定当前线程已拥有所有传入的互斥的所有权
```

| `sizeof...(MutexTypes)`的值 |           行为           |
| :-------------------------: | :----------------------: |
|              0              |        不做任何事        |
|              1              |    等效调用`m.lock()`    |
|            其它             | 等效调用`std::lock(m..)` |

### 5.2 析构函数

```C++
~scoped_lock(); // 释放所有占有的互斥，相当于对传递给scoped_lock构造函数的每个互斥包中的互斥调用unlock()
```

