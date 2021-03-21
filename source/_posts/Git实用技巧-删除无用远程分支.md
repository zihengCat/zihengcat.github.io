---
title: Git 实用技巧 - 删除无用远程分支
catalog: true
date: 2018-07-06
tags: ["Git"]
---

# 前言

使用`Git`工作流「Git Workflow」时，我们有时会遇到这样的情况：远程分支已被删除，但在本地`Git`仓库中仍然保留着指向改远程分支的引用「References」，我们需要手动清除这些仍留在本地的无用远程分支。

# `Git`删除无用远程分支

实际上，`Git`的开发者们也早已经考虑到了这样的使用场景，并提供了一枚方便的命令来帮助我们做这件事。

```bash
$ git remote prune <name>
```
> 代码清单：使用`prune`删除无用远程分支

加入`-n, --dry-run`选项，只打印无用远程分支而不真正执行删除操作。

使用`git remote prune`命令，即可方便地删除无用远程分支，使得本地与远程分支状态保持一致。

# 参考资料

- `Git`官方文档：https://git-scm.com/docs/git-remote#git-remote-empruneem

