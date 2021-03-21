---
title: Linux命令学习-cat
date: 2017-06-14
tags: Linux
---

# cat

Linux上还有猫命令`cat`?!
其实，这个命令不是猫咪的意思啦...`cat`是*concatenate*的意思。
`cat`命令是一般用来查看文件内容(文本文件)。

# 详解

```
$ cat --help
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s), or standard input, to standard output.

  -A, --show-all           equivalent to -vET
  -b, --number-nonblank    number nonempty output lines, overrides -n
  -e                       equivalent to -vE
  -E, --show-ends          display $ at end of each line
  -n, --number             number all output lines
  -s, --squeeze-blank      suppress repeated empty output lines
  -t                       equivalent to -vT
  -T, --show-tabs          display TAB characters as ^I
  -u                       (ignored)
  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
      --help     display this help and exit
      --version  output version information and exit

With no FILE, or when FILE is -, read standard input.

Examples:
  cat f - g  Output f's contents, then standard input, then g's contents.
  cat        Copy standard input to standard output.

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'cat invocation'
```

`cat`后面接一个文件，就是将该文件的内容显示到标准输出;
不接文件的话，就是打印标准输入到标准输出。

- `-E`选项: 将文件中的断行符以`$`的形式显示出来。
- `-T`选项: 将文件中的制表符以`^I`的形式显示出来。
- `-v`选项: 将文件中的非打印字符以特殊标记的形式显示出来(不包括断行符与制表符)。
- `-A`选项: 等同于`-vET`。
- `-n`选项: 为输出的文本行加上行号。
- `-s`选项: 输出时去掉空白行。
- `-b`选项: 等同于`-s`。

只要记住，`-A`显示非打印字符，`-n`显示行号，`-s`去除空行，就可以啦！

