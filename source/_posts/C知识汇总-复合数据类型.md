---
title: C知识汇总-复合数据类型
date: 2017-07-14
tags: C
---

# 数组

数组（Array）是一种复合数据类型，它由一系列相同类型的元素（Element）组成。

## 定义数组

使用`[...]`可以定义一个数组：
```
<type> array_name [<constant expression>];
```
数组元素的存储空间是相邻的。

- ANSI C 数组的元素个数必须在编译时刻确定；
- C99 增添了变长数组(VLA)这一新特性。

## 访问数组元素

数组中的元素通过下标（或叫索引，Index）来访问。
```
array_name[index] = value;
```

**数组下标从“0”开始。**大多数编程语言都是这么规定的，所以计算机术语中有 *Zeroth* 这个词。

使用数组下标不能超出数组的长度范围，这一点在使用变量做数组下标时尤其要注意。**C编译器并不检查数组访问越界的错误**，编译时能顺利通过，运行时会出错。

## 数组初始化和赋值

数组定义时使用`{...}`初始化。
```
type array_name [<constant expression>] = {
    type_value,
    ...
    type_value,
};
```
`{...}`依次初始化数组中的每个变量，未被赋值的变量用(`0`)填充。

可以使用下标为数组元素赋值。**两个数组之间不可直接`=`赋值。**

## 多维数组

数组也可以嵌套，一个数组的元素可以是另外一个数组，这样就构成了多维数组（Multi-dimensional Array）。

```
type array_name[size1][size2]...[sizeN];
```
多维数组最简单的形式是二维数组。一个二维数组，在本质上，是一个一维数组的列表。声明一个 x 行 y 列的二维整型数组，形式如下：
```
type array_name[<x>][<y>];
```
从逻辑模型上看，这个二维数组是x行y列的表，元素的两个下标分别是行号和列号。**从物理模型上看，数组元素在内存中仍然是连续存储的。**

## 字符串

C的字符串可以看作**以`\0`空字符结尾的字符数组**，它的每个元素都是字符(char)型。
每个字符串末尾都有一个字符`\0`结束符，这里的`\0`是ASCII码为0的Null字符，在C语言中这种字符串也称为以零结尾的字符串（Null-terminated String）。数组元素可以通过数组名加下标的方式访问，而字符串也可以像数组一样使用，可以加下标访问其中的字符。

# 结构体

结构体是C语言提供的一种自定义复合数据类型的方式。

## 定义结构体

为了定义复合结构，需要使用 struct 语句。struct 语句定义了一个包含多个成员的新的数据类型，struct 语句的格式如下：

```
struct [structure tag]
{
   member definition;
   member definition;
   ...
   member definition;
} [one or more structure variables];
```

> 注意结构体声明后的`;`不能少。结构体类型定义是一种声明，要以`;`号结尾。

可以在定义了 struct 后直接用普通的声明语句来声明变量：
```
struct structure_tag variable1, variable2;
```

## 访问结构体成员

可以用`.`运算符（`.`号，Period）来访问结构体成员，结构体成员之间的存储空间是相邻的。

## 结构初始化与赋值

结构体变量也可以在定义时初始化：
```
struct structure_tag variable = { member_value, ... member_value };
```
initializer初始化过程中的数据依次赋给结构体的各成员。如果initializer中的数据比结构体的成员多，编译器会报错，但如果只是末尾多个逗号则不算错。如果initializer中的数据比结构体的成员少，**未指定的成员将用0来初始化**，就像未初始化的全局变量一样。

`{...}`只能用于结构体初始化，不能用于结构体赋值。

- ANSI C只允许在`{...}`中使用**常量表达式**来初始化结构体变量，无论是全局变量还是局部变量。

- 局部变量的结构体可以使用另一个变量的值来初始化它的成员，如果是全局变量，就只能用**常量表达式**来初始化。这是C99的新特性。

- 我们可以使用`.`运算符为结构体变量中的成员一一赋值。
- 也可以使用一个相同类型的结构体变量为结构体赋值，此时发生的是**内存拷贝**。

## 嵌套结构体

结构体也是一种递归定义：结构体的成员具有某种数据类型，而结构体本身也是一种数据类型。换句话说，结构体的成员可以是另一个结构体，即结构体可以嵌套定义。
```
struct [structure tag]
{
   member definition;

   struct [structure tag]{
       member definition;
       ...
       member definition;
   } structure variables;

   ...

   member definition;
} [one or more structure variables];

```
嵌套结构体可以嵌套初始化，例如：
```
struct structure_tag variable = { 
    { member_value, ... member_value },
    { member_value, ... member_value },
    { member_value, ... member_value },
};
```
嵌套结构体也可以平坦(*Flat*)初始化，因为结构体成员的内存布局是紧挨着排列的：
```
struct structure_tag variable = { 
    member_value, ... member_value,
    member_value, ... member_value,
    member_value, ... member_value,
}
```
访问嵌套结构体成员要用到多个`.`运算符：
```
struct1.struct2.member = member_value;
```

# 共用体

共用体是一种特殊的数据类型，允许我们在相同的内存位置存储不同的数据类型。可以定义一个带有多成员的共用体，但是同一时刻只能有一个成员带有值。共用体提供了一种使用相同的内存位置的有效方式。

## 定义共用体

定义共用体使用 union 语句，方式与定义结构类似。union 语句定义了一个新的数据类型，带有多个成员。union 语句的格式如下：
```
union [union tag]
{
   member definition;
   member definition;
   ...
   member definition;
} [one or more union variables];
```
## 访问共用体成员

可以用`.`运算符（`.`号，Period）来访问共用体成员。

## 共用体与结构体的区别

- 结构体变量所占内存空间是结构体各成员空间之和（+内存对齐），各成员有其独立的内存空间。

- 共用体变量所占内存空间是**共用体最大成员所占内容空间**，各成员共用同一块内存空间。

所以，同一时刻，共用体只有一个有效数据成员。
使用共用体的目的是为了节约内存。

