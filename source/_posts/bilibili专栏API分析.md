---
title: Bilibili专栏API分析
date: 2018-04-19
tags: Project
---

# 前言

本文对B站专栏API做简要分析。

# 专栏表单数据结构

| 键           | 值       | 说明       |
|:-------------|:---------|:-----------|
| `title`      | `string` | 文章标题   |
| `banner_url` | `url`    | 文章头图   |
| `content`    | `html`   | 文章内容   |
| `summary`    | `string` | 文章小结（前`100`字符） |
| `words`      | `int` | 文章总字符数（不含空白字符） |
| `category`   | `int` | 文章类别   |
| `list_id`    | `int` | 轻小说类别下的轻小说文集（其他类别下为`0`） |
| `tid`        | `int` | **不明**       |
| `reprint`    | `int` | 文章转载权限（`0`不允许转载，`1`允许规范转载） |
| `tags`       | `string` | 文章标签（分隔符`,`）      |
| `image_urls`            | `URL`     | 专栏封面图     |
| `origin_image_urls`     | `URL`     | 专栏封面原始图 |
| `dynamic_intro`         | `string`  | 专栏推荐语     |
| `aid`        | `int`    | 文章草稿编号  |
| `csrf`       | `string` | 跨域访问      |

> 表: B站专栏表单结构

# 富文本编辑器转换规则

| 富文本选项 | 生成HTML    | 说明           |
|:----------:|:-----------:|:--------------:|
| 标题    | `<h1>...</h1>` | 仅支持一级标题 |
| 段落    | `<p>...<p>`    | 段落标记       |
| 粗体    | `<strong>...</strong>` | 粗体文本 |
| 删除线  | `<span style="text-decoration: line-through;">...</span>` | 删除线文本 |
| 字号    | `<span class="font-size-x">...</span>` | 小号12, 标准16, 大号20, 特大23 |
| 颜色    | `<span class="color-xx-x"` | 各种文字颜色 |
| 引用    | `<blockquote><p>...</p></blockquote>` | 引用文本（可嵌套其他标签） |
| 分割线  | `<figure><img src=""></figure>` | [图片]分割线 |
| 对齐    | `<p style="text-align: x;">...</p>` | 左lelf, 中center, 右right |
| 有序列表| `<ol><li>...</li></ol>` | 有序 |
| 无序列表| `<ul><li>...</li></ul>` | 无序 |
| 链接    | `<a>...</a>`        | 仅支持站内链接 |
| 图片    | `<figure><img src=""></figure>` | 插入本地图片 |
| 站内卡片| `<figure></figure>` | 各种站内卡片 |

> 表: B站专栏富文本编辑器转换规则

