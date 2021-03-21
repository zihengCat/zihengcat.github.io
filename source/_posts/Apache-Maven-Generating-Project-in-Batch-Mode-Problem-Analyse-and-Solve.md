---
title: Apache Maven "[INFO] Generating project in Batch mode" 问题分析及解决
subtitle: Apache Maven "[INFO] Generating project in Batch mode" Problem Analyse and Solve
catalog: true
date: 2018-09-30
tags: ["Java", "Apache", "Maven"]
---

# 前言

在学习 Apache Maven 官方指导教程 *Maven in 5 Minutes* 过程中，新人很容易遇到这样一个问题：创建一个`Maven`项目（Creating a `Maven` Project）时，`Maven`构建卡在了如下这步。

```plain
...
[INFO] Generating project in Batch mode
...
```
> 代码清单：`Maven`构建项目 - 卡住步骤

# 问题分析

在执行`mvn`创建项目命令时，额外加入`-X`参数，查看该命令执行的`[DEBUG]`详细信息。

可以发现，Maven 构建程序实际是在如下这步卡住的。

```plain
...
[DEBUG] Searching for remote catalog: http://repo1.maven.org/maven2/archetype-catalog.xml
...
```
> 代码清单：`Maven`构建项目 - 卡住具体步骤

可以推测出，引起该问题的原因是：**由于国内恶劣的网络条件，导致 Maven 在查询/下载目标文件`archetype-catalog.xml`时遇到了障碍。**

# 解决方案

已经知道了问题原因所在，解决方法也非常简单：**手动下载目标文件至本地对应路径，将 Maven 加载方式改为本地即可。**

```bash
# 手动下载目标文件至本地
$ curl -o 'archetype-catalog.xml' \
          'http://repo1.maven.org/maven2/archetype-catalog.xml'
...
# 移动目标文件至 Maven 目录
$ mv archetype-catalog.xml \
     ~/.m2/repository/org/apache/maven/archetype/archetype-catalog/3.0.1/
...
# 加入参数`-DarchetypeCatalog=local`将 Maven 加载方式改为本地
$ mvn archetype:generate \
     -DgroupId=com.mycompany.app \
     -DartifactId=my-app \
     -DarchetypeArtifactId=maven-archetype-quickstart \
     -DinteractiveMode=false \
     -DarchetypeCatalog=local \
     -X
...
```
> 代码清单：`Maven`构建项目 - 问题解决

# 参考资料

- Apache Maven: https://maven.apache.org/

- Maven in 5 Minutes: https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html

