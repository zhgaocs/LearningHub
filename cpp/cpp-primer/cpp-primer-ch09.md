# C++ Primer—Chapter 09 序列式容器

## 1. STL六大组件

### 1.1 分配器：空间配置与管理

```C++
template <class T>
struct allocator;

// 分配器使用之处
template <class T, class Allocator = std::allocator<T>>
class vector;
```

### 1.2 迭代器： 统一的访问容器元素的方式，泛型指针

```C++
// 可能的实现
template <class T, class Allocator = std::allocator<T>>
class vector
{
public:
    ...
    typedef T value_type;
    typedef value_type *iterator;
    ....
private:
    iterator start;
    iterator finish;
    ...
        
public:
    iterator begin() { return start;}
    ...
};
```
### 1.3 容器：存储和管理数据

+ 顺序容器
    + `array`*
    + `deque`
    + `forward_list`*
    + `list`
    + `string`
    + `vector`

+ 关联容器
    + `map`
    + `multimap`
    + `set`
    + `multiset`
    + `unordered_map`*
    + `unordered_set`*
    + `unordered_multimap`*
    + `unordered_multiset`*


*注：带\*为自C++11起*

### 1.4 算法：对容器中的数据进行各种操作和处理，如`copy`、`count`、`find`等

### 1.5 仿函数（函数对象）：行为类似函数，可作为算法的某种策略，需重载`operator()`

```C++
template <class T>
struct greater
{
    bool operator()(const T &lhs, const T &rhs) const;
};
```

```C++
#include <algorithm>

std::vector<int> ivec{2, 1, 4, 3, 7};

// 降序排序
std::sort(ivec.begin(), ivec.end(), std::greater<int>());
```

### 1.6 适配器：修饰容器、仿函数和迭代器接口

#### 1.6.1 容器适配器

```C++
template <class T, class Container = std::deque<T>>
class stack;

template <class T, class Container = std::deque<T>>
class queue;

template <class T, class Container = std::vector<T>,
          class Compare = std::less<typename Container::value_type>>
class priority_queue;
```

|      适配器      | 所需头文件 |                 底层容器                  |      元素访问       |         容量         |                       操作                       |
| :--------------: | :--------: | :---------------------------------------: | :-----------------: | :------------------: | :----------------------------------------------: |
|     `stack`      | `<stack>`  | `deque`（默认）<br />`vector`<br />`list` |        `top`        | `empty` <br />`size` | `push`<br />`emplace`\*<br />`pop`<br />`swap`\* |
|     `queue`      | `<queue>`  |        `deque`（默认）<br />`list`        | `front`<br />`back` | `empty`<br />`size`  | `push`<br />`emplace`\*<br />`pop`<br />`swap`\* |
| `priority_queue` | `<queue>`  |       `vector`（默认）<br />`deque`       |        `top`        | `empty`<br />`size`  | `push`<br />`emplace`\*<br />`pop`<br />`swap`\* |


*注：带\*为自C++11起*



|               成员类型                |             定义              |
| :-----------------------------------: | :---------------------------: |
|           `container_type`            |          `Container`          |
| `value_compare`（仅`priority_queue`） | Compare（仅`priority_queue`） |
|             `value_type`              |    `Container::value_type`    |
|              `size_type`              |    `Container::size_type`     |
|              `reference`              |    `Container::reference`     |
|           `const_reference`           | `Container::const_reference`  |

#### 1.6.2 仿函数适配器

#### 1.6.3 迭代器适配器

## 2. 顺序容器

### 2.1 顺序容器概览

~~所有顺序容器都提供了**顺序**访问元素的能力~~，所需头文件与容器同名

