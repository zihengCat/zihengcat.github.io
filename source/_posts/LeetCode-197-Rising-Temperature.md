---
title: LeetCode - 197. 上升温度（Rising Temperature）
date: 2018-11-14
catalog: true
tags: [LeetCode, SQL]
---

# 前言

本文记录*LeetCode - 197. 上升温度（Rising Temperature）*问题。

# 问题描述

给出如下`Weather`表，请你写一条`SQL`查询语句，找出前一天温度比今天高的所有记录的`Id`号。

```plain
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
```
> 注：`Weather`天气表

举例说明，对于这张`SQL`表，查询语句执行的结果应为：

```plain
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```
> 注：`SQL Query`查询结果集

# 问题解答

为了解决这道SQL问题，我们需要使用`SQL`表的自关联功能。

```sql
SELECT w1.Id
FROM
    Weather AS w1,
    Weather AS w2
WHERE
    w1.Temperature > w2.Temperature
AND
    DATEDIFF(w1.RecordDate, w2.RecordDate) = 1;
```
> 代码清单：`SQL Query`语句

```sql
SELECT w1.Id
FROM
    Weather AS w1
JOIN
    Weather AS w2
ON
    w1.Temperature > w2.Temperature
AND
    DATEDIFF(w1.RecordDate, w2.RecordDate) = 1;
```
> 代码清单：`SQL Query`语句

自关联表，为两张表分别取别名，同时满足（AND）两个筛选条件为：温度更高、日期之差为`1`日。

# 参考资料

- LeetCode: https://leetcode.com/problems/rising-temperature/

