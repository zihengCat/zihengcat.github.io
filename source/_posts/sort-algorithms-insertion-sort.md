---
title: 排序算法 - 插入排序
subtitle: Sort Algorithms - Insertion Sort
date: 2018-03-09
tags: ["Algorithm", "Sort"]
---

# 前言

本文介绍*排序算法 - 插入排序（Insertion Sort）*。

# 插入排序（Insertion Sort）

插入排序（Insertion Sort）是常用的排序算法之一。该排序算法的核心思想是：**构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。**

插入排序与我们在手中排序扑克牌的过程类似。

- 桌上的扑克牌：`[5, 2, 4, 6, 1, 3]`

- 拿起第一张牌：`[5]`

- 拿起第二张牌（将牌`2`插入到手牌）：`[2, 5]`

- 拿起第三张牌（将牌`4`插入到手牌）：`[2, 4, 5]`

- 拿起第四张牌（将牌`6`插入到手牌）：`[2, 4, 5, 6]`

- ...

> 注：插入排序扑克牌

```c
/**
 * 插入排序（Insertion Sort）
 * 参数: 待排序数组, 数组长度
 * 返回: 无
 */
void insertionSort(int* array, int length) {
    for (int i = 0; i < length; ++i) {
        /* 取得待排序元素 */
        int key = array[i];
        /* 取得已排序数组索引 */
        int j = i - 1;
        /* 在已排序数组中从后向前扫描，找到相应索引位置 */
        while (j >= 0 && array[j] > key) {
            array[j+1] = array[j];
            --j;
        }
        /* 插入元素 */
        array[j+1] = key;
    }
}
```
> 代码清单：插入排序（`C/C++`实现）

# 算法复杂度

按平均情况来看，插入排序（Insertion Sort）算法复杂度为：$O(n^2)$，因而，插入排序不太适合数据量较大的排序应用。但是如果需要排序的数据量较小，且已知输入元素大致上按照顺序排列，那么插入排序还是一个不错的选择。

# 参考资料

- Fundamental Data Structures and Algorithms: https://www.bilibili.com/video/av52559737/

