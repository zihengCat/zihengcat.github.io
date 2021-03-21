---
title: Linux 操作系统配置 SSH 免密码公钥登陆
subtitle: Use SSH Public Key Authentication in Linux
catalog: true
date: 2017-08-18
tags: ["Linux", "SSH"]
---

# 前言

使用 GNU/Linux 系统的小伙伴们一定知道大名鼎鼎的 SSH（Secure Shell）远程登陆服务。

使用 SSH 客户端远程登陆到远端服务主机时，服务端提示输入密码进行认证，通常服务器密码一般设置得较为复杂，有的服务器配置禁用密码登录。此时，我们可以使用使用密钥认证方式进行 SSH 远程登陆。

# 配置 SSH 密钥认证登陆

首先，我们需要确认远程主机的`sshd`服务已经开启了密钥认证功能。

查看`sshd`配置文件，确认相关功能选项均已开启。SSH 一般默认开启秘钥登陆，但还是确认一下比较稳妥。如未开启，修改配置文件并重启服务。

```bash
$ sudo vi /etc/ssh/sshd_config
...
SAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
...
$ sudo systemctl restart sshd
```
> 代码清单：开启 SSH 密钥认证登陆

接下来，我们需要生成一对密钥，在**客户端（Client）**使用`ssh-keygen`命令来完成。

```bash
$ ssh-keygen
...
```
> 代码清单：生成密钥

该命令提示3条语句，一般情况下，我们回车跳过即可（更多选项可参看命令`man`文档）。

1. 密钥对存放位置
2. 输入控制密码
3. 再输入（确认）控制密码

该命令生成了一对 RSA 密钥，默认存放在用户家目录`~/.ssh`下`id_rsa`、`id_rsa.pub`。带`.pub`后缀的是公钥，不带后缀的是私钥。

然后，我们把公钥上传到**服务端（Server）**远端主机上完成后续配置。
```
$ ssh-copy-id <user>@<hostname>
```
我们既可以使用`ssh-copy-id`命令自动完成配置，也可以手动操作。
```
$ scp -P <port> ~/.ssh/id_rsa.pub <user>@<hostname>:~
$ ssh <user>@<hostname>
$ cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
$ rm ~/id_rsa.pub
```
其实这一步就是将`id_rsa.pub`公钥写入到`authorized_keys`中。如果没有该文件，就创建一个。再将`authorized_keys`与`~/.ssh`权限设置正确。**权限设置不对，认证就会失败。**

- `.ssh`目录权限：`0700`
- `id_rsa`秘钥权限：`0600`

配置完成后，我们就可以从客户端使用秘钥认证登陆远程目标主机了。

# 工作原理

SSH 免密码登陆是使用密钥加解密认证取代原先了的密码认证。

![ssh-key-auth-flow](./ssh-key-auth-flow.png)

> 图：SSH 密钥认证流程图（取自：DigitalOcean）

首先初始化连接，服务端发送一段随机字符串给客户端，客户端使用私钥加密后发回，客户端再使用公钥解密，如果解密后的内容可以正确配对，则认证通过，允许远程登陆。

关于加解密的过程，使用的是非对称加密算法，个人也没学过密码学，不是很懂...但是，SSH远程登陆不用输密码，还是很方便的。

# 参考资料

- RedHat 官方教程：https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-ssh-configuration-keypairs.html

- Debian 官方教程：https://debian-administration.org/article/530/SSH_with_authentication_key_instead_of_password

- ServerPilot：https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication

<!-- EOF -->