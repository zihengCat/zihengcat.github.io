---
title: 排序算法 - 冒泡排序
subtitle: Sort Algorithms - Bubble Sort
date: 2018-03-06
tags: ["Algorithm", "Sort"]
---

# 前言

本文介绍*排序算法 - 冒泡排序（Bubble Sort）*。

# 冒泡排序（Bubble Sort）

冒泡排序是常用的排序算法之一，该排序算法的核心思想是：**将数组中最大/最小的值冒泡浮到数组尾部，依次迭代。**

```c
/**
 * 冒泡排序（Bubble Sort）
 * 参数: 待排序数组, 数组长度
 * 返回: 无
 */
void bubbleSort(int* array, int length) {
    for (int i = 0; i < length; ++i) {
        /* 迭代至 length - i 位置，跳过已排好序的部分 */
        for (int j = 1; j < length - i; ++j) {
            if (array[j - 1] > array[j]) {
                swap(array, j - 1, j);
            }
        }
    }
}
/**
 * 功能函数：交换数组指定索引位置元素
 */
void swap(int* arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```
> 代码清单：选择排序（`C/C++`代码）

# 算法复杂度

冒泡排序（Bubble Sort）算法非常简单易懂，算法复杂度为：$O(n^2)$，效率并不高。

# 参考资料

- Fundamental Data Structures and Algorithms: https://www.bilibili.com/video/av52559737/

