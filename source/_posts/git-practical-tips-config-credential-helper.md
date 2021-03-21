---
title: Git 实用技巧 - 配置 credential.helper
subtitle: Git Practical Tips - Config Credential Helper
date: 2017-08-26
tags: ["Git"]
---

# 前言

Git 分布式版本管理系统使用的数据交换协议有：`SSH`与`HTTP`。

- 使用`SSH`协议：用户需要先在本地配置`SSH Key`，配置完成后，与服务器同步代码无需输入用户名和密码。

- 使用`HTTP`协议：用户不需要配置，但在每次代码同步时，需要手动输入用户名和密码，操作较为繁琐。

实际上，我们可以使用 Git Credential Helper 来简化`HTTP`协议下用户名和密码的输入过程。

# GitHub 免用户名密码同步

我们以最常用的本地与 GitHub 代码同步为例：配置 GitHub 免用户名密码同步。

## 创建 Git 存储凭证

我们在**用户家目录**下创建一个名为`.git-credentials`的隐藏文件，将用户 GitHub 用户名与密码按**指定格式**写入该文件。

```bash
$ touch ~/.git-credentials
$ echo -n 'https://<user>:<pass>@github.com' >> ~/.git-credentials
```
> 代码清单：配置`.git-credentials`

## 配置 Git Credential Helper

接下来，我们在`.gitconfig`配置文件中加入`credential.helper`配置选项。

```bash
$ git config --global credential.helper store
```
> 代码清单：配置`.gitconfig`

命令执行完成后，用户家目录下的配置文件`.gitconfig`中会多出一枚配置项`[credential]`。以后，我们使用`HTTP`与 GitHub 进行代码同步时，就不需要手动输入用户名与密码了，非常方便。

## 注意事项

需要注意的是，`.git-credentials`凭证文件中的用户名密码以**明文存储**，有一定安全隐患。关于 Git Credential 的更多信息，可以浏览参考资料。

# 参考资料

- 官方文档 Git Credentials：https://git-scm.com/docs/gitcredentials

- Git Tools - Credential Storage：https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage

<!-- EOF -->
