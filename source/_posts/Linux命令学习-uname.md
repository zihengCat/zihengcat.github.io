---
title: Linux命令学习-uname
date: 2017-06-17 
tags: Linux
---

# uname

`uname`命令可以打印当前系统信息。
`uname`是*GNU coreutils*工具之一，遵循*IEEE POSIX*规范。


# 详解

```
$ uname --help
Usage: uname [OPTION]...
Print certain system information.  With no OPTION, same as -s.

  -a, --all                print all information, in the following order,
                             except omit -p and -i if unknown:
  -s, --kernel-name        print the kernel name
  -n, --nodename           print the network node hostname
  -r, --kernel-release     print the kernel release
  -v, --kernel-version     print the kernel version
  -m, --machine            print the machine hardware name
  -p, --processor          print the processor type or "unknown"
  -i, --hardware-platform  print the hardware platform or "unknown"
  -o, --operating-system   print the operating system
      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'uname invocation'
```

- s: 核心名
- o: 操作系统名
- n: 网络用户名
- r: 内核发行版
- v: 内核版本号
- m: 硬件名
- p: 处理器架构
- i: 硬件架构
- a: 打印以上所有信息

我们可以在shell脚本中使用`uname`检测当前系统环境。

