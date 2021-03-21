---
title: Spark SQL 存储模式（SaveMode）概览
subtitle: Spark SQL SaveMode Overview
catalog: true
date: 2019-03-04
tags: ["BigData", "Spark"]
---

# 前言

本文对 Spark SQL 几种存储模式（SaveMode）做整理。

# Spark SQL 存储模式（SaveMode）

Spark SQL 存储模式（SaveMode）枚举类型（Enum）位于`org.apache.spark.sql.SaveMode`中，有4种模式，默认为`ErrorIfExists`模式。

| 存储模式        | 解释说明 |
| :-------------: | -------- |
| `ErrorIfExists` | 默认模式，如果表已存在，直接报错 |
| `Append`        | 追加模式，如果表已存在，追加数据至该表中；如果表不存在，则先创建表，再写入数据 |
| `Overwrite`     | 覆写模式，如果表已存在，删除已有表及其数据，重新创建该表，写入新数据 |
| `Ignore`        | 跳过模式，如果表已存在，则不写入新数据，不报错 |

> 表：Spark SQL 存储模式（SaveMode）概览

# 参考资料

- Spark Java Doc: http://spark.apache.org/docs/latest/api/java/index.html?org/apache/spark/sql/SaveMode.html

