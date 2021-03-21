---
title: Python学习笔记 - 使用Python虚拟环境
date: 2018-01-26
tags: Python
---

# 前言

在项目开发中，我们有时会希望项目有一个专属环境（项目自有目录，特定Python版本，安装指定第三方软件包等），而与系统环境区分开来。

- 不同应用开发环境相互独立
- 环境升级不影响其他应用，也不影响全局Python环境
- 有效防止软件包混乱与软件版本冲突

> 使用Python虚拟环境的优点

# Python3 虚拟环境

Python3 提供了`venv`模块，可以为我们创建出轻量级的*虚拟环境*。该虚拟环境是将全局Python环境复制一份出来给予特定项目。

## 创建虚拟环境

使用 Python3 自带的`venv`模块即可轻松创建一个虚拟环境。

```
python3 -m venv /path/to/new/virtual/environment
```
> 命令后接创建虚拟环境的路径

命令执行完后，我们发现Python已经自动为我们创建好了目录，目录下存放着Python的主二进制文件，标准库文件与一些配置文件（从系统环境中复制而来）。

## 激活与退出

创建完虚拟环境后，我们通过执行`path/bin`目录下的`activate`激活脚本即可在当前Shell下激活虚拟环境（Shell环境不同，激活脚本也不同）。

```
cd /path/to/new/virtual/environment
source ./bin/activate
```
> 使用`source`激活虚拟环境

想要退出当前虚拟环境也非常简单，在虚拟环境命令行下键入`deactivate`即可。

```
(virtual environment)$ deactivate
```
> 执行`Bash`函数退出虚拟环境

## 使用虚拟环境

激活虚拟环境后，我们可以在该虚拟环境下执行任何操作（安装第三方软件包等），而这些操作都与系统Python环境毫不相关。

需要注意的是，默认情况下，虚拟环境与系统环境是隔离的，虚拟环境下也没有系统环境中已经安装的第三方软件包，所有用到第三方软件包都需要我们在虚拟环境下再自行下载一遍（pip默认安装）。

## `venv`命令选项

使用`-h`选项可以查看`venv`命令的帮助文档。

```
python3 -m venv -h
```
简单翻译一下一些常用的选项。

| 选项     | 含义 |
|:--------:|:---- |
| `--system-site-packages` | 虚拟环境可访问系统环境下的第三方软件包 |
| `--symlinks` | 使用软链接而不是直接复制文件 |
| `--copies`   | 直接复制文件而不使用软链接   |
| `--clear`    | 如果目录已有内容存在，清空目录再创建虚拟环境 |
| `--upgrade`  | 更新虚拟环境 |
| `--without-pip` | 创建虚拟环境时不安装`pip`工具（默认安装） |

> 表: `venv`简单功能翻译

# 参考资料

- Python官方文档：https://docs.python.org/3/library/venv.html

