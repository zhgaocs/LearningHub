# C/C++关键字—`volatile`

## 1. 作用

`volatile`是一种类型修饰符，它提醒编译器修饰的变量可能会随时更改，强制每次直接从内存读取，以避免不一致。如果省略`volatile`，编译器可能会优化读写操作，导致潜在不一致性问题，因为它可能使用暂存的寄存器值

## 2. 注意

+ 欲使用`volatile`语义访问非`volatile`对象，必须先将其地址转换成指向`volatile`类型的指针，再通过该指针访问该对象

```C
volatile int n = 1;
int *p = &n;           // warning
volatile int *vp = &n; // no warning
```

+ 指向非`volatile`类型的指针可以隐式转换成指向同一或兼容类型的`volatile`限定版本的指针。逆向转换可以由类型转换表达式进行

```C
int *p = 0;
volatile int *vp = p; // no warning
p = vp;               // warning
p = (int *)vp;        // no warning
```


+ `volatile `限定的结构体或联合体类型，其成员会获取其所属类型的限定（当通过` .` 或 `-> `运算符时）：

```C
struct s
{
    int i;
    const int ci;
} s;                  // s.i类型是int，s.ci的类型是const int
volatile struct s vs; // vs.i和vs.ci的类型各是volatile int和const volatile int
```

+ 数组类型
    + C23前：以`volatile`类型限定符声明数组类型（通过使用`typedef`），则数组类型无`volatile`限定，但其元素类型有
    + C23起：数组类型与其元素类型同等地拥有`volatile`限定

```C
typedef int A[2][3];
volatile A a = {{4, 5, 6}, {7, 8, 9}}; // 元素类型为volatile int的二维数组
int *pi = a[0];                        // warning
int (*p)[2][3] = &a;                   // warning
```

## 3. `const`与`volatile`

### 3.1 含义

- `const` 表示“**请将其视为常量使用**”，而不是“这肯定是个常量”
- `volatile` 表示“**请不要进行自以为是的优化，这个值可能会改变**”，而不是“你可以修改这个值”

### 3.2 作用及阶段

- `const` 仅在编译期间有效，在源代码中保证该变量不会被修改。但在运行时，该变量的值是否被改变并不受 `const` 限制
- `volatile` 在编译期和运行期均有效。在编译期，它告诉编译器不要进行优化，变量的值可能随时改变。在运行期，每次使用该变量的值时，都会直接从内存中取得最新值

### 3.3 搭配使用

```C
#define REGISTER_ADDRESS 0x40025000 // 假设有一个硬件寄存器的地址是0x40025000

const volatile uint32_t *register_ptr = (uint32_t *)REGISTER_ADDRESS; // 使用const和volatile来定义一个指向该寄存器的指针
```

## 4. 用法

+ `static volatile`模拟内存映射的I/O端口，`static const volatile`模拟只读的输入端口

```C
#include <stdint.h>

static volatile uint8_t io_port; // 模拟内存映射的I/O端口

static const volatile uint8_t input_port; // 模拟只读的输入端口

void write_to_io_port(uint8_t value)
{
    io_port = value; // 写入I/O端口
}

uint8_t read_from_input_port()
{
    return input_port; // 从输入端口读取
}
```

+ `volatile`保证`longjmp`后恢复局部变量的值

```C
// test.c
#include <stdio.h>
#include <setjmp.h>

jmp_buf env;

static void func()
{
    longjmp(env, 1);
}

int main()
{
    register int r = 0;
    volatile int v = 0;

    if (setjmp(env))
        printf("After longjmp: r = %d, v = %d\n", r, v);
    else
    {
        r = v = 1;
        printf("Before longjmp: r = %d, v = %d\n", r, v);
        func();
    }

    return 0;
}
```

```BASH
$ gcc -O3 test.c
$ ./a.out
Before longjmp: r = 1, v = 1
After longjmp: r = 0, v = 1
```

`setjmp`函数负责保存程序的执行环境，包括寄存器中的值。`volatile`关键字则保证变量的值始终在内存中进行读写，而不是在寄存器中。这意味着，在使用`longjmp`进行非局部跳转后，`volatile`变量的值将恢复为调用`longjmp`之前的最新值，而不是在调用`setjmp`时的值

正如上述程序运行结果所示，`register`变量恢复了调用`setjmp`时的值，而`volatile`变量则恢复了调用`longjmp`前的最新值

