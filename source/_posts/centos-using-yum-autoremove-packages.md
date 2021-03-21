---
title: CentOS 系统使用 yum 干净卸载软件包
subtitle: CentOS Using yum autoremove Packages
date: 2017-07-25
catalog: true
tags: [Linux, Tips]
---

# 前言

本文记录在*CentOS Linux*系统下如何使用`yum`命令**干净**卸载软件包（即: 连同该软件包的相关依赖包一并移除）。

# 使用`yum`干净移除软件包

对于大部分使用红帽系（RedHat）Linux系统的用户而言，我们习惯使用`yum`命令安装或卸载软件包。当我们使用`yum install`命令安装一枚软件包时，`yum`会将该软件包连同其所有依赖包一并安装到本机。但当我们使用`yum remove`命令卸载一枚已安装软件包时，`yum`默认只会移除你所指定的那枚软件包，并不会移除该包的相关依赖包。

自从*Fedora 18*之后，我们便可以使用`yum autoremove`命令来干净卸载软件包了。

```bash
$ sudo yum autoremove <package>
```
> 代码清单：使用`yum`干净卸载软件包

使用该命令卸载软件，`yum`便会在删除软件包的同时将系统中该软件包不需要的依赖项全部移除，非常的智能与方便。

# 参考资料

- StackOverflow: https://unix.stackexchange.com/questions/40179/remove-unused-packages

