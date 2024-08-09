# C++ 条件变量

## 1. 枚举`std::cv_status`

### 1.1 作用

`std::cv_status`用于描述定时等待是否因时限返回，在 `<condition_variable>` 头文件中定义

### 1.2 定义

```C++
enum class cv_status
{
    no_timeout, // 条件变量因 notify_all、notify_one 或虚假地被唤醒
    timeout     // 条件变量因时限耗尽被唤醒
};
```

## 2. `std::condition_variable`类

### 2.1 作用

`std::condition_variable` 是一种同步原语，用于阻塞线程直到满足特定条件或收到其他线程的通知

### 2.2 构造函数和析构函数

```C++
condition_variable();
condition_variable(const condition_variable &) = delete;

~condition_variable();
```

### 2.3 赋值运算符

```C++
condition_variable &operator=(const condition_variable &) = delete;
```

### 2.4 通知

+ `notify_one`：通知一个等待的线程

```C++
void notify_one() noexcept;
```

+ `notify_all`：通知所有等待的线程

```C++
void notify_all() noexcept;
```

### 2.5 等待

+ `wait`：阻塞当前线程，直到条件变量被唤醒

```C++
void wait(std::unique_lock<std::mutex> &lock);

template <class Predicate>
void wait(std::unique_lock<std::mutex> &lock, Predicate pred);
```

+ `wait_for`：阻塞当前线程，直到条件变量被唤醒，或到指定时限**时长**后

```C++
template <class Rep, class Period>
std::cv_status wait_for(std::unique_lock<std::mutex> &lock,
                        const std::chrono::duration<Rep, Period> &rel_time);

template <class Rep, class Period, class Predicate>
bool wait_for(std::unique_lock<std::mutex> &lock,
              const std::chrono::duration<Rep, Period> &rel_time,
              Predicate pred);
```

+ `wait_until`：阻塞当前线程，直到条件变量被唤醒，或直到抵达指定**时间点**

```C++
template <class Clock, class Duration>
std::cv_status wait_until(std::unique_lock<std::mutex> &lock,
                          const std::chrono::time_point<Clock, Duration> &abs_time);

template <class Clock, class Duration, class Predicate>
bool wait_until(std::unique_lock<std::mutex> &lock,
                const std::chrono::time_point<Clock, Duration> &abs_time,
                Predicate pred);
```

### 2.6 原生句柄

```C++
native_handle_type native_handle();
```

### 2.7 示例

```C++
std::mutex mutex;
std::condition_variable cv;
std::string data;
bool ready = false;
bool processed = false;

void worker_thread()
{
    // wait until main() sends data
    std::unique_lock<std::mutex> lock(mutex);
    cv.wait(lock, []
            { return ready; });

    // after the wait, we own the lock
    std::cout << "Worker thread is processing data\n";
    data += " after processing";

    // send data back to main()
    processed = true;
    std::cout << "Worker thread signals data processing completed\n";

    // manual unlocking is done before notifying, to avoid waking up
    // the waiting thread only to block again (see notify_one for details)
    lock.unlock();
    cv.notify_one();
}

int main()
{
    std::thread worker(worker_thread);

    data = "Example data";
    // send data to the worker thread
    {
        std::lock_guard<std::mutex> lock(mutex);
        ready = true;
        std::cout << "main() signals data ready for processing\n";
    }
    cv.notify_one();

    // wait for the worker
    {
        std::unique_lock<std::mutex> lock(mutex);
        cv.wait(lock, []
                { return processed; });
    }
    std::cout << "Back in main(), data = " << data << '\n';

    worker.join();
}
```

输出：

```TEXT
main() signals data ready for processing
Worker thread is processing data
Worker thread signals data processing completed
Back in main(), data = Example data after processing
```

## 3. `std::condition_variable_any`类

+ **泛化条件变量**: `std::condition_variable_any`是`std::condition_variable`的泛化版本
+ **锁兼容性**: 可以与任何满足**可基本锁定**要求的锁一起使用
+ **性能注意事项**: 如果使用`std::unique_lock<std::mutex>`，`std::condition_variable`可能提供更好的性能
+ **特殊用途**: 可以与`std::shared_lock`结合，在`std::shared_mutex`上以共享所有权模式等待，或用于自定义锁实现可中断等待

