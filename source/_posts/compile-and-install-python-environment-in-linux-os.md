---
title: Linux 下搭建 Python 环境（编译安装）
subtitle: Compile and Install Python Environment in Linux OS
catalog: true
date: 2017-07-06
tags: ["Linux", "Python"]
---

# 前言

本文讲解在 Linux 操作系统（OS）下编译安装 Python 环境。

在 *CentOS 7 Linux* 中，操作系统（OS）默认没有安装`Python3`，`yum`源中提供的`Python3`软件包也并非最新版本。系统自带一枚旧版本`Python 2`（Python 2.7.5）为一些系统组件提供支持，不宜随意更动，不然会出现各种各样的 Bugs（如：`yum`无法正常使用...）。

手动编译安装最新版本 Python ，是 Linux 下搭建 Python 环境的最佳选择。

# Step 1 - 编译环境

首先，我们需要在本机安装「基础编译工具链」，这是编译 Python 源码所必需的。

```bash
$ sudo yum install gcc make
...
```
> 代码清单：安装「基础编译工具」

本机安装好`gcc`、`make`基础编译工具，用来编译 Python 源码（Python 语言的官方实现是 *CPython* ）。

# Step 2 - 获取源码

前往 Python 官方网站，获取最新 Python3 「源码包」。注意，要下载源码包`source tarball`。

> Python 官方网址：https://www.python.org

```bash
$ curl -O https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz
```
> 代码清单：获取`Python`源码包

解压缩，解包，`*.tar.xz`压缩包需要分两步解压，`*.tar.gz`则可以使用`tar -xvz -f`一步到位。

```bash
$ xz -d ./Python-3.6.1.tar.xz
$ tar -xv -f ./Python-3.6.1.tar
```
> 代码清单：解压缩`Python`源码包

亦或者，我们也可以直接克隆 GitHub 上`cpython`代码仓库至本地。

```bash
# 克隆`cpython`源码仓库
$ git clone https://github.com/python/cpython.git
# 进入`cpython`源码目录
$ cd cpython
# 查看`cpython`版本号
$ git tag
# 检出适合`cpython`版本
$ git checkout <tag_version>
```
> 代码清单：使用`git`获取 Python 源代码

# Step 3 - 安装依赖

`cd`进入`Python`源码目录，我们可以先浏览一下`README.rst`文件，该文档指导我们编译安装 Python 的具体步骤。

在编译安装之前，我们需要先安装一些 Python 依赖包，这是`README`上没有提到的（但是会在`./configure`检查依赖项时告知）。为了省去后续查找安装依赖的麻烦，这里把 Python 所需要的依赖包全部列出。

| 依赖库   | RedHat           | Debian             |
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

> 表：Linux 系统下`Python`依赖库表

```bash
#/bin/bash
# --------------------------
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
# --------------------------
for i in ${PYTHON_DEPS}
do
    sudo yum -y install ${i}
done
# --------------------------
echo ${?}
```
> 代码清单：RedHat Linux 下安装 Python 依赖库自动化脚本

手写一支`shell`脚本，把依赖包全部装好。对于`Debian`系的系统，可以使用`apt`命令安装相应的依赖包。

# Step 4 - 编译安装

依赖包都安装好之后，接下来，我们就按照`README`的指引，编译安装`Python`。请确保在 Python 源代码目录下执行以下操作：

```bash
$ ./configure --prefix=/usr/local/python_3.6.1 --enable-optimizations
```
> 代码清单：`configure`编译配置选项

此命令会检查当前系统的编译环境，软件依赖性等等，如果没有问题的话，会成功生成`Makefile`。

这里我们添加了两个编译选项，更多编译选项可以参看：`./configure --help`。

| 编译选项                 | 解释说明 |
| :----------------------- | :------- |
| `--prefix`               | 自定义安装路径，默认在`/usr/local`，个人习惯单独指定安装路径，这样管理起来比较方便。 |
| `--enable-optimizations` | 开启编译优化选项，增加编译时间，换取性能提升，这是非常值得的。 |

> 表：自定义编译选项表

```bash
$ make
...
$ sudo make install
...
```
> 代码清单：`make`编译安装

运行`make`编译安装，编译时间取决于机器性能。如果是虚拟机环境，建议把虚拟机性能调高一些。

如果使用`--prefix`编译选项自定义了安装目录，安装完成后不要忘记了将安装路径加入`PATH`环境变量中，并将 Python 软链接到`/usr/bin/`目录下。

```bash
$ echo 'export PATH=${PATH}:<path/to/python/bin>' >> ~/.bashrc
```
> 代码清单：加入`Python`至系统`PATH`路径

# Step 5 - 检验安装

命令行下直接执行 Python 指令，如果编译、安装、配置都没有问题，会正常进入 Python 交互式环境。

```bash
$ python3
Python 3.6.1 (default, Jul  6 2017, 15:12:56)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
> 代码清单：检验`Python`安装

至此，最新版 Python 已经安装完成，我们可以愉快地使用 Python 写程序啦。

<!-- EOF -->
