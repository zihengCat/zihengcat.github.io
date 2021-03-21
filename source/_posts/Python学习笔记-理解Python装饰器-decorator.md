---
title: Python 学习笔记 - 理解 Python 装饰器（Decorator）
subtitle: Python Learning Notes - Understanding Python Decorator
date: 2017-11-07
tags: ["Python"]
catalog: true
header-img: img_decorator.png
header-mask: 0.3
---

# 前言

Python 装饰器（Decorator）本质上是一个 Python 函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器的返回值也是一个函数/类对象。装饰器常用于以下场景：插入日志、性能测试、事务处理、缓存、权限校验等等。有了装饰器，我们就可以抽离出大量与函数功能本身无关的代码到装饰器中并继续重用。装饰器的作用是：**为已经存在的对象添加额外的功能**。

# 手写装饰器

我们来实现一个简单的功能函数，计算某函数的执行时间，并打印出来。

```python
import time
def time_taken(f):
    # 接受一枚函数，计算该函数执行时间，并打印结果
    def new_f(*args, **kwargs):
        before = time.time()
        f(*args, **kwargs)
        after = time.time()
        res = after - before
        print("[INFO]: Time taken %d" % (res))
        return res
    return new_f
# 任意函数
def my_func():
    pass
if __name__ == '__main__':
    my_func = time_taken(my_func)
    my_func()
```
> 代码清单：手写 Python 装饰器（Decorator）

我们已经完成了这个计算函数执行时间的功能。我们将计算时间这一模式抽象成一枚函数，这里的`time_taken()`函数就是一个"装饰器（decorator）"，接受一个函数，并在内部对该函数做一些"包装"，返回一个新的函数。

# `@` 语法糖

Python 提供了一种更加简洁的装饰器语法，就是`@`符号。在需要应用装饰器的函数定义处加上`@function`，可以省去`my_func = function(my_func)`赋值语句，直接应用装饰器修饰过的函数。

# `*args` 与 `**kwargs`

如果我们的传入的函数需要参数，可以直接在装饰器中指定。当然，我们可以使用`*args`与`**kwargs`来传入任意数量的参数。

# 带参装饰器

装饰器还可以带有参数，非常灵活。其实现原理是，再用一枚函数对装饰器进行封装。

```python
def time_taken(t):
    def decorator(f):
        # 接受一枚函数，计算该函数执行时间，并打印
        def new_f(*args, **kwargs):
            print("Time now %s" % (t))
            before = time.time()
            f(*args, **kwargs)
            after = time.time()
            res = after - before
            print("Time taken %d" % (res))
            return res
        return new_f
   return decorator
# 应用装饰器
@time_taken(time.time())
def my_func():
    pass
# 等价于 ->
# my_func = time_taken(time.time())(my_func)
if __name__ == "__main__":
    my_func()
```
> 代码清单：带参 Python 装饰器（Decorator）

# 装饰器调用顺序

一枚函数可以被多个装饰器修饰，装饰器的调用顺序是**从里到外**。

```python
@a
@b
@c
def f():
    pass
```
> 代码清单：Python 装饰器（Decorator）嵌套应用

则其调用顺序为：`c() -> b() -> a()`。

```python
f = a( b( c( f ) ) )
```
> 代码清单：Python 装饰器（Decorator）嵌套调用顺序

# 参考资料

- Python Decorator in Detail: http://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html

- 封面图（Cover）: https://zhuanlan.zhihu.com/p/30487077

