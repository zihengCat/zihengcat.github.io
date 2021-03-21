---
title: LeetCode - 561. 数组分片 I（Array Partition I）
date: 2018-06-13
catalog: true
tags: [LeetCode, Algorithm, Data Structure]
---

# 前言

本文记录*LeetCode - 561. 数组分片 I（Array Partition I）*问题。

# 问题描述

给出一个含有$2n$个整型数的数组，你的任务是，将这些数分为$n$组，形如：$(a_1, b_1), (a_2, b_2), ... (a_n, b_n)$，使得下式的值取到**最大**，并输出这个最大值。

{% raw %}
$$
S = \sum_{i=1}^{n}\min(a_{i}, b_{i})
$$
{% endraw %}

**示例：**
```
Input: [1,4,3,2]
Output: 4
```

> 解释说明：
> 分组：$n = 2$
> 最大值：$S = 4 = \min(1, 2) + \min(3, 4)$

**注意：**

- $n$是一个正整数，取值范围：$[1, 10000]$
- 数组中所有整数的取值范围：$[-10000, 10000]$

# 问题解法

显然，我们没有办法对一个乱序的数组进行排列组合，使得其每组最小值相加后得到的数值最大。我们首先要对数组进行**排序操作（升序降序均可）**。排序后，分组规则就简单明了，每两个数分一组，遍历求和后得到的数值，一定是最大的。

```C++
/* STL 头文件 */
#include <algorithm>

/* `MIN()`宏函数 */
#define MIN(x, y) \
    ((x) < (y) ? (x) : (y))

class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        /* 对数组进行排序 */
        sort(nums.begin(), nums.end());
        int sum = 0;
        /* 隔二分组 */
        for(int i = 0; i < nums.size(); i += 2) {
            /* 遍历求和 */
            sum += MIN(nums[i], nums[i + 1]);
        }
        return sum;
    }
};
```
> 代码清单：`C++`实现

```java
import java.util.*;
class Solution {
    public int arrayPairSum(int[] nums) {
		Arrays.sort(nums);
        int sum = 0;
        for(int i = 0; i < nums.length; i += 2) {
            sum += this.min(nums[i], nums[i + 1]);
		}
		return sum;
    }
	private int min(int x, int y) {
		return x < y ? x : y;
	}
}
```
> 代码清单：`Java`实现

而使用`Python`，一行代码即可完成。`Python`语言的表现力还是极强的。

```python
class Solution:
    def arrayPairSum(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return sum(sorted(nums)[::2])
```
> 代码清单：`Python`实现

# 参考资料

- LeetCode：https://leetcode.com/problems/array-partition-i/description/

