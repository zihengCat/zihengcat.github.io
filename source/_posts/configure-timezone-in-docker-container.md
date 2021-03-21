---
title: 配置 Docker 容器系统时区
subtitle: Configure TimeZone in Docker Container
catalog: true
date: 2019-01-06
tags: ["Linux", "Docker"]
---

# 前言

默认情况下，我们通过`Dockerfile`构建出容器镜像，使用 Docker 镜像（Image）启动 Docker 容器，容器内部 Linux 系统使用的时间为**世界标准时间（UTC）**，而非宿主机系统的时区时间。有时候，这一情况会对容器的运行造成不便。

# 问题分析

出现这种情况的原因：**容器内部操作系统并未指定时区（TimeZone）信息，系统默认使用世界标准时（UTC+0）。**

# 配置 Docker 容器系统时区（TimeZone）

了解到问题原因后，可采用如下配置方式，将`Docker`容器系统时区配置正确。

我们可以在编写`Dockerfile`时，显式的为镜像加入时区（TimeZone）信息。

```Dockerfile
...
RUN ...
    cp "/usr/share/zoneinfo/Asia/Shanghai" "/etc/localtime" && \
    echo "Asia/Shanghai" > "/etc/timezone" && \
    ...
...
```
> 代码清单：`Dockerfile`构建镜像时配置时区

如果镜像默认没有写入时区（TimeZone）信息，我们也可以在启动容器时，通过**挂载宿主机时区文件**或者**添加时区系统环境变量**以达到配置`Docker`容器系统时区的目的。

```bash
$ docker run -d \
             -v /etc/localtime:/etc/localtime:ro \
...
```
> 代码清单：`Docker`容器启动时配置时区 - 挂载宿主机时区文件

```bash
$ docker run -d \
             -e "TZ=Asia/Shanghai" \
...
```
> 代码清单：`Docker`容器启动时配置时区 - 添加时区系统环境变量

# 参考资料

- StackOverflow: https://stackoverflow.com/questions/22800624/will-docker-container-auto-sync-time-with-the-host-machine

- ivankrizsan's blog: https://www.ivankrizsan.se/2015/10/31/time-in-docker-containers/

