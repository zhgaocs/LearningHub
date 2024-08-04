# C++关键字—`alignas`

## 1. 标准

C++11

## 2. 作用

指定类型或对象的对齐要求

## 3. 语法

```C++
alignas(expression); // 表达式必须是一个整型常量表达式，其值为零或者是合法的对齐或扩展对齐
alignas(type-id);    // 等价于alignas(alignof(类型标识))，即将对齐方式设置为指定类型的对齐要求
alignas(pack...);    // 等价于为模板参数包中每个模板参数使用alignas
```

## 4. `alignas`用法

### 4.1 类的声明或定义

```C++
alignas(8) struct S; // 用于类的声明
alignas(8) struct X
{
    S s;
}; // 用于类的定义
```

### 4.2 非位域类数据成员的声明

```C++
struct S
{
    alignas(16) int x;
    double y;
}; // alignof(S) == 16
```

位域类数据成员是按位定义的结构体成员，可指定其占用的位数

```C++
struct Date {
    unsigned int day   : 5;  // 位域类型数据成员，占用5位，表示1-31
    unsigned int month : 4;  // 位域类型数据成员，占用4位，表示1-12
    unsigned int year  : 11; // 位域类型数据成员，占用11位，表示0-2047
};
```

### 4.3 变量声明

除了不能应用于下列内容

+ 函数形参

+ `catch`子句的异常形参

## 5. 注意

+ 有效的对齐应为2的幂

+ `alignas`不能弱化原生对齐

```C++
struct alignas(4) S
{
    float f;
    double d;
}; // alignof(S) == 8
```

+ 最大的有效`alignas`声明生效，其余被忽略

```C++
alignas(32) alignas(16) int i; // alignof(i) == 32
```

+ 始终忽略`alignas(0)`

