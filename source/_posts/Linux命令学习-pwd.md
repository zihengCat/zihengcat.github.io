---
title: Linux命令学习-pwd
date: 2017-06-01
tags: [Linux, Shell]
---

# pwd

`pwd`命令是一个相当常用的命令，它告诉我们shell当前工作目录。
`pwd`是*Print working directory*的意思。
`pwd`是一个shell内置指令(shell builtin)。

```
$ type pwd
pwd is a shell builtin
```

# 详解

```
$ help pwd
pwd: pwd [-LP]
    Print the name of the current working directory.
    
    Options:
      -L	print the value of $PWD if it names the current working
    	directory
      -P	print the physical directory, without any symbolic links
    
    By default, `pwd' behaves as if `-L' were specified.
    
    Exit Status:
    Returns 0 unless an invalid option is given or the current directory
    cannot be read.
```

可以看到，`pwd`命令实际上是打印出了shell环境变量`$PWD`的值。
`-P`选项打印出实际的路径，而非快捷链接（symbolic links）,该选项有时候会有用。

