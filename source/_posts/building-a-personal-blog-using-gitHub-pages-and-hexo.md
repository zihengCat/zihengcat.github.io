---
title: 使用 GitHub Pages + Hexo 搭建个人小站
subtitle: Building a Personal Blog Using GitHub Pages and Hexo
catalog: true
date: 2017-05-14
tags: ["GitHub", "Node.js", "Hexo"]
---

# 前言

此篇文章将指导你使用`GitHub Pages`与`Hexo`搭建个人独立博客。

![Blog-Hexo](./blog-hexo.png)

## 为什么选择 GitHub Pages + Hexo

如今，自媒体行业繁荣，人人都可以在互联网上表达自己的观点与想法。能写东西的地方非常之多（例如：微信公众号、今日头条、知乎专栏...），这些大平台往往可以提供足够的流量与曝光率，那么我们还要选择 GitHub Pages + Hexo 手动搭建个人博客写作平台呢？

基于我个人的一些理解，总结了以下几点。

- 数据可控

- 支持`Markdown`格式书写

- 自定义博客主题，可扩展性强

- 适用 Git WorkFlow, 与 GitHub 完美契合

- 彰显 Geek 精神

- ...

# 环境搭建

首先，我们需要在本机搭建起 GitHub Pages 与 Hexo 的运行环境，后续任何操作步骤都需要以存在运行环境为前提。

## 搭建`Git`环境

前往`Git`官方网站, 根据你使用的计算机架构，选择系统下载安装`Git`，按照提示安装即可。如果`Git`官网无法访问，挂上代理工具尝试。

> Git 官方网站: https://git-scm.com/

部分操作系统可以使用包管理工具一键安装，比如：我个人使用的是 CentOS Linux 操作系统，使用`yum`命令即可安装。

## 搭建`Node.js`环境

前往 Node.js 官方网站, 根据主机的架构与系统，选择合适的版本下载并安装 Node.js 。既可以选择直接下载预编译的二进制包，也可以下载源码包手动编译安装（需要编译环境）。

> Node.js 官方网站：https://nodejs.org/

## 检验安装

添加 Git 与 Node.js 安装路径到`PATH`环境变量中，打开系统命令行工具（Windows下`CMD`），执行以下命令检验安装。如果正常打印出软件版本号，说明安装成功，没有问题。

```bash
$ git --version
...
$ node --version
...
$ npm --version
...
```
> 注：检验 Git 工具与 Node.js 环境

# 安装 Hexo 博客框架

> A fast, simple & powerful blog framework, powered by Node.js.

Hexo 是一个快速、简洁且高效的博客框架，使用`Node.js`写成，支持 Markdown 格式。Hexo 博客框架的安装过程非常简便。使用一条`npm`命令即可安装。

```bash
$ npm install hexo-cli -g
...
```
> 注：> 使用 NPM 包管理器全局安装 Hexo 命令行工具

安装完成后，执行命令检视，如果正确打印出 Hexo 软件版本号，则说明安装成功。

```
$ hexo --version
...
```
> 注：检验 Hexo 博客框架

# Hexo 使用初步

## 项目初始化

将运行环境、相关工具框架都安装完毕之后，我们就可以上手使用 Hexo 博客框架了。

Hexo 使用第一步，便是需要初始化一个博客项目，执行初始化命令 Hexo 脚手架便会帮助我们初始化好一个默认的博客项目。

```bash
$ hexo init <folder>
...
$ cd <folder>
$ npm install
...
```
> 注：Hexo 初始化博客项目

博客新建初始化完成后，其目录结构如下：

```plain
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
> 注：Hexo 初始化博客目录树

## 博客预览

执行以下命令启动一个Web服务器。
```
$ hexo server
```

> 默认情况下，访问网址为：`http://localhost:4000/`。
> 端口号可手动指定。

打开浏览器，键入`http://localhost:4000/`，就可以看到我们的博客了。

## Hexo 更多

更多有关Hexo的信息，我们可以去查阅官方文档。

> Hexo Docs：https://hexo.io/docs/

> Hexo 中文文档：https://hexo.io/zh-cn/docs/

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

- GitHub Pages 简介: https://help.github.com/articles/what-is-github-pages/

- Git 官方网站:      https://git-scm.com/

- Node.js 官方网站: https://nodejs.org/

- Hexo 官方网站:     https://hexo.io/

<!-- EOF -->

