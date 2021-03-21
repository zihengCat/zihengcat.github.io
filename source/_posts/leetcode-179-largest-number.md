---
title: LeetCode - 179. 最大数排列
subtitle: LeetCode - 179. Largest Number
catalog: true
date: 2019-11-05
tags: ["LeetCode", "Data Structure", "Array", "String"]
---

# 前言

本文记录 *LeetCode - 179. 最大数排列（Largest Number）* 问题。

# 问题描述

给定一枚只包含「非负整数」的列表，请将列表元素重新排列，使得排列后元素拼接出的数字最大。

```plain
输入: [10, 2]
输出: "210"
```
> 注：示例 1

```plain
输入: [3, 30, 34, 5, 9]
输出: "9534330"
```
> 注：示例 2

注意：输出结果可能非常大，所以你需要返回一个字符串而不是整数。

# 问题解答

解决该问题的核心思路是：对输入列表中的元素进行重排列，排序依据是两整数排列结果最大。

```java
import java.util.Arrays;
import java.util.Comparator;
public class Solution {
    public String largestNumber(int[] nums) {
        /* 处理异常 */
        if (nums == null || nums.length == 0) {
            return "";
        }
        /* 整数转字符串 */
        String[] strNums = new String[nums.length];
        for (int i = 0; i < nums.length; ++i) {
            strNums[i] = String.valueOf(nums[i]);
        }
        /* 元素重排列 */
        Arrays.sort(strNums, new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                String r1 = s1 + s2;
                String r2 = s2 + s1;
                char[] v1 = r1.toCharArray();
                char[] v2 = r2.toCharArray();
                int len1 = r1.length();
                int len2 = r2.length();
                int kim = Math.min(len1, len2);
                int i = 0;
                while (i < kim) {
                    char c1 = v1[i];
                    char c2 = v2[i];
                    if (c1 != c2) {
                        return c2 - c1;
                    }
                    ++i;
                }
                return len2 - len1;
            }
        });
        /* 处理 0 值 */
        if (strNums[0].equals("0")) {
            return "0";
        }
        /* 拼接结果 */
        StringBuilder sb = new StringBuilder();
        for (String s : strNums) {
            sb.append(s);
        }
        return sb.toString();
    }
}
/* EOF */
```
> 代码清单：最大数排列（`Java`实现）

# 参考资料

- 「LeetCode - 179. 最大数排列（Largest Number）」：https://leetcode.com/problems/largest-number/

<!-- EOF -->