|    顺序容器    | 大小能否动态变化 | 能否随机访问元素 |                       特点                        |
| :------------: | :--------------: | :--------------: | :-----------------------------------------------: |
|    `array`     |     &#10006;     |     &#10004;     |     语义上等同于C风格数组，不能添加/删除元素      |
|    `deque`     |     &#10004;     |     &#10004;     |            双端队列，头尾插入/删除很快            |
| `forward_list` |     &#10004;     |     &#10006;     |          单向链表，任意位置插入/删除很快          |
|     `list`     |     &#10004;     |     &#10006;     |          双向链表，任意位置插入/删除很快          |
|    `string`    |     &#10004;     |     &#10004;     | 类似`vector`，专门用于保存字符，尾部插入/删除很快 |
|    `vector`    |     &#10004;     |     &#10004;     |          可变大小数组，尾部插入/删除很快          |

### 2.2 类型别名

| 成员类型                 | 定义                                              |
| ------------------------ | ------------------------------------------------- |
| `value_type`             | `T`                                               |
| `size_type`              | 无符号整数类型（通常是 `std::size_t`）            |
| `difference_type`        | 有符号整数类型（通常是 `std::ptrdiff_t`）         |
| `reference`              | `value_type&`                                     |
| `const_reference`        | `const value_type&`                               |
| `pointer`                | `std::allocator_traits<Allocator>::pointer`       |
| `const_pointer`          | `std::allocator_traits<Allocator>::const_pointer` |
| `iterator`               | LegacyRandomAccessIterator to `value_type`        |
| `const_iterator`         | LegacyRandomAccessIterator to `const value_type`  |
| `reverse_iterator`       | `std::reverse_iterator<iterator>`                 |
| `const_reverse_iterator` | `std::reverse_iterator<const_iterator>`           |

---

### 2.3 `array`

#### 2.3.1 类模板声明

```C++
template <class T, std::size_t N>
struct array;
```

#### 2.3.2 构造函数

```C++
// 聚合初始化
std::array<int, 3> arr1 = {3, 6, 9};
std::array<int, 3> arr2{3, 6, 9}; // C++11
std::array<int, 3> arr3 = {3};    // 3 0 0
std::array<int, 3> arr4(arr1);    // 3 6 9
```

#### 2.3.3 元素访问

+ `at`：访问指定的元素，同时进行越界检查

+ `operator[]`：访问指定的元素

+ `front`：访问第一个元素

+ `back`：访问最后一个元素

+ `data`：直接访问底层数组

```C++
// 若!(pos < size()) 则抛出std::out_of_range
constexpr reference at(size_type pos);
// 不会做边界检查，若越界会导致未定义行为
constexpr reference operator[](size_type pos);

constexpr reference front();
constexpr reference back();

// 返回指向作为元素存储工作的底层数组的指针
constexpr T *data() noexcept;
```

#### 2.3.4 迭代器

+ `begin` `cbegin`：返回指向起始的迭代器

+ `end` `cend`：返回指向末尾的迭代器

+ `rbegin` `crbegin`：返回指向起始的逆向迭代器

+ `rend` `crend`：返回指向末尾的逆向迭代器

```C++
std::array<int, 4> iarr{1, 2, 3, 4};

for (auto rit = iarr.crbegin(); rit != iarr.crend(); ++rit)
    std::cout << *rit << ' '; // 4 3 2 1
```

#### 2.3.5 容量

+ `empty`：检查容器是否为空
+ `size`：返回容纳的元素数
+ `max_size`：返回可容纳的最大元素数。因为每个`std::array<T, N>`都是固定大小容器，故`max_size` 返回的值等于 `N` （亦为`size`返回的值）

#### 2.3.6 操作

+ `fill`：以指定值填充容器

```C++
constexpr void fill(const T &value);
```

+ `swap`：交换内容。将容器内容与 `other` 的内容交换。**不会导致迭代器和引用关联到别的容器**

```C++
void swap(array &other) noexcept;
```
```C++
std::array<int, 3> a1{1, 2, 3}, a2{4, 5, 6};

auto it1 = a1.begin();
auto it2 = a2.begin();
int &ref1 = a1[1];
int &ref2 = a2[1];

a1.swap(a2);

std::cout << *it1 << ' ' << *it2 << std::endl; // 4 1
std::cout << ref1 << ' ' << ref2 << std::endl; // 5 2
```
---

