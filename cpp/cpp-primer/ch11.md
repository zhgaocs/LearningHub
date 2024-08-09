# C++ Primer—Chapter 11 关联式容器

## 1. 辅助类`pair`

### 1.1 头文件

`<utility>`

### 1.2 类模板声明

```C++
template <class T1, class T2>
struct pair;
```

### 1.3 成员类型

|   成员类型    | 定义 |
| :-----------: | :--: |
| `first_type`  | `T1` |
| `second_type` | `T2` |

### 1.4 成员对象

|  成员名  | 类型 |
| :------: | :--: |
| `first`  | `T1` |
| `second` | `T2` |

### 1.5 成员函数

#### 1.5.1 构造函数

```C++
constexpr pair();

pair(const T1 &x, const T2 &y);

template <class U1, class U2>
pair(U1 &&x, U2 &&y);

template <class U1, class U2>
pair(const pair<U1, U2> &p);

template <class U1, class U2>
pair(pair<U1, U2> &&p);

pair(const pair &p) = default; // copy constructor

pair(pair &&p) = default; // move constructor
```

```C++
std::pair<int, int> p1; // 0 0
std::pair<double, double> p2(1.1, 1.9);
std::pair<int, int> p3(p2); // 1 1
```

#### 1.5.2 `operator=`：赋值内容

```C++
pair &operator=(const pair &other);

template <class U1, class U2>
pair &operator=(const pair<U1, U2> &other);

pair &operator=(pair &&other) noexcept;

template <class U1, class U2>
pair &operator=(pair<U1, U2> &&other);
```

#### 1.5.3 `swap`：交换内容

```C++
void swap(pair &other) noexcept;
```

### 1.6 非成员函数

#### 1.6.1 `make_pair`：创建一个 `pair` 对象，其类型根据各实参类型定义

```C++
template <class T1, class T2>
std::pair<T1, T2> make_pair(T1 &&t, T2 &&u);
```

```C++
auto p = std::make_pair(0, 1);
std::cout << p.first << ' ' << p.second << std::endl; // 0 1
```

#### 1.6.2 `std::swap`：特化`std::swap`算法

```C++
template <class T1, class T2>
void swap(std::pair<T1, T2> &x, std::pair<T1, T2> &y) noexcept(noexcept(x.swap(y)));
```

#### 1.6.3 `std::get`：访问`pair`的元素

```C++
template <std::size_t I, class T1, class T2>
typename std::tuple_element<I, std::pair<T1, T2>>::type &
get(std::pair<T1, T2> &p) noexcept; // I为0或1

// 获取T.first的引用
template <class T, class U>
constexpr T &get(std::pair<T, U> &p) noexcept; // T和U必须为不同类型

// 获取T.second的引用
template <class T, class U>
constexpr T &get(std::pair<U, T> &p) noexcept; // T和U必须为不同类型
```

```C++
auto p = std::make_pair(0, 1.2f);
std::cout << std::get<0>(p) << ' ' << std::get<float>(p) << std::endl; // 0 1.2
```

## 2. 关联容器

### 2.1 概述

+ 有序关联容器
    + `map`
    + `set` 
    + `multimap`
    + `multiset`

+ 无序关联容器
    + `unordered_map`
    + `unordered_set`
    + `unordered_multimap`
    + `unordered_multiset`


|         容器         |     头文件      |  实现  |  元素  | 键是否唯一 |
| :------------------: | :-------------: | :----: | :----: | :--------: |
|        `map`         |      `map`      | 红黑树 | 键值对 |  &#10004;  |
|        `set`         |      `set`      | 红黑树 |   键   |  &#10004;  |
|      `multimap`      |      `map`      | 红黑树 | 键值对 |  &#10006;  |
|      `multiset`      |      `set`      | 红黑树 |   键   |  &#10006;  |
|   `unordered_map`    | `unordered_map` | 哈希表 | 键值对 |  &#10004;  |
|   `unordered_set`    | `unordered_set` | 哈希表 |   键   |  &#10004;  |
| `unordered_multimap` | `unordered_map` | 哈希表 | 键值对 |  &#10006;  |
| `unordered_multiset` | `unordered_set` | 哈希表 |   键   |  &#10006;  |

### 2.2 成员类型

| 成员类型      | 定义                             |
| ------------- | -------------------------------- |
| `key_type`    | `Key`                            |
| `mapped_type` | `T`                              |
| `value_type`  | `std::pair<const Key, T>`        |
| `iterator`    | 指向`value_type`的老式双向迭代器 |

### 2.3 `map`

#### 2.3.1 类模板声明

```C++
template <class Key, class T, class Compare = std::less<Key>,
          class Allocator = std::allocator<std::pair<const Key, T>>>
class map;
```

#### 2.3.2 构造函数

