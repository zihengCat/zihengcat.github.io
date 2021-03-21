---
title: LeetCode - 627. 交换薪水（Swap Salary）
date: 2018-05-25
tags: [LeetCode, SQL]
---

# 前言

本文记录*LeetCode - 627. 交换薪水（Swap Salary）*问题。

# 问题描述

给出一张名为`salary`的SQL table（如下所示），其中，`m=male`，`f=female`。请你仅使用一条`UPDATE`查询语句（不建立临时表）交换`f`与`m`的值（即将所有的`f`都变成`m`，`m`变成`f`）。

比如有这样一张`SQL`表：
```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
```

执行完查询语句后，该表变为：
```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
```

# 问题解法

这是一道简单的`SQL`题目。更新`SQL`表使用`UPDATE`查询，而交换性别值（字符串）则可以使用`IF`函数完成，其语法格式如下所示。

```
IF(expr1, expr2, expr3)
expr1 == true ? expr2 : expr3
```
> 注：`IF`函数语法说明

```sql
UPDATE salary
SET sex = IF(sex = "m", "f", "m");
```
> 代码清单：`SQL`查询语句（`IF`函数）

# 拓展延伸

`IF`函数虽然简单好用，但是只能处理两种情况，对于多种（大于2）选项情况的查询，就可以考虑使用`CASE...WHEN`结构。比如这道题，可以改写成`CASE...WHEN`结构。

```sql
UPDATE salary
SET sex =
CASE
    WHEN sex = "m" THEN "f"
    WHEN sex = "f" THEN "m"
END;
```
> 代码清单：`SQL`查询语句（`CASE...WHEN`结构）

# 参考资料

- SegmentFault专栏：https://segmentfault.com/a/1190000009676728

