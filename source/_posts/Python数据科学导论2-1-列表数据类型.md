---
title: Python数据科学导论2.1 - 列表数据类型
date: 2018-04-20
tags: Python
---

# 前言

本文是对**[DAT208x]Python数据科学导论2.1 - 列表数据类型**做的内容梳理。

# 课程视频（Bilibili源）

[av22252199 - P4](https://www.bilibili.com/video/av22252199/?p=4)

# 课程小结

**Python列表**

有时候我们使用一枚枚变量来存储数据不太方便，我们可以Python List列表数据类型。列表是一种复合数据类型，表示一系列数据的集合。列表是有序的，我们可以通过下标/索引方便地取得列表中存储的数据。

列表可以存储任何类型的数据，比如：列表中再嵌套子列表。

**建立Python列表**

我们可以使用一对方括号`[ ]`来建立Python列表。列表中的数据以逗号`,`分隔。

# 知识点考察

问：如果`fam`是一枚Python列表变量，则`type(fam)`的结果为？

A => `pylist`
B => `struct`
C => `integer`
D => `list`

> 解: D

问：以下构造Python列表语句中，不合法的是？

A => `x = ["this", "is", "a" True "list"]`
B => `x = ["this", "is", "a" True, "list"]`
C => `x = ["this", "is",  1, True, "list"]`
D => `x = ["this" + "is",  1, True, "list"]`

> 解: A

问：以下Python列表中存储的数据类型有？（多选）

```
x = ["you", 2, "are", "so", True]
```

A => `int`
B => `float`
C => `str`
D => `bool`
E => `list`

> 解: A, C, D

# 编程练习

本节的编程练习，你将学习使用不同的方式创建Python列表，并探究Python列表的一些有趣特性。

## 创建列表

不同于`int`，`bool`这样的基础数据类型，列表是一种复合数据类型。你可以将各种不同数据类型都装进一个列表里。

我们来创建一个列表，按顺序将一系列变量都装入，再打印出列表看一看。

```
# 一些表示面积的变量（单位：平方米）
hall = 11.25
kit = 18.0
liv = 20.0
bed = 10.75
bath = 9.50

# 创建列表`areas`
areas = [hall, kit, liv, bed, bath, ]

# 打印`areas`
print(areas)
```
> 代码清单: 创建列表练习

## 包含不同数据类型的列表

Python列表可以装入任何类型的数据，虽然这不太常用。在上一节编程练习中，我们将房间的面积装入了列表，现在我们将房间的名称也装进去。

```
# 一些表示面积的变量（单位：平方米）
hall = 11.25
kit = 18.0
liv = 20.0
bed = 10.75
bath = 9.50

# 调整列表`areas`
areas = ["hallway", hall,
         "kitchen", kit,
         "living room", liv,
         "bedroom", bed,
         "bathroom", bath]

# 打印`areas`
print(areas)
```
> 代码清单: 包含不同数据类型的列表练习

## 列表嵌套

Python列表装入任何数据类型，当然也包括列表，列表是可以嵌套使用的。我们对上一节编程练习中的代码使用列表嵌套进一步改进。

```
# 一些表示面积的变量（单位：平方米）
hall = 11.25
kit = 18.0
liv = 20.0
bed = 10.75
bath = 9.50

# 嵌套列表`areas`
areas = [[ "hallway", hall, ],
         [ "kitchen", kit, ],
         [ "living room", liv, ],
         [ "bedroom", bed, ],
         [ "bathroom", bath,]]

# 打印`areas`
print(areas)

# 打印`areas`的类型
print(type(areas))
```
> 代码清单: 列表嵌套练习

# 拓展阅读

- Python列表与元组: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014316724772904521142196b74a3f8abf93d8e97c6ee6000

