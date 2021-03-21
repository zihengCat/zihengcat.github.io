---
title: C学习笔记-枚举类型
date: 2017-06-25
tags: C
---

# 前言

本文讲解C中的`enum`枚举类型。

# 枚举类型 enum

在C中，我们可以使用`enum`枚举类型，枚举类型是一种包含**枚举常量**的特殊的类型。
语法:
```
enum identifier {
    enumerator-list
};
```
样例:
```
enum color {            /* 声明枚举类型 color */
    RED,                /* 有3个枚举常量 */
    GREEN,
    BLUE
};
enum color c = RED;     /* 定义枚举类型 */
switch(c){              /* 枚举类型可以用在switch结构中 */
    case RED: 
        /* do something */
    break;
    case GREEN: 
        /* do something */
    break;
    case BLUE: 
        /* do something */
    break;
}
```
我们可以在声明`enum`枚举类型时，为枚举常量指定一个值。如果不指定，那么默认从0开始，依次递增。

# 实现机制

**`enum`枚举类型本质上是用`int`整型来实现的。**

```
#include <stdio.h>
typedef enum color {        /* 声明枚举类型 color */
    RED, GREEN, BLUE,       /* 有3个枚举常量 */
} Color;
int main(void){
    Color c = RED;
    c = BLUE;
    int n = c;              /* 直接对 int 类型赋值 */
    printf("%d %d %lu\n", n, c, sizeof(c));
    return 0;
}
```
```
$ gcc ./enum.c && ./a.out
2 0 4
```
可以看到，`enum`枚举类型占用的字节永远都是4，这正好是一个`int`所占用的字节。
我们也可以直接使用枚举常量对`int`整型赋值。

观察生成的汇编代码:
```
...
    movl    $0, -4(%rbp)
    ...
    movl    $2, -8(%rbp)
...
```
我们可以清楚的看到，`enum`枚举类型与`int`整型生成的汇编代码是一样的。

```
#define RED     0
#define GREEN   1
#define BLUE    2
```
我们完全可以使用`#define`常量实现与`enum`枚举类型同样的效果。
C加入`enum`枚举类型的初衷就是希望用有意义的名字来代替幻数(magic number)，从语言的层面提供了一种使用有名常量的方式。
在任何使用`enum`枚举类型的场合，我们都可以替代性地使用`#define`常量。
子恒喵是更喜欢用`#define`常量啦...

# 参考资料

- C官方文档: http://en.cppreference.com/w/c/language/enum