### 2.4 `deque`

#### 2.4.1 类模板声明

```C++
template <class T, class Allocator = std::allocator<T>>
class deque;
```

#### 2.4.2 构造函数

```C++
deque();

deque(size_type count, const T &value, const Allocator &alloc = Allocator());

explicit deque(size_type count);

template <class InputIt>
deque(InputIt first, InputIt last, const Allocator &alloc = Allocator()); // [first, last)

deque(const deque &other);

deque(deque &&other); // C++11

eque(std::initializer_list<T> init, const Allocator &alloc = Allocator()); // C++11
```

#### 2.4.3 元素访问

+ `at` 
+ `operator[]`
+ `front`
+ `back`

#### 2.4.4 迭代器

+ `begin` `cbegin`

+ `end` `cend`

+ `rbegin` `crbegin`

+ `rend` `crend`

#### 2.4.5 容量

+ `empty`
+ `size`
+ `max_size`
+ `shrink_to_fit`：通过释放未使用的内存减少内存的使用

```C++
void shrink_to_fit();
```

#### 2.4.6 修改

+ `clear`：清除内容
+ `insert`：插入元素
```C++
// 在pos前插入value
iterator insert(const_iterator pos, const T &value);
iterator insert(const_iterator pos, T &&value);

// 在pos前插入value的count个副本
iterator insert(const_iterator pos, size_type count, const T &value);

// 在pos前插入来自范围[first, last)的元素
template <class InputIt>
iterator insert(const_iterator pos, InputIt first, InputIt last);

//  在pos前插入来自initializer_list ilist的元素
iterator insert(const_iterator pos, std::initializer_list<T> ilist);
```
+ `emplace`：原位构造元素
```C++
// 直接于pos前插入元素到容器中
template <class... Args>
iterator emplace(const_iterator pos, Args &&...args);
```
+ `erase`：擦除元素

```C++
// iterator是最后移除元素之后的迭代器
iterator erase(const_iterator pos);

// 移除[first, last)中的元素
iterator erase(const_iterator first, const_iterator last);
```

```C++
std::deque<int> ideq = {0, 1, 2, 3, 4, 5};

auto it = ideq.erase(ideq.cbegin() + 2);
// ideq: {0, 1, 3, 4, 5}
// *it == 2

ideq.erase(ideq.cbegin(), ideq.cbegin() + 3);
// ideq: {4, 5}
```

+ `push_back`：将元素添加到容器末尾
+ `emplace_back`：在容器末尾就地构造元素
+ `pop_back`：移除末元素
+ `push_front`：插入元素到容器起始
+ `emplace_front`：在容器头部原位构造元素
+ `pop_front`：移除首元素

```C++
void push_back(T &&value);

template <class... Args>
void emplace_back(Args &&...args);

void pop_back();

void push_front(T &&value);

template <class... Args>
void emplace_front(Args &&...args);

void pop_front();
```

+ `resize`：改变容器中可存储元素的个数

```C++
// 重设容器大小以容纳 count 个元素，在count == size()时不做任何事
// 如果当前大小大于count，那么减小容器到它的开头count个元素
// 如果当前大小小于count，那么后附额外的默认插入元素/value副本
void resize(size_type count);
void resize(size_type count, const value_type &value);
```

+ `swap`：交换内容

#### 2.4.7 其他成员函数

+ `assign`：将值赋给容器

```C++
void assign(size_type count, const T &value);

template <class InputIt>
void assign(InputIt first, InputIt last); // [first, last)

void assign(std::initializer_list<T> ilist);
```

+ `get_allocator`：返回相关的分配器

```C++
allocator_type get_allocator() const noexcept;
```

---

### 2.5 `forward_list`

#### 2.5.1 类模板声明

```C++
template <class T, class Allocator = std::allocator<T>>
class forward_list;
```

#### 2.5.2 构造函数

