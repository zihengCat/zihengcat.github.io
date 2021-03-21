---
title: Linux命令学习-cd
date: 2017-06-03 
tags: [Linux, Shell]
---

# cd

`cd`命令可以说是广大Linux用户最常使用的命令啦，它的作用是改变shell当前工作目录。
`cd`是*Change directory*的意思。
`cd`是一个shell内置指令(shell builtin)。

```
$ type cd
cd is a shell builtin
```

# 详解

```
$ help cd
cd: cd [-L|[-P [-e]]] [dir]
    Change the shell working directory.
    
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
    
    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.
    
    If the directory is not found, and the shell option `cdable_vars' is set,
    the word is assumed to be  a variable name.  If that variable has a value,
    its value is used for DIR.
    
    Options:
        -L	force symbolic links to be followed
        -P	use the physical directory structure without following symbolic
    	links
        -e	if the -P option is supplied, and the current working directory
    	cannot be determined successfully, exit with a non-zero status
    
    The default is to follow symbolic links, as if `-L' were specified.
    
    Exit Status:
    Returns 0 if the directory is changed, and if $PWD is set successfully when
    -P is used; non-zero otherwise.
```

可以看到，`cd`命令帮我们改变了当前shell的工作目录，并设置`$PWD`环境变量的值。
如果不加任何参数，`cd`命令将使用环境变量`$HOME`（返回当前用户的家目录)。
`-P`选项表示使用实际的路径名，而非符号链接(symbolic links)。

