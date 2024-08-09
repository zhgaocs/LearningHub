# C语言关键字—`restrict`

## 1. 编译标准

`restrict`关键字**自C99引入**，编译时可能需要加上编译标准参数

```bash
gcc -std=c99 ...
```

## 2. 作用

`restrict`广泛用于函数参数上，用于告知编译器该指针是唯一引用某内存的方式，以进行更有效的优化

## 3. 示例

```C
// 1.c
int foo(int *a, int *b)
{
    *a = 5;
    *b = 6;
    return *a + *b;
}
```
```C
// 2.c
int rfoo(int *restrict a, int *restrict b)
{
    *a = 5;
    *b = 6;
    return *a + *b;
}
```

```bash
$ gcc -std=c99 -O3 -c 1.c 2.c
$ objdump -S 1.o 2.o

1.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <foo>:
   0:   c7 07 05 00 00 00       movl   $0x5,(%rdi)
   6:   c7 06 06 00 00 00       movl   $0x6,(%rsi)
   c:   8b 07                   mov    (%rdi),%eax
   e:   83 c0 06                add    $0x6,%eax
  11:   c3                      retq

2.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <rfoo>:
   0:   c7 07 05 00 00 00       movl   $0x5,(%rdi)
   6:   c7 06 06 00 00 00       movl   $0x6,(%rsi)
   c:   b8 0b 00 00 00          mov    $0xb,%eax
  11:   c3                      retq
```

## 4. 注意

虽然**C++标准中没有`restrict`关键字**，但是许多C++编译器支持类似的功能。例如，GCC和Clang支持`__restrict__`关键字，而Microsoft的Visual C++编译器支持`__restrict`关键字，这些关键字的语义与C语言中的`restrict`相似


