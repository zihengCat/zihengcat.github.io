---
title: Hexo博客添加highlight.js代码高亮
date: 2018-03-05
tags: [Hexo, Web, JavaScript]
---

# 前言

技术类个人博客总是绕不开一个重要的需求：漂亮显示代码。美观的代码显示能为博客文章添彩不少。
本文介绍在如何在Hexo博客中利用`highlight.js`库实现美观的代码高亮显示效果。

# `highlight.js`简介

`highlight.js`是一枚 JavaScript 写成的代码高亮库。

- 支持176门语言与79种样式
- 自动语言侦测
- 支持多语言代码高亮
- Node.js可用
- 支持自定义标签
- 方便与其他JS框架结合使用
- 本身不依赖于任何框架

> highlight.js 库的优点

我们可以在[highlight.js](https://highlightjs.org)官方网站上浏览其支持的高亮样式。

# 为Hexo博客添加代码高亮

想要为Hexo博客添加`highlight.js`，我们首先需要**关闭Hexo自带的代码高亮插件**。编辑`_config.yml`配置文件，关闭掉自带highlight功能。

```
...
highlight:
  enable: false
...
```

> 取消Hexo自带代码高亮

接下来，手动引入`highlight.js`库。代码添加位置根据具体使用的Hexo主题定，子恒喵这里是选择直接添加到`footer.ejs`中。

```
<% if (config.highlightjs) { %>
<!-- Highlight.js -->
<link rel="stylesheet"
      href="//cdn.bootcss.com/highlight.js/9.2.0/styles/github.min.css">
<script src="//cdn.bootcss.com/highlight.js/9.2.0/highlight.min.js">
</script>
<script>
    hljs.initHighlightingOnLoad();
</script>
<% } %>
```
先引入JS库文件与CSS样式文件，接着初始化脚本即可。子恒喵这里额外添加了`ejs`控制代码，方便在配置文件中手动启停插件。

# 参考资料

- 官方网站: https://highlightjs.org
- 项目地址: https://github.com/isagalaev/highlight.js

