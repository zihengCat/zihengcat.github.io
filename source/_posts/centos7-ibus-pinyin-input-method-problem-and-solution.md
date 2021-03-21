---
title: CentOS 7 IBus 拼音输入法问题及解决
subtitle: CentOS 7 IBus Intelligent Pinyin Input Method Problem and Solution
catalog: true
date: 2018-10-20
tags: [Linux, Tips]
---

# 前言

个人在使用 CentOS 7 时，遇到了系统自带的智能拼音输入法（Intelligent Pinyin Input Method）无法使用的问题。具体表现为：**系统显示已经切换到了智能拼音输入法，但无法使用拼音输入，输入的仍是英文。**

# 问题分析与解决

排除拼音输入法的设置问题，问题原因定位为：**未正确设置 GTK Input Method 。**

问题解决方法：**命令行下执行`im-chooser`，在弹出的对话框中设置使用 IBus 输入方式，重新登陆图形界面，即可正常使用 IBus 智能拼音输入法。**

![setting-gtk-input-method](./setting-gtk-input-method.png)

> 图：设置 GTK Input Method