```C++
// 全部自C++11
forward_list();

forward_list(size_type count, const T &value, const Allocator &alloc = Allocator());

explicit forward_list(size_type count);

template <class InputIt>
forward_list(InputIt first, InputIt last, const Allocator &alloc = Allocator());

forward_list(const forward_list &other);

forward_list(forward_list &&other); 

forward_list(std::initializer_list<T> init, const Allocator &alloc = Allocator());
```

#### 2.5.3 元素访问

+ `front`

#### 2.5.4 迭代器

+ `before_begin` `cbefore_begin`：返回指向第一个元素之前迭代器
+ `begin` `cbegin`
+ `end` `cend`

*注：无反向迭代器，迭代器只支持**自增**运算*

#### 2.5.5 容量

+ `empty`
+ `max_size`

*注：不提供`size`*

#### 2.5.6 修改

+ `clear`
+ `insert_after`：在某个元素后插入新元素
+ `emplace_after`：在元素后原位构造元素
+ `erase_after`：擦除元素后的元素
+ `push_front`
+ `emplace_front`
+ `pop_front`

```C++
std::forward_list<int> ifwlist{1, 2, 3};

ifwlist.emplace_after(ifwlist.cbefore_begin(), 0); // ifwlist: {0, 1, 2, 3}
```

+ `resize`
+ `swap`

#### 2.5.7 操作

+ `merge`：合并二个**已排序**列表。操作后`other`会变为空
```C++
// 链表应以升序排序，默认调用operator<比较元素
void merge(forward_list &other);
void merge(forward_list &&other);

// 使用给定的比较函数
template <class Compare>
void merge(forward_list &other, Compare comp);

template <class Compare>
void merge(forward_list &&other, Compare comp);

// 比较函数的签名应等价于如下
bool cmp(const Type1 &a, const Type2 &b);
```
```C++
std::forward_list<int> ifwlist{0, 2, 4, 5}, other{1, 3};

ifwlist.merge(other); // ifwlist: {0, 1, 2, 3, 4, 5}

if (other.empty())
    std::cout << "empty" << std::endl; // empty
```

```C++
std::forward_list<int> ifwlist{5, 4, 2, 0}, other{3, 1};

ifwlist.merge(other, std::greater<int>()); // ifwlist: {5, 4, 3, 2, 1, 0}
```

+ `splice_after`：从另一`forward_list`移动元素

```C++
// other所有元素移动到*this的pos后
void splice_after(const_iterator pos, forward_list &other);
void splice_after(const_iterator pos, forward_list &&other);

// it为待插入元素的前向迭代器
void splice_after(const_iterator pos, forward_list &other, const_iterator it);
void splice_after(const_iterator pos, forward_list &&other, const_iterator it);

// (first, last)
void splice_after(const_iterator pos, forward_list &other, const_iterator first, const_iterator last);
void splice_after(const_iterator pos, forward_list &&other, const_iterator first, const_iterator last);
```

```C++
std::forward_list<int> ifwlist{0, 1, 2, 3}, other{4, 5, 6};

ifwlist.splice_after(ifwlist.cbefore_begin(), other, other.cbegin()); // ifwlist: {5, 0, 1, 2, 3}
```

```C++
std::forward_list<int> ifwlist{0, 1, 2, 3}, other{4, 5, 6};

ifwlist.splice_after(ifwlist.cbefore_begin(), other, other.cbegin(), other.cend()); // ifwlist: {5, 6, 0, 1, 2, 3}
```

+ `remove` `remove_if`：移除满足特定标准的元素

```C++
void remove(const T &value);

// p：若应该移除该元素则返回true的一元谓词
template <class UnaryPredicate>
void remove_if(UnaryPredicate p);
```

```C++
struct Point
{
    int _x;
    int _y;

    Point(int x, int y) : _x(x), _y(y) {}
};

inline bool isOnYAxis(const Point &p)
{
    return !p._x;
}

std::forward_list<Point> fwlist{{0, 1}, {1, 0}, {1, 1}, {0, 2}};

fwlist.remove_if(isOnYAxis); // fwlist: {{1, 0}, {1, 1}}
```

