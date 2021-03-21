---
title: LeetCode - 896. 单调数组（Monotonic Array）
catalog: true
date: 2019-03-15
tags: ["LeetCode", "Data Structure", "Algorithm", "Array"]
---

# 前言

本文记录*LeetCode - 896. 单调数组（Monotonic Array）*问题。

# 问题描述

判断给定数组是否为单调数组（Monotonic Array），单调递增（Monotone Increasing）或单调递减（Monotone Decreasing）。

- 单调递增数组：对于任意`i <= j`，均有`A[i] <= A[j]`。

- 单调递减数组：对于任意`i <= j`，均有`A[i] >= A[j]`。

相关限定条件如下：

- `1 <= A.length <= 50000`

- `-100000 <= A[i] <= 100000`

输入整型数组，判断其是否为单调数组，返回布尔值。

具体样例如下：

```
Input: [1, 2, 3]
Output: true
```
> 注：样例 1

```
Input: [1, 2, 2, 4]
Output: true
```
> 注：样例 2

```
Input: [1, 3, 2]
Output: false
```
> 注：样例 3

# 问题解答

该问题的解法简单直接，遍历数组，对比数组前后元素的数值，判断数组是否单调。共有3种情况：非单调，单调递增，单调递减。

```c
bool isMonotonic(int* A, int ASize) {
    /* Set origin status to `true`
       in order to operate `&=` */
    bool isIncreasing = true,
         isDecreasing = true;
    for (int i = 1; i < ASize; ++i) {
        isIncreasing &= A[i - 1] <= A[i],
        isDecreasing &= A[i - 1] >= A[i];
    }
    /* Only three options */
    return isIncreasing || isDecreasing;
}
```
> 代码清单：`C`实现

```c++
class Solution {
public:
    bool isMonotonic(vector<int> A) {
        /* Set origin status to `true`
           in order to operate `&=` */
        bool isIncreasing = true,
             isDecreasing = true;
        for (int i = 1; i < A.size(); ++i) {
            isIncreasing &= A[i - 1] <= A[i],
            isDecreasing &= A[i - 1] >= A[i];
        }
        /* Only three options */
        return isIncreasing || isDecreasing;
    }
};
```
> 代码清单：`C++`实现

```java
class Solution {
    public boolean isMonotonic(int[] A) {
        /* Set origin status to `true`
           in order to operate `&=` */
        boolean isIncreasing = true,
                isDecreasing = true;
        for (int i = 1; i < A.length; ++i) {
            isIncreasing &= A[i - 1] <= A[i],
            isDecreasing &= A[i - 1] >= A[i];
        }
        /* Only three options */
        return isIncreasinginc || isDecreasing;
    }
}
```
> 代码清单：`Java`实现

```python
class Solution:
    def isMonotonic(self, A: List[int]) -> bool:
        isIncreasing: bool = True
        isDecreasing: bool = True
        for i in range(1, len(A)):
            isIncreasing &= A[i - 1] <= A[i]
            isDecreasing &= A[i - 1] >= A[i]
        return isIncreasing or isDecreasing
```
> 代码清单：`Python`实现

# 参考资料

- LeetCode: https://leetcode.com/problems/monotonic-array/

