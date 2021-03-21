---
title: C++学习笔记-类与结构体
date: 2017-06-29
tags: C++
---

# 前言

本文讲解C++中的类`class`与结构体`struct`之间的异同。

# 类与结构

C++对C的结构体做了巨大的扩展，加入了成员函数，继承等功能，以适应面向对象(OOP)的特性。

在C++中，类`class`与结构体`struct`基本上是可以等同起来用的，C++的结构体`struct`也可以有成员函数，可以被继承...
```
struct Point{
public:
    Point(void);
    void showPoint(void);
private:
    int x;
    int y;
};
struct PPoint : public Point{
public:
    void showPPoint(void);
private:
    int z;
};
int main(void){
    PPoint p;
    p.showPPoint();
    return 0;
};
```
**在C++中，在大多数场合，把`class`换成`struct`，编译运行结果都还是一样的。**
C++中类与结构体唯一的区别是，类的**默认访问属性**是`private`，而结构体的默认访问属性是`public`。
```
struct S_Point{     /* 默认访问属性为 public */
    S_Point(void);
    void showPoint(void);
    int x;
    int y;
};
class C_Point{      /* 默认访问属性为 private */
    C)Point(void);
    void showPoint(void);
    int x;
    int y;
};
int main(void){
    S_Point sp;
    C_Point cp;
    sp.x = 32;      /* 可行 */
    cp.y = 64;      /* 报错 */
    sp.showPoint(); /* 可行 */
    cp.showPoint(); /* 报错 */
    return 0;
}
```
C++的访问属性限制只是在编译时刻，编译器做的事情。所以不管是用`class`还是`struct`，生成的汇编代码，目标代码文件还是一样的。
另外，在C++模板编程中，是只能用`class`，不能用`struct`。
C++没有去掉`struct`，大概是考虑到了与C的兼容性问题...
子恒喵的建议是：**请将C++中的`struct`当成C中的`struct`使用，如果要用OOP特性，就用`class`。**

# 参考资料

- C++官方资料: http://en.cppreference.com/w/cpp/language/class