+ `reverse`：将该链表的所有元素的顺序反转

```C++
void reverse() noexcept;
```

+ `unique`：删除**连续**的重复元素

```C++
// 用operator==比较元素
void unique();

// 用二元谓词p比较元素
template <class BinaryPredicate>
void unique(BinaryPredicate p);
```

```C++
struct Person
{
    int birth_year;
    int birth_month;

    Person(int year, int month) : birth_year(year), birth_month(month) {}
};

bool isSameAge(const Person &lhs, const Person &rhs)
{
    if (lhs.birth_year == rhs.birth_year)
        return true;
    else
        return false;
}

std::forward_list<Person> fwlist{{2001, 1}, {2000, 3}, {2001, 1}, {2001, 7}, {2001, 3}, {2002, 5}};

fwlist.unique(isSameAge); // fwlist: {{2001, 1}, {2000, 3}, {2001, 1}, {2002, 5}}
```

+ `sort`：对元素进行排序

```C++
void sort(); // 升序，默认调用operator<比较元素

// 使用给定的比较函数
template <class Compare>
void sort(Compare comp);
```

```C++
std::forward_list<int> ifwlist{1, 0, 4, 3, 2, 5};

ifwlist.sort(); // ifwlist: {0, 1, 2, 3, 4, 5}

ifwlist.sort(std::greater<int>()); // ifwlist: {5, 4, 3, 2, 1, 0}
```

#### 2.5.8 其他成员函数

同`deque`

---

### 2.6 `list`

#### 2.6.1 类模板声明

```C++
template <class T, class Allocator = std::allocator<T>>
class list;
```

#### 2.6.2 构造函数

```C++
list();

explicit list(size_type count, const T &value = T(), const Allocator &alloc = Allocator());

explicit list(size_type count);

template <class InputIt>
list(InputIt first, InputIt last, const Allocator &alloc = Allocator()); // [first, last)

list(const list &other);

list(list &&other); // C++11

list(std::initializer_list<T> init, const Allocator &alloc = Allocator()); // C++11
```

#### 2.6.3 元素访问

+ `front`

+ `back`

#### 2.6.4 迭代器

+ `begin` `cbegin`

+ `end` `cend`

+ `rbegin` `crbegin`

+ `rend` `crend`

#### 2.6.5 容量

+ `empty`
+ `size`
+ `max_size`

#### 2.6.6 修改

同`deque`

#### 2.6.7 操作

同`forward_list`

#### 2.6.8 其他成员函数

同`deque`

---

### 2.7 `string`

#### 2.7.1 类模板声明

```C++
template <class CharT, class Traits = std::char_traits<CharT>, class Allocator = std::allocator<CharT>>
class basic_string;

typedef basic_string<char> string;
```

#### 2.7.2 构造函数

```C++
basic_string();

basic_string(size_type count, CharT ch, const Allocator &alloc = Allocator());

// [pos, other.size())
basic_string(const basic_string &other, size_type pos, const Allocator &alloc = Allocator());               

// [pos, pos + count)
basic_string(const basic_string &other, size_type pos, size_type count, const Allocator &alloc = Allocator());

// [s, s + count)
basic_string(const CharT *s, size_type count, const Allocator &alloc = Allocator());

basic_string(const CharT *s, const Allocator &alloc = Allocator());

// [first, last) 
template <class InputIt>
basic_string(InputIt first, InputIt last, const Allocator &alloc = Allocator());

basic_string(const basic_string &other);

basic_string(basic_string &&other) noexcept; // C++11

basic_string(std::initializer_list<CharT> ilist, const Allocator &alloc = Allocator()); // C++11
```

