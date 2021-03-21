---
title: Python学习笔记 - Hello, Python
date: 2017-08-31
tags: Python
---

# 前言

[Linux下安装Python学习环境。](https://zihengcat.github.io/2017/07/06/Linux%E4%B8%8B%E6%90%AD%E5%BB%BAPython3%E7%8E%AF%E5%A2%83/)

# Hello World!

我们来看看Python程序语言的Hello World。

## 交互式执行

```
$ python3
Python 3.6.2 (default, Jul 19 2017, 02:14:24)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("Hello, World")
Hello, World
>>> exit()
```
## 脚本式执行

打开文本编辑器，键入以下代码。
```
#!/usr/bin/python3
print("Hello World!")
```
使用Python解释器解释执行。
```
$ python3 hello.py
```
或
```
$ chmod 755 hello.py
$ ./hello.py
```

# 逻辑行与物理行

代码文本中的行即为**物理行** 。Python将一条语句当作一个**逻辑行** 。
Python假设一个物理行对应一个逻辑行，可以使用`\`接续逻辑行。
Python鼓励每条语句占一行 ，这样可读性更强。
如果你希望在一个物理行包含多个逻辑行，则必须使用分号`;` 显式说明一个逻辑行(一条语句)的结束。
```
#!/usr/bin/python3
s = 'Hello, World!'; print\
(s)
```

# 注释

Python中的注释有**单行注释**和**多行注释**。注释的内容会被解释器忽略。

## 单行注释

以`#`开头，直到该行行尾(换行符)。

```
#!/usr/bin/python3
print("Hello World!")   # 打印 Hello World
```

## 多行注释

使用成对三单引号`'''`或三双引号`"""`组成的跨行字符串。

```
#!/usr/bin/python3
'''
这是多行注释，使用三单引号
这是多行注释，使用三单引号
这是多行注释，使用三单引号
'''
print("Hello World!")
"""
这是多行注释，使用三双引号
这是多行注释，使用三双引号
这是多行注释，使用三双引号
"""
```

# 缩进

**Python逻辑行开头空白字符十分重要**。被称作缩进。
逻辑行开头的空白字符(空格和制表符)用于确定逻辑行的缩进级别，依此按顺序将语句分组。同组的语句的缩进级别必须相同，语句组也被称作语句块。
**错误的缩进会导致程序出错。**

> 不要混合使用制表符和空格来缩进，Python建议每个缩进层次使用四个空格或单个制表符。

