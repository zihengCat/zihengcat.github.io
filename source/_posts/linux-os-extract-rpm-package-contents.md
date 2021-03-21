---
title: Linux 系统下提取 RPM 包内容
subtitle: Linux OS Extract RPM Package Contents
date: 2017-07-24
tags: ["Linux", "Tips"]
---

# 前言

对于红帽系（Red Hat）的 Linux 系统而言，大多使用`rpm`作为系统包管理工具（RPM Package Manager）。

我们可以使用命令`rpm -qlp`来查看一枚`rpm`包所包含的文件。那么，如何在不安装该`rpm`包的情况下提取出包中的文件呢？

# 提取`RPM`包内容

为了提取`rpm`包中的文件，我们需要用到两款工具：`rpm2cpio`与`cpio`。使用如下命令组合，即可将`rpm`包内容文件提取出来。

```bash
$ rpm2cpio <RPM_PACKAGE.rpm> | cpio -idmv
```
> 代码清单：提取`rpm`包文件

`rpm`包默认使用`cpio`格式进行归档打包，`rpm2cpio`工具可以将`rpm`包转换为`cpio`归档，该命令默认将内容直接输出到标准输出流`stdout`。

`cpio`工具主要用来建立或者还原备份归档，命令可直接读取并处理标准输入流`stdin`，与`tar`工具类似。

- `-i`
extract，导出文件。

- `-d`
make-directories，建立新目录存放导出文件。

- `-m`
保持文件的更新时间。

- `-v`
verbose，显示命令详情。

# 参考资料

- RPM Package Manager: http://rpm.org/