```C++
map();

// [first, last)
template <class InputIt>
map(InputIt first, InputIt last, const Compare &comp = Compare(), const Allocator &alloc = Allocator());

map(const map &other);
map(map &&other);

map(std::initializer_list<value_type> init, const Compare &comp = Compare(), const Allocator &alloc = Allocator());
```

```C++
std::map<char, int> m1;
std::map<char, int> m2{{'a', 97}, {'b', 98}, {'c', 99}};

auto it = m2.cbegin();
++it;
std::map<char, int> m3(m2.cbegin(), it); // m3:{{'a', 97}}
```

#### 2.3.3 元素访问

+ `at`：访问指定的元素，同时进行越界检查
+ `operator[]`：访问**或插入**指定的元素

```C++
std::map<char, int> ascii{{'a', 97}, {'b', 98}, {'c', 99}};
std::cout << ascii.at('c') << std::endl; // 99
std::cout << ascii['a'] << std::endl;    // 97
std::cout << ascii['d'] << std::endl;    // 0
```

#### 2.3.4  迭代器

+ `begin` `cbegin`：返回指向起始的迭代器
+ `end` `cend`：返回指向末尾的迭代器
+ `rbegin` `crbegin`：返回指向起始的逆向迭代器
+ `rend` `crend`：返回指向末尾的逆向迭代器

#### 2.3.5 容量

+ `empty`：检查容器是否为空

+ `size`：返回容纳的元素数
+ `max_size`：返回可容纳的最大元素数

#### 2.3.6 修改

+ `clear`：清除内容
+ `insert`：插入元素或结点

```C++
std::pair<iterator, bool> insert(const value_type &value);
template <class P>
std::pair<iterator, bool> insert(P &&value); // C++11
std::pair<iterator, bool> insert(value_type &&value); // C++17

// 插入value到尽可能接近正好在pos之前的位置
iterator insert(const_iterator pos, const value_type &value);

template <class P>
iterator insert(const_iterator pos, P &&value); // C++11
iterator insert(const_iterator pos, value_type &&value); // C++17

template <class InputIt>
void insert(InputIt first, InputIt last); // [first, last)

void insert(std::initializer_list<value_type> ilist); // C++11
```

```C++
std::map<char, int> ascii{{'a', 97}, {'b', 98}, {'c', 99}};
ascii.insert({'d', 100});
ascii.insert(ascii.cbegin(), {'e', 101});
ascii.insert({{'f', 102}, {'g', 103}});
```

+ `insert_or_assign`*(C++17)*：插入元素，或若键已存在则赋值给当前元素

```C++
template <class M>
std::pair<iterator, bool> insert_or_assign(const key_type &k, M &&obj);

template <class M>
std::pair<iterator, bool> insert_or_assign(key_type &&k, M &&obj);

template <class M>
iterator insert_or_assign(const_iterator hint, const key_type &k, M &&obj);

template <class M>
iterator insert_or_assign(const_iterator hint, key_type &&k, M &&obj);
```

```C++
std::map<char, int> ascii{{'a', 96}, {'b', 98}, {'c', 99}};
ascii.insert_or_assign('a', 97);  // ascii:{{'a', 97}, {'b', 98}, {'c', 99}}
ascii.insert_or_assign('d', 100); // ascii:{{'a', 97}, {'b', 98}, {'c', 99}, {'d', 100}}
```

+ `emplace`*(C++11)*：原位构造元素

```C++
template <class... Args>
std::pair<iterator, bool> emplace(Args &&...args);
```

```C++
std::map<char, int> ascii{{'a', 97}, {'b', 98}, {'c', 99}};
ascii.emplace('d', 100); // ascii:{{'a', 97}, {'b', 98}, {'c', 99}, {'d', 100}}
```

+ `emplace_hint`*(C++11)*：使用提示原位构造元素

```C++
template <class... Args>
iterator emplace_hint(const_iterator hint, Args &&...args);
```

+ `try_emplace`*(C++17)*：若键不存在则原位构造元素并插入，若键存在则不做任何事

```C++
template <class... Args>
pair<iterator, bool> try_emplace(const key_type &k, Args &&...args);

template <class... Args>
pair<iterator, bool> try_emplace(key_type &&k, Args &&...args);

template <class... Args>
iterator try_emplace(const_iterator hint, const key_type &k, Args &&...args);

template <class... Args>
iterator try_emplace(const_iterator hint, key_type &&k, Args &&...args);
```

+ `erase`：擦除元素

```C++
iterator erase(iterator pos);
iterator erase(const_iterator pos);
iterator erase(const_iterator first, const_iterator last);
size_type erase(const Key &key);

template <class K>
size_type erase(K &&x);
```

+ `swap`：交换内容
+ `extract`*(C++17)*：从另一容器释出结点

```C++
node_type extract(const_iterator position);
node_type extract(const key_type &k);

template <class K>
node_type extract(K &&x);
```

+ `merge`*(C++17)*：从另一容器接合结点

