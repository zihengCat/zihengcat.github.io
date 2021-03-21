---
title: C++ STL 简明教程：Pair
subtitle: C++ STL Concise Tutorial - Pair
catalog: true
date: 2020-05-13
tags: ["C++", "STL"]
---

# 概述

C++ 标准模版库 `std::pair` 是一枚模版容器，可以将两个对象封装为一个独立单元，它是 `std::tuple` 的两元素特化版。

# 头文件

```cpp
#include <utility>
```

# 创建容器

```cpp
// 构造函数
pair<int, int> aPair(1, 2);
// 初始化列表
pair<int, double> bPair = {1, 3,14, }
// 模版函数
pair<const char*, void*> cPair = make_pair("hello", (void*)NULL);
```

# 元素访问

```cpp
pair<int, int> aPair = {1, 2, };
// 访问成员变量
cout << aPair.first << endl;
cout << aPair.second << endl;
```

# 常用方法

| 方法         | 解释 |
| :----------- | :--- |
| `operator=`  | 深拷贝 Pair |
| `operator==` | 比较两 Pair 是否相等 |
| `swap`       | 交换两 Pair 元素内容 |

> 表：C++ STL `std::pair` 常用方法表

# 参考资料

- C++ Reference: https://en.cppreference.com/w/cpp/utility/pair

<!-- EOF -->
