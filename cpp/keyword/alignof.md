# C++关键字—`alignof`

## 1. 标准

C++11

## 2. 作用

查询类型的对齐要求

## 3. 语法

```C++
alignof(type-id);
```

返回值类型为`std::size_t`

## 4. 对齐要求

- **基础类型**：对齐要求等于类型的存储大小
- **类类型**：对齐要求是其成员变量中最大的对齐要求

## 5. 注意

- `type-id`为引用类型，`alignof`返回的是**被引用**类型的对齐要求
- `type-id`为数组类型，`alignof`返回的是**数组元素**类型的对齐要求

## 6. 示例

```C++
struct S
{
    bool b;
    char c;
    double d;
    float f;
    int i;
};

int i, &ri = i;
S arr[2];

alignof(ri);  // 4
alignof(S);   // 8
alignof(arr); // 8
sizeof(S);    // 24
sizeof(arr);  // 48
```

