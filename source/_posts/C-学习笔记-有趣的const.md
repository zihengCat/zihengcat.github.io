---
title: C++学习笔记-有趣的const
date: 2017-06-21
tags: C++
---

# 有趣的const

在C中，用`const`关键字修饰变量或者形式参数时，表明该变量不可修改。
然而，C中的`const`更多只是一种"约定"，表明程序不会修改变量值的一种约定。
我们是有办法修改`const`的变量的。
```
#include <stdio.h>
int main(void){
    const int num = 10;     /* const 修饰整型变量 */
    int *p = (int*)(&num);  /* 指针取 const 变量地址*/
    *p = 100;               /* 通过指针修改 const 变量值 */
    printf("%d\n", num);    /* 输出 100 */
    return 0;
}
```
我们可以使用指针间接修改`const`变量，编译器只会给个warning，告诉你：**将const修饰的变量地址交给非const的指针，会丢弃const修饰符。** 如果加上**强制类型转换**，甚至连warning都不会有。
这也表示了，在C中，const修饰符是一种"约定"，编译器在编译时并不保证变量一定不会被改变。毕竟，程序员们使用指针就可以轻松改掉const的值。

然而，在C++中，事情就变得非常有意思啦。
```
#include <stdio.h>
int main(void){
    const int num = 10;
    int *p = (int*)(&num);
    *p = 100;
    printf("%d\n", num);    /* 输出 10 */
    return 0;
}
```
与C不同，C++中，如果不使用强制类型转换，编译器会直接报错，告诉你：**将一个const变量的地址赋给一个非const的指针，这件事情是不允许的。**

更有意思的事情是，当我们使用指针强行修改变量值后，`printf`竟然输出了修改前的值!(使用std::cout也一样的) 这真是让子恒喵怀疑人生...

# const机制窥探

我们可以确定的是，指针p所指向的那片内存空间的值已经是100了，我们确实成功地修改了那片内存空间的值。(debug查看内存)
有趣的是，num的值没有改变...这是怎么回事？难道num不存放在那片栈空间之中？

```
#include <stdio.h>
int main(void){
    const int num = 10;
    int *p = (int*)(&num);
    *p = 100;
    printf("%d\n", *p);     /* 输出 100 */
    printf("%d\n", num);    /* 输出 10 */
    printf("%p %p\n", &num, p);    /* 地址相同 */
    return 0;
}
```
无情的事实告诉我们，都是那片内存空间，但是打印出来的值就是不一样。

```
#include <stdio.h>
int main(void){
    const int num = 10;
    int *p = (int*)(&num);
    *p = 100;
    int t = num;
    printf("%d\n", t);
    return 0;
}
```
这个例子更加明显,同样的源代码,
- 在C中，t的值为100；
- 在C++中，t的值为10。

要搞清楚到底发生了什么，我们需要追踪到它们的汇编代码。
```
$ gcc -O0 -S ./c_const.c
$ g++ -O0 -S ./cc_const.cc
$ diff -c c_const.s cc_const.s
```
分别生成相应的汇编代码文件，通过`diff`比较。
```
...
*** 19,26 ****
    movq    %rax, -8(%rbp)
    movq    -8(%rbp), %rax
    movl    $100, (%rax)
!   movl    -16(%rbp), %eax
!   movl    %eax, -12(%rbp)
    movl    -12(%rbp), %eax
    movl    %eax, %esi
    movl    $.LC0, %edi
--- 19,25 ----
    movq    %rax, -8(%rbp)
    movq    -8(%rbp), %rax
    movl    $100, (%rax)
!   movl    $10, -12(%rbp)
    movl    -12(%rbp), %eax
    movl    %eax, %esi
    movl    $.LC0, %edi
```
注意带`!`的几行。
观察汇编代码文件，子恒喵终于明白了这其中到底发生了什么。
- 在C源码生成的汇编代码中，编译器忠实地翻译了源代码，将目标内存地址(&num)的值赋给了栈上的变量t;
- 而在C++源码生成的汇编代码中，**编译器直接将一个立即数10赋给了栈上的变量t!**

为什么在C++中打印的值还是原来的值？
结论：**编译器在搞鬼。**

这也可以解释，为什么在C++中，一个const的"变量"可以用做数组初始化。
```
...
const int i = 100;
int num[i];     /* 在C++，可行 */
...
```
数组和本地变量相同，都需要在编译时刻就分配好栈空间。但是，数组初始化不一定是需要一个常量，只需要一个在编译时刻确定的值就可以了。C++保证const变量能给出这样一个值。
C++中确定了某种机制，以保证const的值不变。甚至...如果不对const取地址，连变量都不会生成，直接就是一个立即数。
当然，C++也规定了，如果试图修改一个**const对象**的值，结果是未定义的（undefined behavior)，编译器可以有自己的理解。但是至少在上面那个例子中(GCC编译器)，const是真的没有变...
> const\_cast用来丢弃变量的const声明，但不能改变变量所指向的对象的const属性。即：const\_cast用于原本非const的对象；如果用于原本const的对象，结果不可预知（C++语言未对此种情况进行规定）
> -- The C++ Programming Language(Special Edition)
*const* 在C++中的应用十分广泛，搞清楚const的细节还是很有必要的。
C++的编译器会在背后偷偷做很多事情，我们不一定知道。但我们知道，在C++中，const修饰的对象，C++以强有力的机制保证其值不被改变，而不仅仅是一种与程序员之间的"约定"。在C++中，我们可以放心大胆地使用const。

# 参考资料
- C++官方文档: http://en.cppreference.com/w/cpp/language/cv

