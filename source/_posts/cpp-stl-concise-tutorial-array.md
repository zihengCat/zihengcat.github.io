---
title: C++ STL 简明教程：Array
subtitle: C++ STL Concise Tutorial - Array
catalog: true
date: 2020-05-01
tags: ["C++", "STL"]
---

# 概述

C++ 标准模版库 `std::array` 是对 C 语言原生定长数组的容器化封装，于 C++ 11 被加入到 STL 中。

# 头文件

```cpp
#include <array>
```

# 创建容器

```cpp
// 初始化列表
array<int, 3> arrInt = {1, 2, 3, };
array<double, 3> arrDouble = {1.0, 2.0, 3.0, };
array<string, 3> arrStr = {std::string("a"), "b", "c", };
array<int*, 3> arrPointer = {NULL, NULL, NULL, };
```

# 元素访问

```cpp
array<int, 3> arr = {1, 2, 3, };

// 下标访问
cout << arr[0] << endl;
cout << arr.at(0) << endl;

// 迭代器
for (int *iter = arr.begin(); iter != arr.end(); iter++) {
    cout << *iter << endl;
}

// forEach 迭代
for (int n : arr) {
    cout << n << endl;
}
```

# 常用方法

| 方法         | 解释 |
| :----------- | :--- |
| `at`         | 下标访问（带边界检查） |
| `operator[]` | 下标访问（无边界检查） |
| `front`      | 访问首位元素 |
| `back`       | 访问末位元素 |
| `data`       | 获取底层 C 数组指针 |
| `size`       | 获取容器容量 |
| `fill`       | 填充容器元素 |

> 表：C++ STL `std::array` 常用方法表

# 参考资料

- C++ Reference: https://en.cppreference.com/w/cpp/container/array

<!-- EOF -->
