---
title: C++学习笔记-布尔类型
date: 2017-06-27
tags: C++
---

# 前言

本文讲解C++中的布尔类型`bool`。


# 布尔类型Boolean

现代的程序设计语言中，一定少不了`bool`布尔类型之一基本数据类型。
可是啊，C语言是没有`bool`类型的，后来C提供了一个标准库来补充`bool`类型,但是不管怎么说，C原生是没有`bool`类型的。
想要在C中使用`bool`，我们一般会用整型来替代，C判断逻辑真假的规则也很宽泛：**`0`为假，`非0`为真**。

> **Boolean type**
> bool - type, capable of holding one of the two values: true or false. The value of sizeof(bool) is implementation defined and might differ from 1.

在C++中，终于加上了对布尔类型的支持。`bool`, `true`, `false`成为了C++中的关键字。
```
int main(void){
    bool b_t = true;
    bool b_f = false;
    return 0;
}
```
C++继承了C逻辑判断的规则。

# 实现机制

C++与C的逻辑判断有细微的不同。

```
#include <stdio.h>
#define EXPE_T    (1 <= 2)
#define EXPE_F    (1 == 2)
int main(void){
    printf("%d %lu\n", EXPE_T, sizeof(EXPE_T));
    printf("%d %lu\n", EXPE_F, sizeof(EXPE_F));
    return 0;
}
```
C和C++执行上述代码，结果有细微区别:

- `true`为1, `false`为0，这点C/C++相同
- `sizeof`的值不同，C为4，C++为1

我们来观察C++代码生成的汇编代码:
```
int main(void){
    bool b_t = true;
    bool b_f = false;
    return 0;
}
```

```
...
main:
    ...
    movb    $1, -1(%rbp)
    movb    $0, -2(%rbp)
    ...
...
```
原来，**在C++中，`bool`类型是占1个字节的整型，`true`的字面量为1，`false`的字面量为0。**而在C中，逻辑类型占4字节，正好是`int`整型所占的大小。

# 参考资料

- C++官方资料: http://en.cppreference.com/w/cpp/language/bool_literal

