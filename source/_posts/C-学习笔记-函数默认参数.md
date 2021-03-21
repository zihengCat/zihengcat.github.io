---
title: C++学习笔记-函数默认参数
date: 2017-06-19
tags: C++
---

# 前言

本文介绍C++中的函数默认参数。

# 函数默认参数(Default arguments)

在C++中，我们可以在**函数原型声明**中为形式参数指定一个默认的值。
例如: 
```
void f(int size, int init = 0); /* 函数默认参数 */
int main(void){
    f(10);      /* 给1个参数 */
    f(10, 10);  /* 给2个参数 */
    return 0;
}
void f(int size, int init){
    /* do something */
}
```
# 实现机制

C++中函数默认参数的实现机制很简单，就是编译器自动帮你补齐参数...
```
$ g++ -S df_func.cc
$ less df_func.s
```
观察汇编代码文件，我们发现: 
- 函数还是那个函数，需要2个参数
- 调用函数时，缺失的参数由默认参数补齐(编译器完成)

```
...
void f(int size, int init = 0);   /* 函数默认参数 */
...
    f(10);      /* 实际上是 f(10, 0) */
    f(10, 10); 
...
```
# 注意事项

1. 函数默认参数是写在**函数原型声明declaration**中的，不能写在**函数定义definition**之中。(编译器是通过函数原型补上默认参数)
2. 函数默认参数是编译器在编译时*compile-time*做的事情，而非运行时*run-time*机制。
3. 函数默认参数必须**从右往左**写，因为编译器是从左往右匹配参数的。
4. 函数默认参数并不安全，因为可以很容易地修改函数原型。
``` 
void f1(int i = 0, int j, int k);    /* 非法 */
void f2(int i, int j = 0, int k);    /* 非法 */
void f3(int i, int j = 0, int k = 0);/* 合法 */
```
函数默认参数与函数重载结合在一起，可能使代码看上去很混乱...
谨慎使用函数默认参数吧。

# 参考资料

- C++官方文档: http://en.cppreference.com/w/cpp/language/default\_arguments
- MSDN文档: https://docs.microsoft.com/en-us/cpp/cpp/default-arguments

