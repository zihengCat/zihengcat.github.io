---
title: 为CentOS7添加EPEL源
date: 2017-06-12
tags: [Linux, Tips]
---

# 前言
> EPEL (Extra Packages for Enterprise Linux) 是Fedora小组维护的一个软件仓库项目，为RHEL/CentOS提供他们默认不提供的软件包。这个源兼容RHEL以及像CentOS和Scientific Linux这样的衍生版本。

EPEL源包含了许多高质量的软件，大部分软件的依赖包也可以在EPEL里找到。有了它，我们不用再为软件/依赖包缺失而苦恼啦。

# 安装

## 命令安装

EPEL源的安装非常简单，只需一条`yum`命令就可以安装啦。
这个包在CentOS Extras repository里面，默认是启用的。

```
$ sudo yum install epel-release
```

## 手动安装

当然，我们也可以手动安装:

### Step 1 检查redhat版本

```
$ cat /etc/redhat-release
```
`uname`也可以，只要知道当前系统架构(i386/x86\_64)与系统版本(CentOS-5/6/7)就可以啦。

### Step 2 安装EPEL包

前往[EPEL](http://fedoraproject.org/wiki/EPEL)官方网站，获取最新版本的rpm包。

- [EL7 version](https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm)
- [EL6 version](https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm)
- [EL5 version](https://dl.fedoraproject.org/pub/epel/epel-release-latest-5.noarch.rpm)

下载完rpm包后，安装上就可以啦。

```
$ sudo rpm -ivh <epel-release-version.rpm>
```

### Step 3 检验EPEL源

执行以下命令查看`yum`仓库: 

```
$ yum clean all && yum repolist
```

```
Loaded plugins: fastestmirror
Cleaning repos: base epel extras updates
Cleaning up everything
Cleaning up list of fastest mirrors
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirrors.163.com
 * epel: mirrors.ustc.edu.cn
 * extras: mirrors.163.com
 * updates: mirrors.njupt.edu.cn
repo id                       repo name                                                      status
base/7/x86_64                 CentOS-7 - Base                                                 9,363
epel/x86_64                   Extra Packages for Enterprise Linux 7 - x86_64                 11,787
extras/7/x86_64               CentOS-7 - Extras                                                 380
updates/7/x86_64              CentOS-7 - Updates                                              1,851
repolist: 23,381
```

可以看到，EPEL源已经被添加上了，仓库里多了好多的软件呐!

# 手动安装?

子恒喵是直接将rpm包里的`epel.repo`拿出来放到`/etc/yum.repos.d/`里面去，当成一个第三方源来用了...
这样就不需要多装一个rpm包了...(似乎有点强迫症呐Orz...)

