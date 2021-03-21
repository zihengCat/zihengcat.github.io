---
title: Linux下编译安装Vim并添加Python支持
date: 2017-08-11
tags: [Linux, Vim]
---

# 前言

*Vim*一直是个人最钟爱的文本编辑器（Text Editor），为Vim添加上Python的支持，更使得Vim如虎添翼。本文讲解在Linux系统下编译安装Vim并添加Python支持。

# Step 1 - 搭建Python环境

Python目前有两个大版本，Python2与Python3，这两个版本并不兼容。我们可以选择安装任意版本，也可以把两个版本都安装上。使用系统包管理器安装或者手动编译安装都可以，手动编译安装可以参看这篇博文。

> Linux下搭建Python3环境（编译安装）：
> https://zihengcat.github.io/2017/07/06/Linux%E4%B8%8B%E6%90%AD%E5%BB%BAPython3%E7%8E%AF%E5%A2%83

# Step 2 - 获取源代码

获取`vim`源代码，可以直接拉取`vim`在GitHub上的镜像仓库，也可以前往`vim`官网下载源码包。

> Vim官网：http://www.vim.org
> GitHub仓库：https://github.com/vim/vim

```
$ git clone https://github.com/vim/vim.git
```
> 代码清单：`git`拉取`vim`源码

# Step 3 - 解决依赖性

本机安装好`gcc`，`make`等基础编译工具。`vim`所需要的依赖库相当少，编译安装时基本不会遇到依赖性问题。如果缺少依赖包（如提示需要安装终端操作库`ncurses`），`configure`会给出报错提示信息，按照提示装好相应的依赖库即可。

## Step 4 - 添加配置选项

`vim`有着非常丰富的配置选项，需要添加哪些功能，就把相应的配置选项打开。`vim`对`Ruby`，`Perl`，`Lua`等的动态语言都有提供支持接口，当然前提是系统已经安装好了相应环境。这里我们只开启对`Python`语言的支持。

```
./configure --with-features=huge \
            --enable-gui=no \
            --enable-pythoninterp=yes \
            --with-python-config-dir=/usr/lib64/python2.7/config \
            --enable-python3interp=yes \
            --with-python3-config-dir=/usr/local/packages/python-3.6.2/lib/python3.6/config-3.6m-x86_64-linux-gnu \
            --disable-selinux \
            --disable-sysmouse \
            --disable-nls \
            --prefix='/usr/local/packages/vim_80/' \
            CFLAGS='-O3  -Wall'
```
> 个人编译配置选项：不添加 GUI 与多语言环境的支持
> 更多编译选项参看：`./configure --help`

注意这几枚配置选项：

| 选项 | 含义 |
|:-----|:---- |
| `--prefix`                         | 指定安装目录         |
| `--enable-pythoninterp=yes`        | 打开`Python2`支持      |
| `--with-python-config-dir=<PATH>`  | 指定`Python3`配置库路径|
| `--enable-python3interp=yes`       | 打开`Python3`支持      |
| `--with-python3-config-dir=<PATH>` | 指定`Python3`配置库路径|

> 表：`vim`编译配置选项表

在开启Python支持选项后，还需要指定Python配置库的路径。Python的配置库路径一般在：`<PATH>/lib/pythonx.x/config-xx/*`下。其中`<PATH>`是Python的安装目录，具体路径，不同版本会略有不同。

## Step 5 - 编译安装

开源软件标准编译安装流程：`./configure && make && make install`。如果配置没有什么问题，应该可以顺利地编译安装。

```
$ make
$ sudo make install
```
> 代码清单：编译安装`vim`流程

如果自定义了安装目录，注意在安装完成后添加安装目录至系统`<PATH>`路径。

## Step 6 - 检验安装

```
$ vim --version
...
```
> 代码清单：打印`vim`版本信息

安装没问题的话，可以看到Vim的版本信息。编译好的Vim已经包含了对Python的支持，并正确链接到Python的配置库。

```
:python import sys; print(sys.version)
:python3 import sys; print(sys.version)
```
> 代码清单：验证`vim`的Python支持功能

进入`vim`，输入以上指令，如果可以顺利打印出Python版本号，说明`vim`的Python支持确实可用。