```C++
template <class C2>
void merge(std::map<Key, T, C2, Allocator> &source);

template <class C2>
void merge(std::map<Key, T, C2, Allocator> &&source);

template <class C2>
void merge(std::multimap<Key, T, C2, Allocator> &source);

template <class C2>
void merge(std::multimap<Key, T, C2, Allocator> &&source);
```

#### 2.3.7 查找

+ `count`：返回匹配特定键的元素数量

```C++
size_type count(const Key &key) const;

template <class K>
size_type count(const K &x) const;
```

+ `find`：寻找带有特定键的元素

```C++
iterator find(const Key &key);

const_iterator find(const Key &key) const;

template <class K>
iterator find(const K &x); // C++14

template <class K>
const_iterator find(const K &x) const; // C++14
```

+ `contains`*(C++20)*：检查容器是否含有带特定键的元素

```C++
bool contains(const Key &key) const;

template <class K>
bool contains(const K &x) const;
```

+ `equal_range`：返回匹配特定键的元素范围

```C++
std::pair<iterator, iterator> equal_range(const Key &key);

std::pair<const_iterator, const_iterator> equal_range(const Key &key) const;

template <class K>
std::pair<iterator, iterator> equal_range(const K &x); // C++14

template <class K>
std::pair<const_iterator, const_iterator> equal_range(const K &x) const; // C++14
```

+ `lower_bound`：返回指向首个不小于给定键的元素的迭代器

```C++
iterator lower_bound(const Key &key);

const_iterator lower_bound(const Key &key) const;

template <class K>
iterator lower_bound(const K &x); // C++14

template <class K>
const_iterator lower_bound(const K &x) const; // C++14
```

+ `upper_bound`：返回指向首个大于给定键的元素的迭代器

```C++
iterator upper_bound(const Key &key);

const_iterator upper_bound(const Key &key) const;

template <class K>
iterator upper_bound(const K &x); // C++14

template <class K>
const_iterator upper_bound(const K &x) const; // C++14
```

----

### 2.4 `set`

大致同`map`，区别如下：

+ 不可以用`at`和`operator[]`进行元素访问
+ 在修改容器方面，同`map`相比，没有`insert_or_assign`和`try_emplace`函数

---

### 2.5  `multimap`

大致同`map`，区别是不可以用`at`和`operator[]`进行元素访问

---

### 2.6 `multiset`

大致同`set`

---

### 2.7 `unordered_map`

#### 2.7.1 类模板声明

```C++
template <
    class Key, class T,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>>
class unordered_map;
```

#### 2.7.2 构造函数

#### 2.7.3 迭代器

+ `begin` `cbegin`
+ `end` `cend`

#### 2.7.4 修改

同`map`

#### 2.7.5 查找

+ `at`
+ `operator[]`
+ `count`
+ `find`
+ `contains`
+ `equal_range`

#### 2.7.6 桶接口

+ `begin(size_type)` `cbegin(size_type)`：返回一个迭代器，指向指定的桶的开始

```C++
local_iterator begin(size_type n);
const_local_iterator begin(size_type n) const;
const_local_iterator cbegin(size_type n) const;
```

+ `end(size_type)` `cend(size_type)`：返回一个迭代器，指向指定的桶的末尾

```C++
local_iterator end(size_type n);
const_local_iterator end(size_type n) const;
const_local_iterator cend(size_type n) const;
```

+ `bucket_count`：返回桶数

```C++
size_type bucket_count() const;
```

+ `max_bucket_count`：返回桶的最大数量

```C++
size_type max_bucket_count() const;
```

+ `bucket_size`：返回在特定的桶中的元素数量

```C++
size_type bucket_size(size_type n) const;
```

+ `bucket`：返回带有特定键的桶

```C++
size_type bucket(const Key &key) const;
```

#### 2.7.7 哈希策略

+ `load_factor`：返回每个桶的平均元素数量

```C++
float load_factor() const;
```

+ `max_load_factor`：管理每个桶的平均元素数量的最大值

```C++
float max_load_factor() const;  // 返回最大加载因子
void max_load_factor(float ml); //  设置最大加载因子为ml
```

+ `rehash`：为至少为指定数量的桶预留存储空间并重新生成散列表

```C++
void rehash(size_type count);
```

+ `reserve`：为至少为指定数量的元素预留存储空间并重新生成哈希表

```C++
void reserve(size_type count); // 设置桶数为适应至少count个元素，而不超出最大加载因子所需的数，并重哈希容器
```

### 2.8 `unordered_set`

大致同`unordered_map`，区别如下：

+ 不可以用`at`和`operator[]`进行元素访问
+ 在修改容器方面，同`unordered_map`相比，没有`insert_or_assign`和`try_emplace`函数

### 2.9 `unordered_multimap`

大致同`unordered_map`，区别是不可使用`at`和`operator[]`进行查找

### 2.10 `unordered_multiset`

大致同`unordered_set`
