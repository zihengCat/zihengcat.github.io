---
title: 排序算法 - 选择排序
subtitle: Sort Algorithms - Selection Sort
date: 2018-03-08
tags: ["Algorithm", "Sort"]
---

# 前言

本文介绍*排序算法 - 选择排序（Selection Sort）*。

# 选择排序（Selection Sort）

选择排序是一种简单直观的排序算法，该排序算法的核心思想是：**在未排序序列中寻找最大（小）的元素，置换到序列起始位置，依次迭代。**

| 迭代轮数 | 数组元素       |
| :------: | :------------: |
| 0        | [ 2, 1, 5, 6 ] |
| 1        | [ 6, 1, 5, 2 ] |
| 2        | [ 6, 5, 1, 2 ] |
| 3        | [ 6, 5, 2, 1 ] |

> 表：选择排序（降序）

可以看到，每轮迭代后，就有一个值被放到了正确的索引位置上。

```c
/**
 * 选择排序 (Selection Sort)
 * 参数: 待排序数组, 数组长度
 * 返回: 无
 */
void selectionSort(int* array, int length) {
    /* 每轮迭代都有一个数被归位，下一轮迭代从下个位置开始 */
    for (int i = 0; i < length; ++i) {
        putLargestValue(array, length, i);
    }
}
void putLargestValue(int* array, int length, int index) {
    int largestValueIndex = index;
    /* 找到当前数组中最大元素的下标 */
    for (int i = index; i < length; ++i) {
        if (array[i] > array[largestValueIndex]) {
            largestValueIndex = i;
        }
    }
    /* 将找到元素与数组最前位置元素进行交换 */
    swap(array, index, largestValueIndex)
}
void putSmallestValue(int* array, int length, int index) {
    int smallestValueIndex = index;
    /* 找到当前数组中最小元素的下标 */
    for (int i = index; i < length; ++i) {
        if (array[i] < array[smallestValueIndex]) {
            smallestValueIndex = i;
        }
    }
    /* 将找到元素与数组最前位置元素进行交换 */
    swap(array, index, smallestValueIndex);
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
> 代码清单：选择排序（`C/C++`实现）

# 算法复杂度

可以看到，选择排序的时间复杂度为：$O(n^2)$，并不算是一种很高效的排序算法，当空间复杂度要求较高时，可以考虑选择排序，其实际适用的场景较少。

# 参考资料

- Fundamental Data Structures and Algorithms: https://www.bilibili.com/video/av52559737/

