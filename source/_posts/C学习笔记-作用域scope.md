---
title: C学习笔记-作用域scope
date: 2017-06-26
tags: C
---
# 前言

本文讲解C中的作用域。

# 代码块作用域(Block scope)

代码块(Block)是以`{...}`包裹起来的一组语句。

```
#include <stdio.h>
#define PRTP(x) \
    printf("%s: %d: %p\n", #x, (x), &(x))
int a = 4;        /* 全局作用域 'a' */
int main(void){
    int a = 0;    /* main 函数作用域内的 'a' */
    a++;          /* main 函数内的 'a'++ */
    PRTP(a);
    {
        int a = 4;  /* 第2个 'a' */
        a *= 2;     /* 第2个'a'*=2 */
        PRTP(a);
    }
    a = 128;      /* main 函数内的'a'=128 */
    PRTP(a);
    return 0;
}
```
不同代码块作用域内的同名变量，它们的地址是不一样的。即使名字相同，也不会有重复定义的问题。如果在不同代码块内重定义同名变量，代码块内部会**屏蔽**外部变量。

```
#include <stdio.h>
int main(void){
    {
        int n = 64;
    }
    printf("%d\n", n);     /* 报错 */
    return 0;
}
```
在代码块外部无法访问到代码块内部的变量,外部变量对于代码块内不可见。

```
#include <stdio.h>
#define PRTP(x) \
    printf("%s: %d: %p\n", #x, (x), &(x))
int main(void){
    int n = 64;
    {
        n *= 2;
        PRTP(n);    /* OK */
    }
    return 0;
}
```
而在代码块内可以访问代码块外的变量。
代码块作用域内的全部变量都会在函数退出时`右花括号}`消亡，函数参数也属于函数代码块作用域。(要将所有本地变量出栈拿到返回地址)

# 参考资料

- C官方资料: http://en.cppreference.com/w/c/language/scope

