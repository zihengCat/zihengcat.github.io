---
title: Vim学习笔记-执行shell命令
date: 2017-07-26
tags: Vim
---

# 前言

本文介绍在Vim下执行shell命令的几种方式。

# Vim 执行 shell 命令

在终端 *Terminal* 下使用 vim，我们偶尔会有执行shell命令的需求：
- 查看目录文件
- 查看当前IP地址
- 执行shell脚本
- ...

这在vim中是非常容易办到的。

## `Ctrl + z`

传统方式: 使用shell的任务管理功能 *job control*，`Ctrl + z`将当前 vim 进程挂起，返回shell命令行。执行完相应操作后，再使用`fg`将 vim 拉回。

## `:!{cmd}`

如果我们只想执行一条shell命令，可以直接在vim命令行模式下键入`!{cmd}`，感叹号`!`后接需要执行的shell命令。命令执行完成后，按任意键返回vim。

## `:sh[ell]`
还有一种方式，我们可以在vim下启动一支shell子进程，在子进程完成相应操作：`:sh[ell]`。

> 参考:
> :help shell

