---
title: Linux 下使用 OpenVPN 连接远程虚拟专有网络
subtitle: Install and Configure OpenVPN in Linux OS
date: 2018-06-23
tags: ["Linux", "Tips"]
catalog: true
header-img: header.png
header-mask: 0.3
---

# 前言

VPN (Virtual Private Network) 虚拟专有网络通过在公网上创建一个专用且加密的Tunnel（网络隧道）进行通讯。在实际的应用中，VPN可以帮助远程用户、公司分支机构、商业伙伴及供应商等之间建立一个安全可信的网络连接。

![OpenVPN][openvpn]

> 图：OpenVPN LOGO

`OpenVPN`是一款全功能的 VPN 软件，包含了客户端与服务端，它使用行业标准`SSL/TLS`协议实现 OSI 第 2 层或第 3 层安全网络扩展，支持电子证书，公开密钥，用户名/密码等多种灵活客户端认证方式，并允许用户使用防火墙规则应用于 VPN 虚拟接口的组专用访问控制策略，具有**开源、跨平台、易于使用、稳定、安全**等特点。`OpenVPN`是一款开源软件，以`GPLv2`开源协议释出。

> OpenVPN 官网：http://openvpn.net

> GitHub 地址：https://github.com/OpenVPN/openvpn

# Linux下安装`OpenVPN`（编译安装）

Windows与macOS下都已经有了优秀的`OpenVPN`图形化（GUI）工具，安装配置起来都非常简单方便。

| OS      | App         | Link                                       |
| :-----: | :---------: | :----------------------------------------: |
| Windows | OpenVPN-GUI | https://github.com/OpenVPN/openvpn-gui     |
| macOS   | Tunnelblick | https://github.com/Tunnelblick/Tunnelblick |

> 表：`OpenVPN`图形化（GUI）工具

而对于 Linux 系统，手动编译安装是使用`OpenVPN`的极佳方法。当然，使用系统级包管理器（`apt`，`yum`）直接安装也未尝不可。本文讲解从源码手动编译安装`OpenVPN`。

`OpenVPN`的源代码托管在了 GitHub 上，我们可以非常方便地使用`git`拉取源码。也可以前往`OpenVPN`源代码下载页面，获取源码包。

> 源码包下载地址：https://openvpn.net/index.php/download/community-downloads.html

> GitHub Releases：https://github.com/OpenVPN/openvpn/releases

> 注：`OpenVPN`源码包下载地址国内访问不畅，需翻墙。

无论使用哪种方法，获取源码都是我们编译安装软件的第一步。下载到源码包后，使用`tar -xvz -f`解压缩解包`*.tar.gz`。

```bash
# 克隆源代码仓库
$ git clone https://github.com/OpenVPN/openvpn.git
...
# 进入源代码目录
$ cd ./openvpn
# 查看软件发行版本（tags）
$ git tag -l
...
# 切换到目标版本（v2.4.6）
$ git checkout v2.4.6
...
```
> 代码清单：使用`git`取得`OpenVPN`源代码

`OpenVPN`与其他 Linux 开源软件的编译安装方式大同小异，主要都是分为三步走：`configure` -> `make` -> `make install`。

有所不同的是，`OpenVPN`需要先使用`autoconf`生成一份`configure`配置脚本。请确保你的操作系统已经安装上了 GNU 基础编译套件：`gcc`，`make`，`autoconf`...

```bash
# 使用`autoconf`生成`configure`配置脚本
$ autoreconf -i -v -f
...
$ ./configure
...
$ make
...
$ sudo make install
...
```
> 代码清单：编译安装`OpenVPN`流程

下表总结了编译`OpenVPN`所需要的依赖库，可以通过系统包管理工具安装上，不同系统包名略有差异。如果系统缺失依赖库，执行`./configure`配置检查时会有报错提示。

| 依赖库  | `deb`包（`apt`安装） | `rpm`包（`yum`安装） | 简述                                 |
| :-----: | :-------------- ---: | :------------------: | :----------------------------------- |
| OpenSSL | libssl-dev           | openssl-devel        | `SSL/TLS`开源实现库                  |
| Route   | net-tools            | net-tools            | 基础网络工具`route`                  |
| LZO     | liblzo2-dev          | lzo-devel            | 一种致力于解压速度的数据压缩无损算法 |
| PAM     | libpam0g-dev         | pam-devel            | 本地认证模块                         |

> 表：`OpenVPN`依赖库表

编译安装完成后，我们可以执行命令来确认`OpenVPN`是否成功安装。

```bash
# 打印`OpenVPN`版本信息
$ openvpn --version
...
```
> 代码清单：检验`OpenVPN`安装

# `OpenVPN`客户端连接远程VPN网络

安装完成后，我们就可以使用`OpenVPN`来连接远程 VPN 网络了。

一般来说，企业 VPN 服务端的运维人员会给需要登录到远程网络的客户端一份`*.ovpn`配置文件。该配置文件记录了用户登录该 VPN 所需的必要信息（认证方式，加密证书，使用协议...）。拿到这份配置文件，我们就可以使用`OpenVPN`登录远程 VPN 了。

```bash
# 连接 VPN 需要使用`sudo`管理员权限（属于系统操作）
$ sudo openvpn --config <path/to/your/*.ovpn>
...
```
> 代码清单：使用`OpenVPN`连接远程网络

# 参考资料

- 编译 OpenVPN 及解决相关依赖问题：https://segmentfault.com/a/1190000009414135

[openvpn]: ./openvpn.png

