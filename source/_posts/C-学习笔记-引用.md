---
title: C++学习笔记-引用
date: 2017-06-24
tags: C++
---

# 前言

本文介绍C++中的引用(reference)。

# 引用(reference)

在C++中，可以为一个已存在的对象取一个别名，这被称为引用(reference)。
声明引用的符号是`&`，没错，就是取址运算符。

```
int main(void){
    int n = 8;      /* 定义一个int变量 */
    int& rn = n;    /* 声明int引用rn, 初始化为变量n */
    n = 32;         /* 对 n 进行赋值 */
    rn = 64;        /* 对 rn 进行赋值 */
    return 0;
}
```
上述代码，我们声明了变量n的引用rn。对于rn的赋值操作等同于对n的操作。
我们可以简单的理解为，rn就是n的一个别名(alias)。
引用更多的应用是在函数形式参数。

```
void f(int& n);
int main(void){
    int n = 64;
    f(n);       /* 引用传递 */
    return 0;
}
void f(int& n){
    n *= 2;
}
```
在函数形式参数中使用引用，相当于C中的指针传递，在f函数中对n的操作会直接影响到main函数中的n。
在C中，我们已经习惯了值传递(call by value)避免副作用，如果想对函数外的变量做些修改，我们会使用指针，但是即使是传递指针，也是值传递(传递的值是地址)。
C++引入了引用这一特性。

# 实现机制

但是，C++中的引用到底是什么呢？只是已有对象的一个别名吗？如果是，为什么引用作为函数参数有指针的效果呢？
**实际上，C++的引用就是通过指针的形式实现的。**
我们很难从源代码的层面去验证这一点，无论是对引用的赋值，还是取地址等操作，都是对其所绑定对象的操作。

```
void f_r(int& n);
void f_p(int* n);
int main(void){
    int n = 64;
    int* np = &n;
    int& nr = n;
    *np *= 2;        /* 以指针的方式操作变量 */
    nr *= 2;         /* 以引用的方式操作变量 */ 

    return 0;
}
void f_r(int& n){
    /* do something */
}

void f_p(int* n){
    /* do something */
}
```
我们可以做这样一个实验，分别以指针和引用的方式操作变量。
观察它们的汇编代码:

```
$ g++ cc_ref.cc -S -O0
```

```
main:
.LFB0:
.cfi_startproc
    pushq   %rbp
    .cfi_def_cfa_offset 16
    .cfi_offset 6, -16
    movq    %rsp, %rbp
    .cfi_def_cfa_register 6
    movl    $64, -12(%rbp)      /* 栈分配 12 字节空间 */
    leaq    -12(%rbp), %rax
    movq    %rax, -8(%rbp)      /* 指针赋值操作 */
    movq    -8(%rbp), %rax
    movl    (%rax), %eax
    leal    (%rax,%rax), %edx
    movq    -8(%rbp), %rax
    movl    %edx, (%rax)
    movl    $0, %eax
    popq    %rbp
    .cfi_def_cfa 7, 8
    ret
.cfi_endproc
```
我们发现，指针和引用生成的汇编代码是完全相同的！引用的本质是一个8个字节的指针变量。
同样，在函数传递参数时，指针与引用生成的汇编代码也是完全相同的。
**C++引用是以指针的形式实现的。**
更准确的说，引用是**const指针**，所以在声明引用时需要绑定对象(初始化)，绑定后就不能更改绑定对象了。
C++的引用不如指针来得灵活，但是，引用在C++中有非常重要的应用，了解引用的具体细节是非常有必要的。
据说，BS当年加入引用是为了让代码少些`*`号，看着更简洁一些....

# 参考资料

C++官方资料: http://en.cppreference.com/w/cpp/language/reference

