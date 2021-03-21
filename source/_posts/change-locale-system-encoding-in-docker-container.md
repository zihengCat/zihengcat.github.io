---
title: 修改 Docker 容器 locale 系统编码
subtitle: Change locale System Encoding in Docker Container
catalog: true
date: 2018-09-10
tags: [Linux, Docker]
---

# 前言

默认情况下，我们使用 Docker 镜像（Image）启动 Docker 容器，容器 Linux 系统默认使用的`locale`系统编码为`POSIX`，有时候这会为开发工作带来极大的不便（不支持 Unicode 显示）。

```
$ locale
...
LANG=
LC_CTYPE="POSIX"
LC_NUMERIC="POSIX"
LC_TIME="POSIX"
LC_COLLATE="POSIX"
LC_MONETARY="POSIX"
LC_MESSAGES="POSIX"
LC_PAPER="POSIX"
LC_NAME="POSIX"
LC_ADDRESS="POSIX"
LC_TELEPHONE="POSIX"
LC_MEASUREMENT="POSIX"
LC_IDENTIFICATION="POSIX"
LC_ALL=
...
```
> 代码清单：查看`Docker`容器内部`locale`系统编码

# 改变`locale`编码格式

我们可以在`Dockerfile`中指定系统需要使用的`locale`编码格式，再构建镜像。这样启动的`Docker`容器中就会使用指定好的系统编码了。

```
...
ENV LANG     en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL   en_US.UTF-8
...
```
> 代码清单：在`Dockerfile`中指定`locale`系统编码为`en_US.UTF-8`

或者，我们可以在启动容器时，为该容器指定系统编码格式环境变量。

```
$ sudo docker run <SOME_DOCKER_IMAGE> \
       --env LC_ALL=en_US.UTF-8 \
...
```
> 代码清单：在容器启动时指定`locale`系统编码为`en_US.UTF-8`

# 参考资料

- Stackoverflow「28405902」：https://stackoverflow.com/questions/28405902/how-to-set-the-locale-inside-a-ubuntu-docker-container

- Docker and Locales：http://jaredmarkell.com/docker-and-locales/

