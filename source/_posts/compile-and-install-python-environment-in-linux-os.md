---
title: Linux 下搭建 Python 环境（编译安装）
subtitle: Compile and Install Python Environment in Linux OS
catalog: true
date: 2017-07-06
tags: ["Linux", "UNIX", "Python"]
---

# 前言

本文主要讲解在 Linux 系统下编译安装 Python 环境，也同样适用于其他 UNIX-like 系统。

在 *CentOS 7 Linux* 中，默认情况下系统默认不安装 Python 3，而`yum`源中提供的`python3`软件包也并非最新版本。系统自带了一枚旧版本 Python 2 为一些系统组件提供支持，不应随意更动，不然会出现各种各样的问题（如：`yum`无法正常使用...）。

因此，手动编译安装最新版本 Python 成为 Linux 下搭建 Python 环境的绝佳选择。

# Step 1 - 准备编译环境

首先，我们需要在本机安装「基础编译工具链」，Python 语言的官方实现`cpython`使用 C 语言写成，我们需要`gcc`、`make`等基础编译工具用以编译 Python 源代码。

```bash
$ sudo yum install gcc make
...
```
> 代码片段：安装「基础编译工具」

# Step 2 - 获取源代码

前往 Python 官方网站，下载最新 Python 3 「源码包」（Source Tarball）。

> Python 官方网址：https://www.python.org

```bash
$ curl -O https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz
...
```
> 代码片段：获取 Python 源码包

下载源码包后，解压缩 + 解包，`*.tar.xz`压缩包需要分两步解压，`*.tar.gz`则可以使用`tar -xvz -f`一步到位。

```bash
$ xz -d ./Python-3.6.1.tar.xz
...
$ tar -xv -f ./Python-3.6.1.tar
...
```
> 代码片段：解压缩解包 Python 源码包

亦或者，我们也可以选择克隆 GitHub 上`cpython`代码仓库的方式获取 Python 源代码。

```bash
# 克隆`cpython`源码仓库
$ git clone https://github.com/python/cpython.git
# 进入`cpython`源码目录
$ cd cpython
# 查看`cpython`版本号
$ git tag
# 检出适合`cpython`版本
$ git checkout <tag-version>
```
> 代码片段：Git 获取 Python 源代码

# Step 3 - 安装依赖项

进入到 Python 源代码根目录，我们可以先浏览一下`README.rst`文件，该 README 文档指导我们编译安装 Python 的具体步骤。

在编译安装之前，我们需要先安装一些 Python 依赖包，这是 README 上没有提到的（但会在`configure`检查依赖项时告知）。为了省去后续查找安装依赖的麻烦，此处将 Python 所需要的依赖包列出。

手写一支 Bash 自动化脚本，把依赖包全部装好。对于 Debian 系 Linux 系统，使用`apt`命令安装相应依赖包即可。

| Library  | RedHat           | Debian             |
| :------- | :--------------- | :----------------- |
| ZLib     | `zlib-devel`     | `zlib1g-dev`       |
| BZip2    | `bzip2-devel`    | `libbz2-dev`       |
| LZMA     | `xz-devel`       | `liblzma-dev`      |
| UUID     | `uuid-devel`     | `uuid-dev`         |
| OpenSSL  | `openssl-devel`  | `libssl-dev`       |
| RaedLine | `readline-devel` | `libreadline-dev`  |
| NCurses  | `ncurses-devel`  | `libncursesw5-dev` |
| SQLite   | `sqlite-devel`   | `libsqlite3-dev`   |
| FFI      | `libffi-devel`   | `libffi-dev`       |
| GDBM     | `gdbm-devel`     | `libgdbm-dev`      |
| Tcl/Tk   | `tk-devel`       | `tk-dev`           |

> 表：Linux 系统下 Python 依赖库表

```bash
#/bin/bash

PYTHON_DEPS="zlib-devel \
             bzip2-devel \
             xz-devel \
             uuid-devel \
             libuuid-devel \
             openssl-devel \
             readline-devel \
             ncurses-devel \
             sqlite-devel \
             libffi-devel \
             gdbm-devel \
             tcl-devel \
             tk-devel"

for i in ${PYTHON_DEPS}
do
    sudo yum -y install ${i}
done

echo ${?}
```
> 代码片段：安装 Python 依赖库 Bash 自动化脚本

# Step 4 - 编译安装

安装好依赖包后，我们按照 README 的指引，编译安装 Python 。请确保在以下操作在 Python 源代码根目录下执行。

`configure` 指令会检查当前系统的编译环境、软件依赖性等，如果检查确认没有问题，则会成功生成`makefile`。这里我们配置两枚编译选项，用于自定义安装路径与开启编译优化，更多编译选项可以参看：`./configure --help`）。

```bash
$ ./configure --prefix=/usr/local/python_3.6.1 --enable-optimizations
...
```
> 代码片段：`configure`编译配置选项

| 编译选项                  | 解释说明   |
| :----------------------- | :------- |
| `--prefix`               | 自定义安装路径，默认为`/usr/local`。个人习惯单独指定安装路径，这样管理起来比较方便。 |
| `--enable-optimizations` | 开启编译优化选项，增加编译时间，换取性能提升。这是非常值得的。 |

> 表：自定义编译选项表

执行`make`编译安装，编译时间取决于机器性能，多核机器可以加入`-j`选项开启多任务加快编译速度。如果使用虚拟机，建议将虚拟机性能调高一些。

```bash
$ make
...
$ sudo make install
...
```
> 代码片段：`make`编译安装

如果配置了`--prefix`编译选项自定义安装目录，安装完成后不要忘记将自定义 Python 路径添加到`PATH`环境变量中，并将 Python 二进制软链接到`/usr/bin/`目录下。

```bash
$ echo 'export PATH=${PATH}:/path/to/python/bin' >> ~/.bashrc
$ source ~/.bashrc
```
> 代码片段：添加 Python 安装目录到系统 PATH 路径

# Step 5 - 检验安装

最后我们验证一下 Python 编译安装结果，命令行下执行 Python 指令，如果编译、安装、配置都没有问题，则会正常进入 Python 交互式环境。

```bash
$ python3
Python 3.6.1 (default, Jul  6 2017, 15:12:56)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hello, world")
hello, world
>>>
```
> 代码片段：检验 Python 安装

至此，最新版 Python 安装完成，可以愉快地使用 Python 写程序了。

<!-- EOF -->
