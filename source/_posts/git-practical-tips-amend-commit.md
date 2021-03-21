---
title: Git 实用技巧 - 覆写上次提交
subtitle: Git Practical Tips - Amend Commit
date: 2017-08-16
tags: ["Git"]
---

# 前言

在日常使用 Git 版本控制工具的时候，我们有时会遇到需要修改上次`commit`提交信息的情况，例如：修改上次提交信息中的错误内容，或者想为上次提交加入些新内容等等。

# Git 覆写上次提交（amend）

我们可以使用`git commit`命令的`amend`选项来覆写上次提交。

```bash
$ git commit --amend
```
> 代码清单：使用`--amend`选项覆写上次提交

执行此命令后，`git`会自动弹出编辑界面，让你可以修改上次的**提交信息**。

```bash
$ git commit --amend -m "an updated commit message"
```
> 代码清单：使用`-m`选项快速修改

而使用`--no-edit`选项，表示不更动上次已经提交的信息。适用于添加新内容到上次提交的情况。

```bash
$ git commit --amend --no-edit
```
> 代码清单：使用`--no-edit`选项追加内容

而使用`--author`选项，则可以仅修改提交者信息。

```bash
$ git commit --amend --author='someone <someone@example.com>'
```
> 代码清单：使用`--author`选项修改提交者

# 注意事项

```plain
--amend

    Replace the tip of the current branch by creating a new
    commit. The recorded tree is prepared as usual (including
    the effect of the -i and -o options and explicit pathspec),
    and the message from the original commit is used as the
    starting point, instead of an empty message, when no other
    message is specified from the command line via options such
    as -m, -F, -c, etc. The new commit has the same parents and
    author as the current one (the --reset-author option can
    countermand this).

    It is a rough equivalent for:

                $ git reset --soft HEAD^
                $ ... do something else to come up with the right tree ...
                $ git commit -c ORIG_HEAD

    but can be used to amend a merge commit.

    You should understand the implications of rewriting history
    if you amend a commit that has already been published. (See
    the "RECOVERING FROM UPSTREAM REBASE" section in git-
    rebase(1).)

```
> 注：关于`--amend`的说明文档

需要注意的是：`amend`选项不会**直接修改**上次的提交，而是会**新创建**出一枚提交，包含了上次提交的全部内容与新添加的内容，原先的提交会被“删除”。可以观察到，修改前后的`commit`的`hash`值是不同的。

所以，**对于已经推送到远程仓库的提交**，需谨慎使用`amend`。

```bash
$ git push <remote> <branch> --force
```
> 代码清单：`git`强制覆盖远程分支

对于已经推送到远程仓库的提交，可以使用**强制推送**来覆盖（谨慎使用）。

![amend-pic](./amend-pic.svg)

> 图：`git commit --amend` 原理示意图「取自Atlassian」

![amend-caution](./amend-caution.jpg)

> 图：`git commit --amend` 注意事项图「取自Pikicast」

# 参考资料

- Atlassian Tutorials：https://www.atlassian.com/git/tutorials/rewriting-history

- 重写项目历史：https://github.com/geeeeeeeeek/git-recipes/wiki/2.7-%E9%87%8D%E5%86%99%E9%A1%B9%E7%9B%AE%E5%8E%86%E5%8F%B2

<!-- EOF -->
