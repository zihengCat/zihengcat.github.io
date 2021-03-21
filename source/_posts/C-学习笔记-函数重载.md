---
title: C++学习笔记-函数重载
date: 2017-06-18
tags: C++
---

# 前言

本文讲述C++中的函数重载机制。

# 函数重载(Function Overloading)

在C++中，如果函数的函数名相同，返回类型相同，参数表不同，那么这些函数就构成重载关系，这被称为函数重载(Function Overloading)。

例如:

```
...
void func(int);
void func(double);
...
func(3)     /* 调用void func(int)    函数 */
func(3.14)  /* 调用void func(double) 函数 */
...
```
相同函数名，根据不同参数列表，选择调用不同的函数。

# 实现机制

如果函数的名称相同，参数表相同，返回类型不同，那么函数不构成重载关系...而且会报二义性错误。

这是因为，编译器在处理时必须明确的区分到底调用了哪个函数，仅根据函数返回类型是无法做到这一点的。

```
...
void func(void);
int  func(void);
...
func();     /* 编译器无法区分到底调用了哪个函数 */
func();
...

```
我们知道，在C中，是没有函数重载这一机制的。如果想实现类似的效果，我们只能这么做:
```
...
void func_i(int);
void func_d(double);
...
func_i(3)     /* 调用void func_i(int)    函数 */
func_d(3.14)  /* 调用void func_d(double) 函数 */
...
```
没错，只能根据不同的参数表，取不同的函数名称...

实际上，C++的编译器就是这么干的!
C++的编译器会根据函数的参数表，改动函数名。在C中，我们需要手动完成，C++中，编译器帮我们自动完成了这件事。

```
$ g++ -S functionOverloading.cc
$ cat functionOverloading.s
...
_Z4funci:
    ...
_Z4funcd:
    ...
_main:
    ...
    call _Z4funci
    ...
    call _Z4funcd
    ...
...
```
可以看到，在生成的汇编代码中，重载的`func函数`已经根据参数表，改变了名字；而我们对重载函数的调用也根据提供参数的不同而对应到相应的函数上去啦。

函数重载在类，模版，类型转换中有更加复杂的应用。但是，其机制是简单明了的，根据参数表调用不同的函数。

# 参考资料

- C++官方教程: http://en.cppreference.com/book/intro/function\_overloading
- C++官方文档: http://en.cppreference.com/w/cpp/language/overload\_resolution

