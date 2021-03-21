---
title: Macintosh macOS 重置网络
subtitle: Macintosh macOS reset Network
date: 2017-07-30
catalog: true
tags: [Macintosh, Tips]
---

# 前言

最近，子恒喵苹果笔记本电脑的`Wi-Fi`网络似乎出现了点问题。
有时候，显示的是`Wi-Fi` 网络正常连接，但是网络就是卡住连不上，网络时断时续的感觉...
在**重置 Macintosh macOS 网络配置**之后，这个问题就得以解决。

# Macintosh macOS 重置网络

重置`macOS`的网络非常简单粗暴，进入存放网络配置文件的目录，清空**系统网络配置文件**即可。

> `macOS`系统网络配置文件路径：`/Library/Preferences/SystemConfiguration/`

```bash
$ sudo rm -rf /Library/Preferences/SystemConfiguration/
$ sudo reboot
```
> 代码清单：清空`macOS`系统网络配置文件

清空配置文件后，重启一遍系统，`macOS`系统会自动初始化网络配置文件。之后再去**系统偏好设置「System Preferences」**里把网络重新配置一遍，就没有太大问题了。

