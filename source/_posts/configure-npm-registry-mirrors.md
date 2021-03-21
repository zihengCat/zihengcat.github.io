---
title: NPM 包管理工具 Registry 镜像配置
subtitle: Configure NPM Registry Mirrors
catalog: true
date: 2019-01-26
tags: ["JavaScript", "Node.js", "NPM"]
---

# 前言

`npm`是一款 JavaScript 世界的软件包管理工具，也是是 Node.js 平台默认的软件包管理工具。通过`npm`我们可以方便地安装、共享、分发代码，管理项目依赖。但是，NPM 的代码仓库（Registry）默认托管在 Amazon S3 上，国内访问不便，我们可以使用国内镜像（如：淘宝 NPM 镜像）代替。

# 配置 NPM Registry 镜像

先查看当前`npm`使用的 Registry 仓库地址。默认地址为：https://registry.npmjs.org/

```bash
$ npm config get registry
...
```
> 代码清单：查看当前`npm` Registry 仓库地址

我们可以使用`npm config set`设置 Registry 仓库镜像，也可以直接将配置写入到`.npmrc`配置文件中。

```bash
$ npm config set registry 'https://registry.npm.taobao.org/'
```
> 代码清单：使用`npm config`设置 Registry 仓库镜像

```bash
$ echo 'registry = https://registry.npm.taobao.org/' >> ~/.npmrc
```
> 代码清单：Registry 仓库镜像写入`.npmrc`配置文件

如果只是为了单次使用，直接在运行`npm`命令时加入`--registry`选项即可。

```bash
$ npm --registry 'https://registry.npm.taobao.org/' ...
```
> 代码清单：`npm`命令加入`--registry`镜像地址

# 参考资料

- 淘宝 NPM 镜像站: http://npm.taobao.org/

