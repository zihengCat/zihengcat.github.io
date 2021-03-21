---
title: 排序算法 - 快速排序
subtitle: Sort Algorithms - Quick Sort
catalog: true
date: 2018-04-08
tags: ["Algorithm", "Sort"]
---

# 前言

本文介绍 *排序算法 - 快速排序（Quick Sort）*。

# 快速排序（Quick Sort）

```java
public class QuickSort {
    public static void qsort(int[] arr, int left, int right) {
        if (left >= right || arr == null) {
            return;
        }
        int index = partition(arr, left, right);
        qsort(arr, left, index - 1);
        qsort(arr, index + 1, right);
    }
    private static void partition(int[] arr, int left, int right) {
        int pivot = arr[left];
        int i = left;
        int j = right;
        while (i < j) {
            while (arr[j] <= pivot && i < j) {
                --j;
            }
            while (arr[i] >= pivot && i < j) {
                ++i;
            }
            if (i < j) {
                swap(arr, i, j);
            }
        }
        swap(arr, i, left);
        return i;
    }
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
/* EOF */
```
> 代码清单：快速排序（Quick Sort）- `Java`实现

```java
import java.util.Arrays;
import QuickSort;
public class QuickSortTest {
    public static void main(String[] args) {
        int[] arr = new int[]{2, 3, 1, 6, 8, 9, 5, 0, 4, 7, 0};
        QuickSort.qsort(arr, 0, arr.length - 1);
        System.out.println(
            Arrays.toString(arr)
        );
    }
}
/* EOF */
```
> 代码清单：快速排序（Quick Sort）- 测试代码

# 参考资料（References）

- 极客学院：https://wiki.jikexueyuan.com/project/easy-learn-algorithm/fast-sort.html

