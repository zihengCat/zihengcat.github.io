---
title: macOS 小技巧 - 提高按键连续输入速率
subtitle: Increase Keyboard Repeat Rate on macOS
catalog: true
date: 2020-07-10
tags: ["Macintosh", "macOS", "Tips"]
---

# 前言

使用苹果计算机（Macintosh）进行日常开发的程序员们可能会感觉到 macOS 系统的***长按输入响应***与***光标移动速度***有些迟缓，即便是修改了`System Preferences >> Keyboard`中关于按键输入的系统配置项，也还是不尽如人意。

实际上，我们还有更巧妙的方法可以提高 macOS 按键连续输入速率。

![keyboard-repeat-preference][keyboard-repeat-preference]

> 图：`System Preferences >> Keyboard`系统配置项

# macOS 设置提高按键连续输入速率

打开命令行终端 Terminal，键入如下命令，重启/重新登陆。

```bash
$ defaults write -g KeyRepeat -int 1
$ defaults write -g InitialKeyRepeat -int 10
```
> 代码清单：macOS 设置提高按键连续输入速率

原理：**通过系统 UI 与命令行 CLI 修改的是同一份系统配置，通过命令行强行写入比系统默认值更低的按键响应参数。**

- `KeyRepeat`：**字符连续输入间隔**，此处为`1`（15ms），系统默认最低值`2`（30ms）。

- `InitialKeyRepeat`：**响应连续输入的等待时间**，此处为`10`（150ms），系统默认最低值`15`（225ms）。

当然，每个人都有适合自己的按键输入速率，可以根据自身实际情况调整参数。按键响应速度快，写起代码来更加得心应手。

# 参考资料

- StackExchange: https://apple.stackexchange.com/questions/10467/how-to-increase-keyboard-key-repeat-rate-on-os-x

<!-- Begin -->

[keyboard-repeat-preference]: ./keyboard-repeat-preference.png

<!-- End -->

<!-- EOF -->
