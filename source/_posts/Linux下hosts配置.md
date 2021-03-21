---
title: Linux 下 hosts 配置
date: 2017-08-01
tags: ["Linux"]
---

# 前言

本文介绍 Linux 下`hosts`文件的相关配置。

# hosts 文件

hosts: *The static table lookup for host name（主机名查询静态表）*

hosts文件(域名解析文件)是一个用于储存计算机网络中各节点信息的计算机文件。这个文件负责将主机名称映射到相应的IP地址。hosts文件通常用于补充或取代网络中DNS的功能。和DNS不同的是，计算机的用户可以直接对hosts文件进行控制。**hosts文件请求优先于DNS解析。**
在Linux系统下，hosts文件以ASCII格式保存在`/etc/hosts`。

# hosts 文件格式

一般情况下，hosts文件以行为单位作解析。每行由三部份组成，每个部份以空白符隔开。其中`#`开头的行是注释，不会被系统解释。

- 第一部分：网络IP地址
- 第二部分：主机名/域名
- 第三部分：主机别名(可省略)

**hosts文件的格式**
```
# 注释
IP地址    主机名/域名    别名(可省略)
```
> 域名与主机名无需过于区分，
> - 在局域网中称主机名。
> - 在互联网中称域名。

# 规避 DNS 污染

由于`hosts`文件请求优先于`DNS`解析，我们可以修改本地`hosts`文件来规避部分`DNS`污染。

> `hosts`开源项目: https://github.com/racaljk/hosts

有好心的朋友整理了一些可用的IP地址，放置在 GitHub 上，我们可以使用。

将`hosts`文件下载下来，替换掉`/etc/hosts`文件即可。

```bash
#!/bin/bash
# Update hosts file from GitHub
function update_hosts() {
    HOSTS_LINK='https://raw.githubusercontent.com/racaljk/hosts/master/hosts'
    # mirror link
    # HOSTS_LINK="https://coding.net/u/scaffrey/p/hosts/git/raw/master/hosts"
    HOSTS_TMP='hosts.tmp'
    HOSTS_SRC='/etc/hosts'

    echo 'Downloading latest hosts file...'
    wget ${HOSTS_LINK} -O ${HOSTS_TMP}

    echo 'Using sudo to replace file...'
    sudo cp -f ${HOSTS_TMP} ${HOSTS_SRC}

    if test ${?} -eq 0
    then
        head -n 3 ${HOSTS_SRC}
        rm ${HOSTS_TMP}
        echo 'Update hosts Successful!'
        sudo systemctl restart network.service
    else
        echo 'Something Error...'
    fi
}
update_host
```
> 代码清单：更新`hosts`文件

这样配置下来，Google 是可以正常使用的，YouTube 还是看不了的。

