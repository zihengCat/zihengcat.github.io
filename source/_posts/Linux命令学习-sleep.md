---
title: Linux命令学习-sleep
date: 2017-06-15 
tags: Linux
---

# sleep

`sleep`命令的作用非常简单，就是使系统**暂停**一段时间。
`sleep`是*GNU coreutils*工具之一。

# 详解

```
$ sleep --help
Usage: sleep NUMBER[SUFFIX]...
  or:  sleep OPTION
Pause for NUMBER seconds.  SUFFIX may be 's' for seconds (the default),
'm' for minutes, 'h' for hours or 'd' for days.  Unlike most implementations
that require NUMBER be an integer, here NUMBER may be an arbitrary floating
point number.  Given two or more arguments, pause for the amount of time
specified by the sum of their values.

      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'sleep invocation'
```
命令没有特殊的选项，就是*help*与*version*两个常规的选项啦。

```
sleep number[smhd]…
```
用法是，sleep后接一个数字(可以是整数，也可以是浮点数)，再接一个后缀：

- s: 秒
- m: 分
- h: 时
- d: 日

默认是秒。
另外，接上多个数字的话，会计算时间的总和，比如: `sleep 10 20` 暂停30秒。

字恒喵会在写脚本的时候使用`sleep`命令。

