---
title: Git 实用技巧 - 分支重命名
subtitle: Git Practical Tips - Rename Branch
catalog: true
date: 2019-12-18
tags: ["Git"]
---

# 前言

在 Git 工作流程中，重命名分支是一项比较常用的操作。如果我们建立分支时起了个不恰当的名字，还将该分支推送到了远端仓库，这时候重命名分支就非常有必要了。本文讲解如何快捷重命名 Git 分支。

# Git 分支重命名

对**当前**本地分支重命名。

```bash
$ git branch -m <new_name>
```
> 代码清单：Git 重命名当前本地分支

对**任意**本地分支重命名。

```bash
$ git branch -m <old_name> <new_name>
```
> 代码清单：Git 重命名本地分支

对于已经推送到远端仓库的分支，可以执行如下 *快捷命令* 进行重命名，命令逻辑等同于：先删除远端分支，再推送本地分支至远端仓库。

```bash
$ git push origin :<old_name> <new_name>
```
> 代码清单：Git 重命名远端分支（注意`:`号）

# 参考资料

- Git 官方文档：https://git-scm.com/docs/git-branch

<!-- EOF -->
