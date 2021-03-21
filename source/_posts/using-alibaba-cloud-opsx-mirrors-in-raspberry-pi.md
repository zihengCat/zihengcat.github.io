---
title: Raspberry Pi 使用阿里云 OPSX 镜像
subtitle: Using Alibaba Cloud OPSX Mirrors in Raspberry Pi
catalog: true
date: 2018-05-14
tags: [Linux, Raspberry Pi]
---

# 前言

本文记录在树莓派「Raspberry Pi」下使用阿里云「OPSX」开源镜像。

# 阿里云开源镜像站 - OPSX

阿里家的开源镜像站（OPSX），提供了主流开源仓库的软件镜像，非常良心。开源镜像的目的在于宣传自由软件的价值，提高自由软件社区文化氛围，推广自由软件在国内应用。在推广开源的道路上，阿里真是走在全国最前列。

阿里云开源镜像站由阿里巴巴「天基团队」提供支持。目前提供 Debian、Ubuntu、Fedora、Arch Linux、CentOS、OpenSUSE、Scientific Linux、Gentoo 等多个发行版的软件安装源和ISO下载服务，竭力为互联网用户提供全面，高效和稳定的开源软件服务。

> 官方网址：https://opsx.alibaba.com/

![OPSX][OPSX]

> 图：阿里云开源镜像站 LOGO 图（取自 OPSX 官网）

# 镜像仓库使用方法

阿里云开源镜像站有提供树莓派「Raspberry Pi」的软件镜像。使用方法与 Debian 系统类似，修改`/etc/apt/source.list`即可（**使用前请先备份原配置文件**）。

编辑`/etc/apt/source.list`文件，将 Raspbian 官方源地址注释掉，添加上阿里云的镜像地址。注意，镜像地址应使用`https://mirrors.aliyun.com/raspbian/raspbian/`而不是`https://mirrors.aliyun.com/raspbian/`。

```plain
# Raspbian Official
#deb http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi

# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi

# Aliyun OPSX Mirror
deb https://mirrors.aliyun.com/raspbian/raspbian/ stretch main contrib non-free rpi
```
> 注：
> `stretch` -> Raspbian系统版本（根据实际系统填写）
> `main contrib non-free rpi` -> 启用的软件仓库（类别）

修改完成后，使用`apt`命令清理缓存文件，重建软件包索引，树莓派「Raspberry Pi」就可以正常使用阿里云镜像地址了。

```
$ sudo apt clean all
$ sudo apt update
```
> 代码清单：`apt`更新「Raspberry Pi」系统软件包

# 参考资料

- Alibaba OPSX: https://opsx.alibaba.com/

[OPSX]: ./opsx.png

