---
title: 使用 Python 包管理器 Pip
subtitle: Using Pip Python Package Manager
catalog: true
date: 2017-11-12
tags: Python
---

# 前言

`pip`是一款由 The Python Packaging Authority（PyPA）社区开发维护的、简单好用的**`Python`包管理工具**。现如今，Python 开发者社区已认可`pip`作为 Python 程序设计语言的**默认包管理器**。

本文记录下一些常用的`pip`命令选项。

# `pip`常用选项

## 通用（General）

| 选项              | 含义                |
|:---------------   | :-----------------  |
|`--help`           | 打印帮助页面        |
|`--version`        | 打印版本信息        |
|`--verbose`        | 显示详细信息        |
|`--quiet`          | 显示简略信息        |
|`--no-cache-dir`   | 不使用缓存          |
|`--proxy <proxy>`  | 使用代理            |

> 表：`pip`通用选项表

## 搜索（Search）

`pip`搜索命令，直接输入搜索字符串，`pip`就会在PyPI仓库中搜索相关的软件包。

```
$ pip search [options] <query>
```
> 代码清单：`pip`搜索命令（格式）

## 安装（Install）

`pip`安装软件包分为**在线安装**与**离线安装**两类。

在线安装软件包，全局安装可能需要提供 *sudo* 权限。

```
$ pip install [options] <package> ...
```
> 代码清单：`pip`在线安装命令（格式）

离线安装软件包（`.whl`是 Python 软件包通用打包格式）。

```
$ pip install <package.whl> ...
```
> 代码清单：`pip`离线安装命令（格式）

想要更新已安装软件包，则可以使用`--upgrade`选项。也可以指定从`requirement.txt`依赖文件中安装软件包。

| 选项                  | 含义                       |
| :-------------------- | :------------------------  |
| `-U`, `--upgrade`     | 更新已安装的包             |
| `-r`, `--requirement` | 递归安装文件列表中的依赖包 |
| `--no-cache-dir`      | 禁用离线下载缓存           |

> 表：`pip`安装选项表

## 卸载（Uninstall）

`pip`卸载命令，用于卸载已安装的包。

```
$ pip uninstall [options] <package> ...
```
> 代码清单: pip 卸载软件包命令格式

| 选项                  | 含义                       |
| :-------------------- | :------------------------- |
| `-y`, `--yes`         | 直接确认卸载               |
| `-r`, `--requirement` | 递归卸载文件列表中的依赖包 |

> 表：`pip`卸载选项表

## 列表（List）

`pip`列表命令，可以列出已安装包的相关信息。

注意，`pip`的**检查更新**功能是放在`list`里的。

```
$ pip list [options]
```
> 代码清单：`pip`卸载软件包命令（格式）

| 选项         | 含义                           |
| :----------  | :----------------------------  |
| `--outdated` | 列出有更新的包（**检查更新**） |
| `--uptodate` | 列出已更新的包                 |
| `--local`    | 仅列出在虚拟环境（venv）中的包 |

> 表：`pip`列出选项表

## 显示（Show）

`pip`显示命令，查看已安装包的详细信息。

| 选项      | 含义                     |
| :-------: | :----------------------: |
| `--files` | 显示该软件包中的所有文件 |

> 表：`pip`显示选项表

## 依赖（Freeze）

`pip freeze`命令，可以生成一份**依赖包列表**，该命令生成的列表格式可被`pip`其他命令选项接受使用。该命令常用来生成项目依赖表。

# 更换`pip`源

`pip`使用 PyPI 仓库来为我们提供 Python 软件包。但是默认的官方仓库地址位于国外，国内访问不畅，时不时会遇上下载个包要花很长时间，甚至超时无法下载的情况。

我们可以利用`-i`选项使用自定义镜像仓库。

> PyPI 默认仓库地址：https://pypi.python.org/pypi

| 选项                   | 含义                     |
| :--------------------: | :----------------------: |
| `--index <mirror_url>` | 自定义 Python 包仓库地址 |

> 表：`pip`换源选项表

## PyPI 国内镜像推荐

由于国内恶劣网络条件的限制，在国内使用官方 PyPI 源时常会有**卡断，速度缓慢**的情况。为了解决这一问题，我们可以选择使用一些 PyPI 国内镜像。

| 提供方   | 地址                                         |
| :------: | :------------------------------------------- |
| 阿里云   | https://mirrors.aliyun.com/pypi/simple/      |
| 豆瓣网   | http://pypi.doubanio.com/simple/             |
| 清华大学 | https://pypi.tuna.tsinghua.edu.cn/simple/    |
| 中科大   | https://mirrors.ustc.edu.cn/pypi/web/simple/ |

> 表：一些 PyPI 国内镜像

## 如何使用 PyPI 镜像

用 PyPI 镜像非常简单，一次性使用的话，在`pip`安装命令后使用`-i`参数接想使用的镜像地址即可。

```
$ pip install -i <mirror_url> some-packages
```
> 代码清单：`pip`使用 PyPI 镜像

修改`pip.conf`文件可以将自定义镜像地址设为默认。操作系统（OS）不同，`pip.conf`配置文件存放位置也会不尽相同。如果对应目录下的配置文件不存在，手动创建即可。

| 操作系统  | 文件位置        |
| :------:  | :-------------- |
| Linux     | `~/.pip/pip.conf` |
| macOS     | `$HOME/Library/Application Support/pip/pip.conf` |
| Windows   | `%APPDATA%\pip\pip.ini` |

> 表：`pip.conf`存放位置表

我们可以在`pip.conf`配置文件中全局指定 PyPI 镜像地址。

```
...
[global]
index-url = <mirror_url>
...
[install]
trusted-host = <mirror_host>
...
```
> 代码清单：`pip.conf`全局指定 PyPI 镜像地址

# 参考资料

- `pip`官方文档：https://pip.pypa.io/en/latest/

