---
title: Python数据科学导论3.1 - 函数
date: 2018-04-23
tags: Python
---

# 前言

本文是对**【DAT208x】Python数据科学导论3.1 - 函数**做的内容梳理。

# 课程视频（Bilibili源）

[av22252199 - P7](https://www.bilibili.com/video/av22252199/?p=7)

# 课程小结

**Python函数**

函数是一段可重用的代码，我们可以使用函数来帮助我们解决特定的问题。比如，使用`print()`函数做输出，使用`type()`函数判断变量数据类型。

```python
fam = [1.73, 1.68, 1.71, 1.89]
# 调用最大值函数找到最大值
tallest = max(fam)
# 调用函数做四舍五入
round(tallest)
```

**Python函数帮助**

我们可以在Python交互式环境下使用`help()`函数查阅某个类/函数的帮助文档。这对我们认识、使用一个新函数很有帮助。

# 知识点考察

问：要找出列表`x`中的最小值，使用最小值函数`min()`，代码该怎么写？

```python
x = [1.73, 1.68, 1.71, 1.89]
```

A => `min x`
B => `min[x]`
C => `min(x)`
D => `min = c`

> 解: C

问：使用`round()`函数对`x`做四舍五入，保留2位小数，如何调用？

```python
x = 3.141519
```

A => `round(x)`
B => `round(x, 2)`
C => `round(x, "2")`
D => `round(2, x)`

> 解: B

# 编程练习

本节的编程练习，你将学习调用各类函数。

## 函数调用

Python提供一系列开箱即用的内置函数，使用函数可以使你的编程更加简单。我们已经熟悉`print()`，`type()`以及类型转换函数，这些都是内置函数。

```python
# 创建变量
var1 = [1, 2, 3, 4]
var2 = True

# 调用`type()`函数判断变量类型
print(type(var1))

# 调用`len()`函数获取可迭代对象长度
print(len(var1))

# 调用`int()`函数做类型转换
out2 = int(var2)
```
> 代码清单: 函数调用练习

## 多参数函数

有的函数可以接受不止一枚参数。例如`sorted()`函数。通过`help()`查阅该函数的帮助文档，我们会发现该函数接受三个参数：`iterable`，`key`，`reverse`。其中两个参数具备默认参数。**函数默认参数：如果函数调用者不指定参数值，则该参数取得默认值。**

```python
# 创建变量变量
first = [11.25, 18.0, 20.0]
second = [10.75, 9.50]

# 连接列表
full = first + second

# 按降序重排序列表
full_sorted = sorted(full, reverse = True)

# 打印查看结果
print(full_sorted)
```
> 代码清单: `sorted()`函数调用练习

# 拓展阅读

- 调用函数: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014316784721058975e02b46cc45cb836bb0827607738d000

