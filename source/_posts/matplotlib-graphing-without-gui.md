---
title: 无GUI环境下的Python Matplotlib绘图
subtitle: Matplotlib Graphing without GUI
date: 2018-05-15
catalog: true
tags: [Linux, Python, Matplotlib]
---

# 前言

`Matplotlib`绘图库默认使用`X11`的用户图形界面（GUI）展示绘制的图形，对于不带GUI的服务器系统，想要使用`Matplotlib`绘图需要做一些额外配置。

# 服务器端「无GUI」使用`Matplotlib`绘图

想让`Matplotlib`在无图形界面（GUI）的环境下绘图，我们需要先导入`maptlotlib`包，再为其指定使用的后端（Backend）绘图引擎。最简单的情况，就是设置使用`Agg`作为后端绘图引擎，`Agg`的含义是「**C++ Antigrain**」绘图引擎，它不仅能够渲染出高质量的PNG图片，还支持导出其他格式「PDF, PS, EPS, SVG」图片。

配置`Matplotlib`使用`Agg`后端引擎非常简单，导入`maptlotlib`库，使用`use`方法配置即可。注意，配置后端这一动作要在导入`pylab`或`pyplot`之前完成。

```python
# do this before importing pylab or pyplot
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
# ...
```
> 代码清单：`Matplotlib`配置`Agg`后端引擎

# 参考资料

- `Matplotlib`官方文档: https://matplotlib.org/faq/howto_faq.html#matplotlib-in-a-web-application-server

