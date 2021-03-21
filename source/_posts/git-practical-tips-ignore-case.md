---
title: Git 实用技巧 - 区分文件名大小写
subtitle: Git Practical Tips - Ignore Case
date: 2018-06-07
catalog: true
tags: ["Git"]
---

# 前言

`Git`默认配置**对于文件名大小写不敏感，即不区分文件名大小写**。这意味着，如果我们只修改了文件名大小写（大写字母改小写或小写字母改大写），`Git`并不会侦测到任何改动。这一默认配置有时候会对项目开发造成影响。

Windows 系统默认不区分大小写，macOS 默认是**Mac OS 扩展（日志式）**磁盘格式，也不区分大小写，而 Linux 则是区分大小写的，或许是为了系统兼容性考虑，`Git`选择默认不区分文件名大小写。

# `Git`中启用大小写敏感

在一枚`git`仓库中，我们可以使用如下命令启动大小写敏感模式。

```bash
$ git config core.ignorecase false
```
> 代码清单：`git`启用大小写敏感模式

也可以加入`--global`参数将配置项写入`.gitconfig`全局配置文件中。

```plain
...
[core]
        ignorecase = false
...
```
> 代码清单：`.gitconfig`大小写敏感模式

在默认配置「文件名大小写不敏感」的情况下，使用`git mv`命令来修改文件名，也可以让`git`侦测到文件名称的变动。

```bash
$ git config core.ignorecase true
$ git mv <old_name> <new_name>
```
> 代码清单：使用`git mv`修改文件名

# 参考资料

- 知乎问答：https://www.zhihu.com/question/57779034

