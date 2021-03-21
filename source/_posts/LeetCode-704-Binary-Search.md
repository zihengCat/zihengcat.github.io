---
title: LeetCode - 704. 二分查找
subtitle: LeetCode - 704. Binary Search
catalog: true
date: 2019-05-14
tags: ["LeetCode", "Algorithm", "Array"]
---

# 前言

本文记录 *LeetCode - 704. 二分查找* 问题。

# 问题描述

输入一个**已排序（升序）**的、**拥有`n`个元素**的整型数组`nums`与一个目标数`target`，请你实现一个函数，在整型数组中搜索目标数。如果该数存在，返回其在数组中的索引下标，不存在则返回`-1`。

```plain
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```
> 注：样例-1

```plain
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```
> 注：样例-2

注意：

- 可以假设数组中的元素都是唯一不重复的。

- `n`的取值范围为：`[1, 10000]`。

- 整型数组中元素的取值范围为：`[-9999, 9999]`。

# 问题解答

非常经典的二分查找问题，来复习一下二分查找法吧。

```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        int mid, midVal;
        while (low <= high) {
            mid = (low + high) / 2;
            midVal = nums[mid];
            if (target > midVal) {
                low = mid + 1;
            } else if (target < midVal) {
                high = mid - 1;
            } else {
                /* key found */
                return mid;
            }
        }
        /* key not found */
        return -1;
    }
}
```
> 代码清单：二分查找（`Java`实现）

```c
int search(int* nums, int numsSize, int target) {
    int low = 0, high = numsSize - 1, mid, midVal;
    while (low <= high) {
        mid = (low + high) / 2;
        midVal = nums[mid];
        if (target == midVal) {
            /* Found */
            return mid;
        } else {
            if (target > midVal) {
                low = mid + 1;
            } else if (target < midVal) {
                high = mid - 1;
            }
        }
    }
    /* Not found */
    return -1;
}
```
> 代码清单：二分查找（`C/C++`实现）

二分查找法还可以写成递归形式。

```c
int search(int* nums, int numsSize, int target) {
    return searchRecursive(nums, 0, numsSize - 1, target);
}
int searchRecursive(int* nums, int startIndex, int endIndex, int target) {
    if (startIndex <= endIndex) {
        int midIndex = startIndex + (endIndex - startIndex) / 2;
        /* compare midElement with target */
        if (nums[midIndex] == target) {
            /* Found */
            return midIndex;
        } else if (nums[midIndex] < target) {
            /* midElement is too small - go right */
            return searchRecursive(nums, midIndex+1, endIndex, target);
        } else {
            /* midElement is too big - go left */
            return searchRecursive(nums, startIndex, midIndex-1, target);
        }
    }
    /* Not found */
    return -1;
}
```
> 代码清单：递归二分查找（`C/C++`实现）

# 参考资料

- LeetCode: https://leetcode.com/problems/binary-search/

