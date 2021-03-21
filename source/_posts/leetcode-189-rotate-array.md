---
title: LeetCode - 189. 旋转数组
subtitle: LeetCode - 189. Rotate Array
catalog: true
date: 2019-08-31
tags: ["LeetCode", "Data Structure", "Algorithm", "Array"]
---

# 前言

本文记录 *LeetCode - 189. 旋转数组（Rotate Array）* 问题。

# 问题描述

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 为非负数。

```plain
Input: [1,2,3,4,5,6,7] and k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```
> 样例1

```plain
Input: [-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
Explanation:
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```
> 样例2

说明：

- 请你尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。

- 能否使用空间复杂度为$O(1)$的原地算法呢？

# 问题解答

经典的数组问题，数组元素向后移动，越界元素会被添加到数组头部。依照这一思路，我们可以使用暴力平移数组元素的方法。

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        int tmp = 0;
        for (int i = 0; i < k; ++i) {
            /* 暂存数组末位元素 */
            tmp = nums[nums.length - 1];
            /* 数组元素后移一位 */
            shiftArray(nums);
            /* 将末位元素置换到数组头部 */
            nums[0] = tmp;
        }
    }
    private void shiftArray(int[] arr) {
        /* 后移数组元素 */
        for (int j = arr.length - 2; j >= 0; --j) {
            arr[j + 1] = arr[j];
        }
    }
}
/* EOF */
```
> 代码清单：暴力平移法（`Java`实现）

暴力平移数组元素的方法效率比较低下，可以引入一个双向队列，辅助完成操作。

```java
import java.util.Deque;
import java.util.LinkedList;
public class Solution {
    public void rotate(int[] nums, int k) {
        /* 引入辅助双向队列 */
        Deque<Integer> deque = new LinkedList<Integer>();
        for (int i = 0; i < nums.length; ++i) {
            deque.add(nums[i]);
        }
        /* 将末位元素置换到队列头部 */
        for (int j = 0; j < k; ++j) {
            deque.addFirst(
                deque.removeLast()
            );
        }
        /* 复制元素 */
        Object[] numsArr = deque.toArray();
        for (int i = 0; i < nums.length; ++i) {
            nums[i] = (int)numsArr[i];
        }
    }
}
/* EOF */
```
> 代码清单：双向队列辅助法（`Java`实现）

另一个解决该问题比较优秀的算法是：三步翻转法。

```plain
-----------------------------------------------
原始数组                        : 1 2 3 4 5 6 7
k                               : 3
-----------------------------------------------
翻转数组（第一轮）              : 7 6 5 4 3 2 1
翻转数组前 k 个数字（第二轮）   : 5 6 7 4 3 2 1
翻转数组后 n-k 个数字（第三轮） : 5 6 7 1 2 3 4 -> 最终结果
-----------------------------------------------
```
> 注：三步翻转法

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        /* 过滤多余步骤，防止数组越界 */
        k = k % nums.length;
        /* 三轮翻转 */
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    private void reverse(int[] arr, int start, int end) {
        for(int i = start, j = end; i < j; ++i, --j) {
            swap(arr, i, j);
        }
    }
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
/* EOF */
```
> 代码清单：三步翻转法（`Java`实现）

说明一下`k = k % nums.length`语句的作用：当输入 *k* 大于等于数组长度时，会出现冗余操作；过长的 *k* 值还会造成数组越界的情况出现。

```plain
Input [1,2,3], k = 5:
When k=0, it returns [1,2,3].
When k=1, it returns [3,1,2].
When k=2, it returns [2,3,1].
When k=3, it returns [1,2,3], which is the same result as the situation k=0.
When k=4, it returns [3,1,2], the same result as the situation k=1.
...
So we can see that the result is related to the remainder of (k/nums.length).
We can use the mod operator.
```
> 注：关于取余操作`%`的说明

# 参考资料

- 「LeetCode（Rotate Array）」：https://leetcode.com/problems/rotate-array/

