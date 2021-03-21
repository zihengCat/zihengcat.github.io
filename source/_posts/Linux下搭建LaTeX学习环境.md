---
title: Linux下搭建LaTeX学习环境
date: 2017-07-09
tags: LaTeX
---

# 前言

本文讲解如何在Linux下安装`LaTeX`环境: `TeX Live`。

# TeX简介

![TeX-1](./tex-1.png)

> TeX（希腊语：/tɛx/[1]，音译“泰赫”，文本模式下写作TeX），是一个由美国计算机教授高德纳（Donald Ervin Knuth）编写的功能强大的排版软件。它在学术界十分流行，特别是数学、物理学和计算机科学界。TeX被普遍认为是一个优秀的排版工具，特别是在处理复杂的数学公式时。利用诸如是LaTeX等终端软件，TeX就能够排版出精美的文本以帮助人们辨认和查找。

子恒喵对`TeX`排版软件的浅显理解：Microsoft Word的替代品，可以使用纯文本与命令行完成精美的排版任务。

# TeX, LaTeX 与 TeX Distribution

![TeX-2](./tex-2.png)

> 其实世界上只有一个TeX程序，它就叫做 "TeX", 它是由计算机科学家 D. E. Knuth 设计并且实现的。TeX 不仅是一个排版程序，而且是一种程序语言。LaTeX 就是用这种语言写成的一个“TeX 宏包”，它扩展了 TeX 的功能，使我们很方便的逻辑的进行创作而不是专心于字体，缩进这些烦人的东西。TeX 还有其它的大型宏包，它们和 LaTeX 一起都被叫做 "format"，现在还有一种常用的format叫做 ConTeXt, 用它能方便的作出极其漂亮的幻灯片，动态屏幕文档…… 我们通常用 TeX 都是在用 LaTeX, ConTeXt, 因为 TeX 的底层需要更多的知识才能了解，一般人不需要自己设计自己的格式。

类比说明：

| TeX | LaTeX | TeX Distribution |
| --- | ----- | ---------------- |
| 程序设计语言 | 语言的标准库 | 编译器, 开发工具, 第三方库, 说明文档等的集合 |

我们要安装的`TeX Live`正是一支TeX Distribution。

# TeX Live简介

![TeX Live Logo](./tex_live_Logo.png)

> TeX Live是由国际TeX用户组（TeX Users Group，TUG）整理和发布的TeX软件发行套装，包含与TeX系统相关的各种程序、编辑与查看工具、常用宏包及文档、常用字体及多国语言支持。TeX Live是许多Linux/Unix系统（比如Fedora、Debian、Ubuntu、Gentoo以及OpenBSD、FreeBSD、NetBSD等）默认或推荐的TeX套装，同时也支持包括Windows和Mac OS X等在内的其它操作系统。TeX Live是开发状态最为活跃的TeX发行版之一，保持着每年一版的更新频率。TeX Live属于免费软件。

TeX Live是目前最好的TeX发行版，每年都会进行版本更新。

# 安装TeX Live

对于`TeX Live`的安装，推荐使用`TeX Live`官方的**ISO镜像包**，不要使用`yum`或`apt`安装。

## Step 1 获取安装镜像

前往Tug的镜像下载站点，下载最新的ISO镜像安装包:

**站点地址: https://www.tug.org/texlive/acquire-iso.html**

安装包接近4GB，选择最近的镜像站下吧。

## Step 2 安装

在安装时需要用到perl的MD5校验与`perl-Tk`图形界面（可选）。
安装依赖包:
```
$ sudo yum -y install wget
$ sudo yum -y install perl-Digest-MD5
$ sudo yum -y install perl-Tk
```
挂载镜像文件：
```
$ sudo mount -o loop <filename>.iso /mnt
$ cd /mnt
```
启动安装脚本，可以选择图形界面（`-gui`），也可以命令行操作（`-no-gui`）：
```
$ sudo ./install-tl -gui
```

![texlive-2017](./texlive2017.png)

图形界面下，各种安装选项清晰明了。调整相关参数后安装即可。
安装完成后，将安装目录加入到`PATH`之中：

```
$ echo 'export PATH=${PATH}:<install path>/2017/bin' >> ~/.bashrc
```

## Step 3 检验安装

命令行下键入`tex`命令，如果可以正确打印出软件信息，即说明安装成功。

```
$ tex -v
TeX 3.14159265 (TeX Live 2017)
kpathsea version 6.2.3
Copyright 2017 D.E. Knuth.
There is NO warranty.  Redistribution of this software is
covered by the terms of both the TeX copyright and
the Lesser GNU General Public License.
For more information about these matters, see the file
named COPYING and the TeX source.
Primary author of TeX: D.E. Knuth.
```

# 参考资料

- TeX介绍: https://ctan.org/tex/
- TeX说明: http://www.ctex.org/documents/shredder/tex_frame.html