```C++
#include <utility>

const char *s = "abcdefg";

std::string s1;                                   // 空字符串
std::string s2(s);                                // "abcdefg"
std::string s3(3, 'a');                           // "aaa"
std::string s4(s2, 2);                            // "cdefg"
std::string s5(s2, 2, 3);                         // "cde", [pos, pos + count)
std::string s6(s, 4);                             // "abcd", [s, s + count)
std::string s7(s2.cbegin() + 2, s2.cbegin() + 6); // "cdef", [first, last)
std::string s8(s2);                               // "abcdefg", copy constructor
std::string s9(std::move(s2));                    // move constructor，s2的值未定义，s9为"abcdefg"
```

#### 2.7.3 元素访问

+ `at`
+ `operator[]`
+ `front`
+ `back`
+ `data`
+ `c_str`：将`string`对象转换为C风格字符串，`c_str`与 `data`功能相同。通过 `c_str()` 写入字符数组是未定义行为

```C++
const CharT* c_str() const noexcept;
```

```C++
void print(const char* str)
{
    for(int i = 0; '\0' != str[i]; ++i)
        std::cout << str[i];
    std::cout << std::endl;
}

std::string s("Hello World");
print(s.c_str()); //  Hello World
```

#### 2.7.4 迭代器

+ `begin` `cbegin`
+ `end` `cend`
+ `rbegin` `crbegin`
+ `rend` `crend`

#### 2.7.5 容量

+ `empty`
+ `size` `lenghth`：返回字符数
+ `max_size`
+ `reserve`：设置预留空间大小，避免不断重新分配内存
+ `capacity`：返回当前对象分配的存储空间能保存的字符数量

+ `shrink_to_fit`：通过释放不使用内存减少内存使用，减少`capacity`到`size`的非强制请求，**是否满足请求取依赖于实现**

```C++
void reserve(size_type new_cap = 0);
size_type capacity() const noexcept;
void shrink_to_fit();
```

```C++
std::string s(20, 'a');
std::cout << s.capacity() << std::endl;
s.reserve(50);
std::cout << s.capacity() << std::endl;
s.shrink_to_fit();
std::cout << s.capacity() << std::endl;
/*
可能的输出
20
50
20
/*
```

#### 2.7.6 操作

+ `clear`
+ `insert`
+ `erase`
+ `push_back`
+ `pop_back`
+ `append`
+ `operator+=`

```C++
basic_string &operator+=(const basic_string &str);
basic_string &operator+=(CharT ch);
basic_string &operator+=(const CharT *s);
basic_string &operator+=(std::initializer_list<CharT> ilist);
```

```C++
std::string s("Hello"), s1(" Worl");
char ch('d');
const char *str("Hello World");

s += s1; // "Hello Worl"
s += ch; // "Hello World"
s.clear();
s += str;
s += {'!', '!', '!'}; // "Hello World!!!"
```

+ `compare`

```C++
int compare(const basic_string &str) const noexcept;
int compare(size_type pos1, size_type count1, const basic_string &str) const;
int compare(size_type pos1, size_type count1, const basic_string &str, size_type pos2, size_type count2) const;
int compare(const CharT *s) const;
int compare(size_type pos1, size_type count1, const CharT *s) const;
int compare(size_type pos1, size_type count1, const CharT *s, size_type count2) const;
```

+ `starts_with`*(C++20)*
+ `ends_with`*(C++20)*
+ `contains`*(C++23)*

```C++
constexpr bool contains(CharT c) const noexcept;
constexpr bool contains(const CharT *s) const;
```

+ `replace`

```C++
basic_string &replace(size_type pos, size_type count, const basic_string &str);
basic_string &replace(const_iterator first, const_iterator last, const basic_string &str);
basic_string &replace(size_type pos, size_type count, const basic_string &str, size_type pos2, size_type count2);
basic_string &replace(size_type pos, size_type count, const CharT *cstr, size_type count2);
basic_string &replace(const_iterator first, const_iterator last, const CharT *cstr, size_type count2);
basic_string &replace(size_type pos, size_type count, const CharT *cstr);
basic_string &replace(const_iterator first, const_iterator last, const CharT *cstr);
basic_string &replace(size_type pos, size_type count, size_type count2, CharT ch);
basic_string &replace(const_iterator first, const_iterator last, size_type count2, CharT ch);

template <class InputIt>
basic_string &replace(const_iterator first, const_iterator last, InputIt first2, InputIt last2);

basic_string &replace(const_iterator first, const_iterator last, std::initializer_list<CharT> ilist);
```

