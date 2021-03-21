---
title: 关于 SSH 公钥检查
subtitle: Something about SSH StrictHostKeyChecking
catalog: true
date: 2018-06-21
tags: ["Linux"]
---

# 前言

SSH 公钥检查（StrictHostKeyChecking）是 SSH 的一个重要的安全机制，可以有效防范中间人劫持等黑客攻击。但是在某些特定场景下，严格 SSH 公钥检查会破坏一些依赖 SSH 协议的自动化任务（CI Tasks），我们需要一种手段能够绕过 SSH 公钥检查。

# 关于`SSH`公钥检查（StrictHostKeyChecking）

参看`ssh_config`说明文档，我们可以了解到，SSH 的公钥检查（StrictHostKeyChecking）有三种模式。

```plain
$ man ssh_config
...
StrictHostKeyChecking

    If this flag is set to yes, ssh(1) will never automatically add host keys to
    the ~/.ssh/known_hosts file, and refuses to connect to hosts whose host key has
    changed.  This provides maximum protection against trojan horse attacks, though
    it can be annoying when the /etc/ssh/ssh_known_hosts file is poorly maintained
    or when connections to new hosts are frequently made.  This option forces the
    user to manually add all new hosts.  If this flag is set to no, ssh will auto‐
    matically add new host keys to the user known hosts files.  If this flag is set
    to ask (the default), new host keys will be added to the user known host files
    only after the user has confirmed that is what they really want to do, and ssh
    will refuse to connect to hosts whose host key has changed.  The host keys of
    known hosts will be verified automatically in all cases.

...
```
> 代码清单：关于`SSH`公钥检查（StrictHostKeyChecking）说明

当`StrictHostKeyChecking=yes`时，SSH 执行严格公钥检查模式，拒绝连接没有记录在`/etc/ssh/ssh_known_hosts`列表中的远程主机。

当`StrictHostKeyChecking=ask`时，SSH 执行默认公钥检查模式，当用户连接远程主机时，会检查主机的公钥。如果用户是第一次连接该主机，会显示出该远程主机的公钥摘要，并提示用户是否信任该远程主机。选择接受，则将该远程主机的公钥追加到文件`~/.ssh/known_hosts`中，用户下次再连接时，就不会出现提示信息了。

当`StrictHostKeyChecking=no`时，SSH 执行宽松公钥检查模式，自动将连接的远程主机的公钥追加到文件`~/.ssh/known_hosts`中，不出现提示信息提示用户信任该远程主机。

# 禁用`SSH`公钥检查（StrictHostKeyChecking）

确认中间人劫持攻击风险较小的情况下（如：内网环境），我们可以禁用 SSH 公钥检查。这是基于 SSH 协议的自动化任务常用的手段。

SSH 客户端提供一个`UserKnownHostsFile`配置项，允许用户指定`known_hosts`文件，利用这一配置项，就不会出现公钥冲突导致连接中断的情况了。

```bash
$ ssh -o StrictHostKeyChecking=no \
      -o UserKnownHostsFile=/dev/null \
      -p <PORT> \
      <USER>@<HOST>
...
```
> 代码清单：禁用`SSH`公钥检查（StrictHostKeyChecking）

# 参考资料

- `ssh_config`说明文档：`man ssh_config`

