---
title: Shell学习笔记 - Bash状态解释
date: 2017-05-30
tags: Shell
---

# 前言

本文介绍Linux下Bash的几种执行状态。

# Bash 状态

有个有意思的Bash环境变量`SHELLOPTS`，还有一个Bash特殊变量`$-`。它们揭示出当前Shell的工作状态。

```
$ echo "$-"
himBH
```

`himBH`是什么意思？它又表示当前shell的什么状态呢？

搜索了*Stack Overflow*上的问答后，子恒喵大概明白了`himBH`是啥意思。

- `h`: 以*Hash*方式缓存环境变量`$PATH`中的可执行文件，用来加速命令执行。如果在已经缓存的情况下移动了可执行文件，shell就不一定找不到命令啦。使用`type`检查命令时，出现的Hashed，就是这个选项启用的。
- `i`: 表示*Interactive*，当前shell是可交互的。
- `m`: 启用*Job control*，Bash的工作控制功能。
- `B`: 启用*Brace expansion*，使得shell可以展开`*`，`?`这些形式的命令。
- `H`: 启用*History substitution*，Bash的历史机制啦，`history`，`!`这些。

在shell script中显示shell工作状态：
```
#!/bin/sh
echo $-
```
```
$ bash ./test.sh
hB
```
只有`hB`，说明在shell script中，**默认是不启用可交互性，历史纪录，工作控制的**。

# `set`命令

子恒喵查阅`man`文档，发现了这样一段话：

> Expands to the current option flags as specified upon invocation, by the set builtin command, or those set by the shell itself (such as the -i option).

在Bash中，`set`命令可以显示出当前shell环境下的所有变量。实际上，`set`命令还可以通过添加参数改变当前shell的工作状态。

```
$ help set
...
set [-abefhkmnptuvxBCHP] [-o option-name] [--] [arg ...]
...
```
具体可以用`help`或者`man`查看，有很多的shell状态flag。

其中，个人用的比较多的是`set -u`。`-u`标志会对shell的变量替换作更加严格的限制，比如取用未定义的变量会报错。

```
$ echo $- && echo ${unset_var}
himBH

```
未定义变量的值为`null`。
```
$ set -u && echo $- && echo ${unset_var}
himuBH
-bash: unset_var: unbound variable
```
在`-u`严格模式下，取用一个未定义的变量，Bash会报错。
这对命令行操作或shell脚本撰写很有帮助。

# 参考资料

- Stack Overflow 问答: https://stackoverflow.com/questions/18146443/what-do-the-characters-in-the-bash-environment-variable-mean

- Job control 详解: https://www.gnu.org/software/bash/manual/html\_node/Job-Control.html#Job-Control

- Brace Expansion 详解: https://www.gnu.org/software/bash/manual/html\_node/Brace-Expansion.html#Brace-Expansion

- History Expansion 详解: https://www.gnu.org/software/bash/manual/html\_node/History-Interaction.html#History-Interaction

