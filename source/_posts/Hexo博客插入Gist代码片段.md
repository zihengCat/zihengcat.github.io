---
title: Hexo博客插入Gist代码片段
date: 2017-08-20
tags: Hexo
---

# 前言

程序员们的博客一定会有插入代码片段的需求。
Markdown支持代码段，但是还有更好的方案：**GitHub Gist**。

# Gist 测试

测试Hexo博客插入Gist代码片段。

<!-- Get UTF-8 Size (ANSI C) -->
<!-- Begin -->
<script src="https://gist.github.com/zihengcat/1a80a31b671e5bb3db6eb7af5a4b30f7.js"></script>
<!-- End -->

# 实现

在博客中插入Gist代码片段非常简单。

1. 前往[GitHub Gist](https://gist.github.com/)创建所需的 Gist 代码片段。
2. 将 Gist Embed Script 插入到博客中。

Markdown格式是支持**内联HTML代码**的，我们可以方便的在文章中插入Gist Embed Script。
比如测试用例，直接在Markdown正文里写：
```
...

<!-- Get UTF-8 Size (ANSI C) -->
<!-- Begin -->
<script src="https://gist.github.com/zihengcat/1a80a31b671e5bb3db6eb7af5a4b30f7.js"></script>
<!-- End -->

...
```
很好很强大，给 GitHub 点赞！

