---
title: LeetCode - 448. 寻找数组中消失的数
subtitle: LeetCode - 448. Find All Numbers Disappeared in an Array
catalog: true
date: 2019-05-20
tags: ["LeetCode", "Data Structure", "Algorithm", "Array", "HashSet"]
---

# 前言

本文记录 *LeetCode - 448. 寻找数组中消失的数（Find All Numbers Disappeared in an Array）* 问题。

# 问题描述

输入一个整型数组：`1 ≤ a[i] ≤ n`（`n`为数组长度），数组中某些元素出现了两次，其他元素则出现一次。

请你寻找在`[1, n]`范围内，该数组中没有出现的数，将这些消失的数以新数组形式返回。

```plain
Input: [4, 3, 2, 7, 8, 2, 3, 1]

Output: [5, 6]
```
> 注：输入输出样例

注意：

- 输入数组取值范围：`1 ≤ a[i] ≤ n`（`n`为数组长度）

- 输出数组排列顺序无关紧要

# 问题解答

这个问题需要我们将给定数组对比完整数组`[1, n]`（`n`为数组长度），寻找给定数组中缺失的数。

但是给定数组中某些数出现了多次（两次），这会影响我们的判断，使用`Set`数据结构过滤给定数组中多次出现的元素，再将其与完整数组作对比，即可找到给定数组中缺失的数。

另外需要注意的一点是，完整数组的取值范围为`[1, n]`（`n`为数组长度），遍历时索引下标从`1`开始。

```java
import java.util.List;
import java.util.LinkedList;
import java.util.Set;
import java.util.HashSet;
public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> list = new LinkedList<Integer>();
        Set<Integer> set = new HashSet<Integer>();
        /* Filter array of integer using a Set<Integer> */
        for (int i = 0; i < nums.length; ++i) {
            set.add(nums[i]);
        }
        /* Compare to full array of integer, find missing numbers */
        for (int i = 1; i <= nums.length; ++i) {
            if (!set.contains(i)) {
                list.add(i);
            }
        }
        return list;
    }
}
```
> 代码清单：寻找数组中消失的数（`Java`实现）

# 参考资料

- LeetCode: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/

