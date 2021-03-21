---
title: 实时输出流打印 - 缓冲问题与解决方案
subtitle: Output Buffering in Realtime - Question and Solution
catalog: true
date: 2018-07-20
tags: ["Linux", "Python", "Shell"]
---

# 前言

现代程序设计语言（尤其是动态脚本语言）的`I/O`库在设计时，大多会针对输出流「Output Stream」做缓冲「Buffering」机制。其目的是为了最小化「Minimize」`I/O`操作。众所周知，`I/O`操作是非常慢、非常费时的，例如：读写磁盘，访问网络，打印信息到控制台「Console」。

# 缓冲「Buffering」机制

在执行`I/O`操作时，程序会在内存中开辟出一块合适的空间，用作缓存「Buffer」，在缓存区域收到一定数据量或触发事件`flush()`前，不会真正执行磁盘读写`I/O`操作。

# 不同语言下的实时输出流

`I/O`缓冲机制无疑是优秀的设计，但有时候，这一机制也会我们带来一定的麻烦（比如，无法实时查看`Jenkins`持续集成 CI 系统下的控制台信息输出）。我们需要某些方法能做到实时输出流打印。

## `Python`语言

在`Python`中，做到实时输出的方法还是很多的。

使用Python解释器提供的`-u`参数。

```
$ python -u <script.py>
```
> 代码清单：`Python`语言使用`-u`参数

设置环境变量`PYTHONUNBUFFERED`的值为`1`。

```
$ export PYTHONUNBUFFERED=1
```
> 代码清单：设置环境变量`PYTHONUNBUFFERED`

使用`fileObject.flush()`。在 Python3.3 之后，我们可以直接在`print()`函数中指定`flush`参数。在 Python2 中，我们也可以通过`from __future__ import print_function`来使用新版本打印函数。

```python
import sys
sys.stdout.write("...")
# 立即刷新缓冲区
sys.stdout.flush()
```
> 代码清单：使用`flush()`刷新缓冲区

## `Ruby`语言

对于`Ruby`语言，我们可以直接设置`STDOUT.sync`，命令其做实时输出流打印。

```ruby
STDOUT.sync = true
```
> 代码清单：`ruby`实时输出流打印设置

## `Perl`语言

在`perl`脚本语言中，也可以设置实时输出流打印。

```perl
autoflush STDOUT 1;
```
> 代码清单：`perl`实时输出流打印设置

# 参考资料

- Python3 `print()`说明文档：https://docs.python.org/3/library/functions.html#print
- StackOverflow 问答[107705]：https://stackoverflow.com/questions/107705/disable-output-buffering
- StackOverflow 问答[11631951]：https://stackoverflow.com/questions/11631951/jenkins-console-output-not-in-realtime

<!-- EOF -->