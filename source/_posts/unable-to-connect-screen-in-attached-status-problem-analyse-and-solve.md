---
title: Linux 下 Screen 状态 Attached 无法连接 - 问题分析及解决
subtitle: Unable to Connect Screen in Attached Status - Problem Analyse and Solve
catalog: true
date: 2018-11-23
tags: ["Linux", "Screen", "Tips"]
---

# 前言

正常情况下，我们使用`Ctrl + D`分离 Screen 会话（Session），使用`screen -r <session_id>`恢复一枚 Screen 会话。

但是，当一枚 Screen Session 状态为`Attached`时，我们不能简单的通过`-r`选项恢复会话。

# 问题分析

出现这一问题的原因是：**上一用户仍留存在 Screen Session 中，未正常退出。**

# 问题解决

使用`screen -D -r <session_id>`命令即可正常恢复会话。

> 注：加入`-D`选项，表示强制踢出上一用户。

# 参考资料

- Screen Manual: `man screen`

