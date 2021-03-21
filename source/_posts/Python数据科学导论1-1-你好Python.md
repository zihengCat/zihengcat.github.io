---
title: Python数据科学导论1.1 - 你好, Python
date: 2018-04-17
tags: Python
---

# 前言

本文是对**[DAT208x] Python数据科学导论1.1 - 你好, Python**做的内容梳理。

# 课程视频（bilibili源）

[av22252199 - P2](https://www.bilibili.com/video/av22252199/?p=2)

# 课程小结

**关于Python**

Python是一门非常流行的高级编程语言，最初由 Guido van Rossum 开发，目前由Python开源社区维护。
Python是一门通用型编程语言，你可以在不同领域使用Python编程。
由于Python开源性质，在各个领域都有大量好用的第三方库可供调用。
Python目前有两个大版本（2和3），由于历史原因，Python2将会被逐渐淘汰，Python3会成为主流，本课程使用Python3教学。

**Python脚本与Python解释器**

与编译型语言有所不同，Python是一门动态语言。这意味着我们可以直接在Python解释器中输入代码语句执行；也可以将代码语句写入到一个后缀名为`.py`的文本文件中，再将该文本文件输入到Python解释器解释执行。

# 知识点考察

Python脚本文件的后缀名是什么？

A. `.script`
B. `.pyscript`
C. `.py`
D. `.python`

> 解: C

如果要计算出`3 + 4`的结果并打印出来，Python代码该怎么写？

A. `print 3 + 4`
B. `print(3 + 4)`
C. `put 3 + 4`
D. `put(3 + 4)`

> 解: B

# 编程练习

本节的编程练习，你将亲手撰写你自己的Python代码。你将学会如何与Python解释器打交道，以及使用Python来做些运算。

## Hello, world!

学习一门新的编程语言，第一个程序总是会写`Hello, world!`，这就像一个仪式般的开始。

```
# 让 Python 说出 Hello, world!
print("Hello, world!")
print("你好，世界！")
```
> 代码清单: Hello, world!

## Python作计算器

接下来，我们使用Python做些运算（加减乘除四则运算）。

```
# 用Python作加减乘除四则运算
print(2 + 2)
print(5 - 3)
print(2 * 3)
print(4 / 8)
```
> 代码清单: 将Python用作计算器

## Python程序注释

让我们了解一下**程序注释**的概念。注释是程序非常重要的一部分，注释使得你和他人都可以充分理解代码的意义，注释是给人看而不是给机器看的。

想在Python中加入注释，我们可以使用`#`符号。从**`#`开始到该行行末**的所有内容都会被Python解释器忽略，不会被真正执行。

```
# 这是Python注释
print("这是一条Python语句")
```

> 代码清单: 使用Python注释

## Python编程求解实际问题

最后，我们尝试使用Python编程解决一个实际问题。

假设你将$100$元用作投资，年收益$10\%$，则$7$年后本金加收益共计多少？
第$1$年: $100 \times (1 + 0.1) = 110$
第$2$年: $100 \times (1 + 0.1) \times (1 + 0.1) = 121$
...
第$7$年: $?$

```
# 幂次(4的2次方)
print(4 ** 2)

# 取余(18对7取余数)
print(18 % 7)

# 编程解决实际问题
print(100 * (1 + 0.1) ** 7)
```

> 代码清单: 使用Python解决实际问题

# 拓展阅读

- 关于Python: https://zh.wikipedia.org/wiki/Python

- Windows下安装Python: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014316090478912dab2a3a9e8f4ed49d28854b292f85bb000

- Linux下从源码编译安装Python: https://zihengcat.github.io/2017/07/06/Linux%E4%B8%8B%E6%90%AD%E5%BB%BAPython3%E7%8E%AF%E5%A2%83/

