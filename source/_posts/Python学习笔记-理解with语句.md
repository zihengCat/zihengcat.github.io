---
title: Python学习笔记 - 理解with语句
date: 2018-05-23
tags: Python
---

# 前言

Python的`with`语句是从 Python 2.5 开始引入的一种与**异常处理**相关的功能。`with`语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的**清理**操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等。

# 基本语法

`with`语句的基本语法格式如下所示，`with`后接上下文表达式（Context Expression），如果指定了`as`子句的话，Python会将上下文管理器的`__enter__()`方法的返回值赋给`target(s)`。`target(s)`可以是单个变量，也可以是`()`元组（不能是仅仅由`,`分隔的变量列表，必须加`()`）。

```python
# 语句头
with context_expression [as target(s)]:
    # 语句体
    with-body
```
> 代码清单：`with`语句语法格式

Python 中很多内建对象与标准库都支持`with`语句，比如可以自动关闭文件、线程锁的自动获取和释放等。以文件操作为例，使用`with`语句可以写如下代码：

```python
with open(r'filename') as f:
    # do with file f
    # ...
```
> 代码清单：使用`with`语句操作文件对象

这里使用了`with`语句，不管在处理文件过程中是否发生异常，都能保证`with`语句执行完毕后关闭打开的文件句柄。使用传统`try...except...finally`异常处理结构，则可以写出如下代码：
```python
f = open(r'filename')
try:
    # do with file f
    # ...
finally:
    f.close()
```
> 代码清单：传统方式操作文件对象

比较起来，使用`with`语句可以减少代码量，还可以使我们在操作资源对象时不会因忘记释放资源而出现问题（`with`语句在结束时会执行`__exit__()`方法自动回收资源）。

# 工作原理

要理解`with`语句的工作原理，我们可以参考下列代码：

```python
context_manager = context_expression
# 取得但不调用退出方法
exit = type(context_manager).__exit__
value = type(context_manager).__enter__(context_manager)
# True 表示正常执行，即便有异常也忽略
# False 表示重新抛出异常，需要对异常进行处理
exc = True
try:
    try:
        target = value  # 如果使用了 as 子句
        # with-body     # 执行 with-body
    except:
        # 执行过程中有异常发生
        exc = False
        # 如果 __exit__ 返回 True，则异常被忽略
		# 如果返回 False，则重新抛出异常
        # 由外层代码对异常进行处理
        if not exit(context_manager, *sys.exc_info()):
            raise
finally:
    # 正常退出，或者通过 statement-body 中的 break/continue/return 语句退出
    # 或者忽略异常退出
    if exc:
		# None 在上下文中被看做是 False
		# 默认返回 None
        exit(context_manager, None, None, None)
```
> 代码清单：`with`语句执行过程

具体执行步骤为：

1. 执行`context_expression`，生成上下文管理器`context_manager`

2. 调用上下文管理器的`__enter__()`方法；如果使用了`as`子句，则将`__enter__()`方法的返回结果赋值给`as`子句中的`target(s)`

3. 执行`with`语句体`with-body`

4. 不管是否执行过程中是否发生了异常，都会执行上下文管理器的`__exit__()`方法，该方法负责执行"清理"动作（如释放资源等）。

5. 出现异常时，将异常传递给`__exit__()`方法进行处理，如果返回`False`（异常处理失败），则会重新抛出异常，让`with`之外的语句逻辑来处理异常。如果返回`True`（异常处理成功），则忽略异常，不再对异常进行处理。


# 参考资料

- PEP343: https://www.python.org/dev/peps/pep-0343/

- IBMDevelopworks: https://www.ibm.com/developerworks/cn/opensource/os-cn-pythonwith/index.html


