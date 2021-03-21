---
title: Apache Airflow 1.10.0 新版发布 | 高光特性
subtitle: Apache Airflow 1.10.0 Released | Highlights
catalog: true
date: 2018-10-08
tags: [Apache, Airflow]
---

# 前言

在经过8个月漫长的等待后，*Apache Airflow 1.10.0* 终于发布了。

# 高光特性（Highlights）

新 Role-Based Access Control (RBAC) 网页接口现已可用（in Beta）。Flask AppBuilder 取代 Flask Admin 成为新的 RBAC Web UI。这一改变将支持更多、更智能、开箱即用的认证后端，以及视图与 ORM 权限控制，简化 View 与 DAG 的权限控制等级。你可以在`airflow.cfg`配置文件中指定相关配置项来启用这一新特性。

```
...
rbac = True
...
```
> 代码清单：`airflow.cfg` RBAC 配置项

新 **Kubernetes Operator and Executor** 现在已支持，使用 Kubernetes 原生 API 实现。这使得开发者们可以更自由、更灵活的使用 Airflow。

![Airflow-Kubernetes-Operator](./airflow-kubernetes-operator.png)

> 图：Airflow 操作 Kubernetes

大型 DAG **性能优化**：包含大量子任务的 DAGs 如今会运行得更快、更好。

> 感谢 PR 贡献：https://github.com/apache/incubator-airflow/pull/3116

**时区支持**，参看以下详情链接。

> https://github.com/apache/incubator-airflow/blob/master/docs/timezone.rst

增添 GCP（Google Kubernetes Engine）与 AWS（Amazon Web Service）智能化支持。

大量 Bug 修复：参看以下详情链接。

> https://github.com/apache/incubator-airflow/blob/master/CHANGELOG.txt

文档更新：查看最新 1.10.0 版本文档，请访问详情链接。

> https://airflow.apache.org/

# 安装与更新（Installation / Upgrading）

由于 GPL 协议许可的问题，最新版本的 Apache Airflow 更新了依赖库，在安装或更新时候，用户需要设置环境变量：`SLUGIFY_USES_TEXT_UNIDECODE=yes`或`AIRFLOW_GPL_UNIDECODE=yes`。Airflow 会按照顺序选择安装依赖项（`python-nvd3` -> `python-slugify` -> `unidecode`）。

```bash
$ export AIRFLOW_GPL_UNIDECODE=yes
...
$ export SLUGIFY_USES_TEXT_UNIDECODE=yes
...
```
> 代码清单：设置 Airflow 环境变量

# 参考资料

- Medium: https://medium.com/datareply/apache-airflow-1-10-0-released-highlights-6bbe7a37a8e1

- Airflow Documents (1.10.0): https://airflow.apache.org/