+ `substr`：返回子串

```C++
basic_string substr(size_type pos = 0, size_type count = npos) const; // [pos, pos + count)
```

```C++
std::string s("Hello World");

std::cout << s.substr(2, 3) << std::endl; // llo
```

+ `copy`：复制字符，**产生的字符串不是空终止的**

```C++
size_type copy(CharT *dest, size_type count, size_type pos = 0) const; // 复制子串[pos, pos + count)到dest
```

```C++
#include <cstring>

char arr[6];
memset(arr, 'a', sizeof(arr) / sizeof(arr[0]));

std::string s("Hello World");
s.copy(arr, 5);
std::cout << arr << std::endl; // Helloa
```

+ `resize`
+ `swap`

#### 2.7.7 查找

+ `find`
+ `rfind`
+ `find_first_of`
+ `find_first_not_of`
+ `find_last_of`
+ `find_last_not_of`

```C++
// 其他五个只有函数名不同
size_type find(const basic_string &str, size_type pos = 0) const noexcept;
size_type find(const CharT *s, size_type pos, size_type count) const;
size_type find(const CharT *s, size_type pos = 0) const;
size_type find(CharT ch, size_type pos = 0) const noexcept;
```

#### 2.7.8 常量

+ `npos`

```C++
static const size_type npos = -1; // 特殊值，等于size_type类型可表示的最大值
```

```C++
std::string s = "test";
if (s.find('a') == std::string::npos)
    std::cout << "no 'a' in 'test'\n";
```

#### 2.7.9 数制转换（非成员函数）

所需头文件`<string>`

+ `stoi`*(C++11)*
+ `stol`*(C++11)*
+ `stoll`*(C++11)*
+ `stoul` `stoull`*(C++11)*
+ `stof` `stod` `stold`*(C++11)*
+ `to_string`*(C++11)*

```C++
int stoi(const std::string &str, std::size_t *pos = 0, int base = 10); // 底的合法集是{0,2,3,...,36}
...
float stof(const std::string &str, std::size_t *pos = 0);
...
std::string to_string(int value);
```
```C++
std::string s("111");

std::cout << std::stoi(s, nullptr, 2) << std::endl; // 7

std::cout << std::to_string(1.2e+4) << std::endl; // 12000.000000
```
#### 2.7.10 其他成员函数

同`deque`

### 2.8 `vector`

#### 2.8.1 类模板声明

```C++
template <class T, class Allocator = std::allocator<T>>
class vector;
```

#### 2.8.2 构造函数

```C++
vector();

vector(size_type count, const T &value, const Allocator &alloc = Allocator());

explicit vector(size_type count);

template <class InputIt>
vector(InputIt first, InputIt last, const Allocator &alloc = Allocator());

vector(const vector &other);

vector(vector &&other); // C++11

vector(std::initializer_list<T> init, const Allocator &alloc = Allocator()); // C++11
```

#### 2.8.3 元素访问

+ `at`

+ `operator[]`

+ `front`

+ `back`

+ `data`

#### 2.8.4 迭代器

+ `begin` `cbegin`
+ `end` `cend`
+ `rbegin` `crbegin`
+ `rend` `crend`

#### 2.8.5 容量

+ `empty`
+ `size` 
+ `max_size`
+ `reserve`
+ `capacity`
+ `shrink_to_fit`

#### 2.8.6 修改

+ `clear`
+ `insert`
+ `emplace`*(C++11)*
+ `erase`
+ `push_back`
+ `emplace_back`*(C++11)*
+ `pop_back`
+ `resize`
+ `swap`

#### 2.8.7 特化

`vector<bool>` ：节省空间的动态`bitset `

#### 2.8.8 其他成员函数

同`deque`