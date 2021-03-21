---
title: 使用 GitHub Pages + Hexo 搭建个人小站
subtitle: Building Your Personal Blog Using GitHub Pages and Hexo
catalog: true
date: 2017-05-14
tags: ["GitHub", "Node.js", "Hexo"]
---

# 前言

此篇文章将指导你如何使用`GitHub Pages`与`Hexo`搭建个人独立博客。

![Blog-Hexo](./blog-hexo.png)

## 为什么选择 GitHub Pages + Hexo

如今，自媒体行业繁荣，能写东西的地方非常之多（例如：今日头条、简书、知乎专栏...），这些大平台往往可以提供足够的流量与曝光率，那么我们要选择 GitHub Pages + Hexo 手动搭建个人博客写作平台呢？

- 数据可控

- 支持 Markdown 书写，可扩展性强

- 适用 Git WorkFlow, 与 GitHub 完美契合

- Geek 精神

> 注：以上几点总结只是个人的一些浅显理解。

# 环境搭建

首先，我们需要在本机搭建起 GitHub Pages 与 Hexo 的运行环境，任何软件都需要与其相对应的运行环境。

## `Git`环境搭建

[**Git官网**](https://git-scm.com/)

前往`Git`官方网站, 根据你使用的计算机架构，选择系统下载安装`Git`，按照提示即可。

> 注：如果`Git`官网无法访问，建议使用代理工具尝试。

希望大家养成下载软件去官网的好习惯。

子恒喵用的是CentOS Linux系统，一条`yum`命令就装好啦。

## `Node.js`环境搭建

[**Node.js官网**](https://nodejs.org/)

前往Node.js官网, 根据计算机架构，选择合适的版本下载安装Node.js。
可以直接下载预编译的二进制包，也可以下载源代码手动编译安装。
子恒喵是手动编译安装的。

## 检验安装

添加安装路径到`PATH`环境变量中，打开命令行工具（Windows下`CMD`），执行命令。

```
$ git --version
$ node --version
```
如果正常打印出软件版本号，说明你的安装是成功的！

# Hexo 安装

> A fast, simple & powerful blog framework, powered by Node.js.

Hexo 是一个快速、简洁且高效的博客框架，使用`Node.js`写成，支持Markdown格式。

Hexo的安装非常简单。使用`npm`命令安装即可。
```
$ npm install hexo-cli -g
```
> 使用NPM（Node.js默认包管理器）安装上Hexo, 安装的是命令行版本的Hexo。
> `-g` 参数表示全局安装。

```
$ hexo --version
```
安装完成后，执行以上命令，如果正确打印出Hexo及其相关软件的版本号，则说明安装成功。

# Hexo 使用

安装完成之后，我们终于可以上手用用这个Hexo啦。
执行以下命令Hexo将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

```
执行以下命令启动一个Web服务器。
```
$ hexo server
```

> 默认情况下，访问网址为：`http://localhost:4000/`。
> 端口号可手动指定。

打开浏览器，键入`http://localhost:4000/`，就可以看到我们的博客了。

# Hexo 更多

更多有关Hexo的信息，可以去查阅官方文档。

[**Hexo Docs**](https://hexo.io/docs/)
[**Hexo 中文文档**](https://hexo.io/zh-cn/docs/)

# GitHub Pages

> Websites for you and your projects.

GitHub Pages 是GitHub 为广大开发者们提供的一个`静态站点托管服务`。

- GitHub 提供 1GB 仓库容量, 超过会有限制
- 每月 100GB 的带宽流量和10万次请求
- 开发者项目与站点紧密相关

注意，只托管静态网站哦（不支持动态性）。
如果你的博客配有大量高清图片, 推荐使用CDN。

[**GitHub Pages 官方教程**](https://pages.github.com/)

官方教程写得简洁明了，熟悉Git的你一定能轻松上手。

# 发布网站

现在，我们已经在GitHub上建立了自己的GitHub Pages：`yourname.github.io`。
也在本地跑起来了我们的Hexo博客小站：`http://localhost:4000`。
最后一步，我们要把本地的站点托管到GitHub Pages上。

由于GitHub Pages不支持动态网站，我们需要生成**静态网页文件**再托管到GitHub上。Hexo可以轻松完成这项任务。

```
$ hexo generate
```

以上命令生成了静态网页文件，放置在`public`目录下。

```
$ git remote add origin <your-github-pages-repo>
$ git add --all
$ git commit
$ git push origin master
```
将`public`下的静态网页文件移置GitHub Pages的仓库中，提交推送就OK啦。
```
$ hexo generate --deploy
```
使用这个命令就可以将生成静态网页文件与提交到GitHub这两步结合起来，比较方便，前提是要先在配置文件中写好GitHub的相关信息。

稍等片刻，访问`https://yourname.github.io`，我们的个人博客搭建完成啦。

# 备份建议

毫无疑问，博客的博文资料是最珍贵的，框架可以换，但博文资料我们需要采用某些措施妥善的备份起来。
子恒喵给大家的建议是，使用**GitHub备份博文资料**。
由于我们上传到GitHub Pages 仓库上的是借助Hexo生成器生成的静态网页文件，我们不希望备份它们，这对日后博客内容的移植，修改造成不便。
我们需要将本地的Markdown文件备份下来，这才是我们需要做的。
Hexo本地目录下`source`文件夹存放着我们写的博文内容，我们只需要使用GitHub将这些内容备份起来。
这里子恒喵使用了`Git branch`分支功能来保存博文资料，没有新建出一个仓库。

```
$ git clone <your-remote-github-repo.git>
$ cd <folder>
$ git branch source
$ git checkout source
$ rm -rf ./[^.]*
$ cp -r -v <hexo/source> ./
$ git add --all
$ git commit
$ git push origin source
```
虽然Git branch分支功能并不是为备份数据而生的，但是在这个特殊场景下，利用分支功能却非常合适。`master`分支存放静态网页文件，`source`分支存放Markdown文件。备份工作完满完成。


# 参考资料

- Git官方网站:      https://git-scm.com/
- Node.js 官方网站: https://nodejs.org/
- Hexo官方网站:     https://hexo.io/
- GitHub-Pages介绍: https://help.github.com/articles/what-is-github-pages/

# 更新日志

- 2017-05-14 --> 完成[初稿]
- 2017-05-15 --> 添加[备份建议]
- 2018-03-05 --> 修改[表述语句]

