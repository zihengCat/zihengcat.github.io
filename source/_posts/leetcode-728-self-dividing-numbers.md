---
title: LeetCode - 728. 自整除数
subtitle: LeetCode - 728. Self Dividing Numbers
catalog: true
date: 2019-09-01
tags: ["LeetCode"]
---

# 前言

本文记录 *LeetCode - 728. 自整除数（Self Dividing Numbers）* 问题。

# 问题描述

所谓自整除数（Self Dividing Numbers），是指对自身每一位都能整除的数。

例如，128 就是一个自整除数，因为：`128 % 1 == 0`，`128 % 2 == 0`，`128 % 8 == 0`。

当然，一个自整除数不允许含有0位（除数不能为`0`）。

给出整数序列的上下边界（包含边界），输出该整数序列中所有的自整除数。

```plain
Input: left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]
```
> 注：输入输出样例

注意：

输入参数的上下边界为：`1 <= left <= right <= 10000`

# 问题解答

比较简单的一道题，问题描述已经讲得很清楚了，代码实现出**判断自整除数的逻辑**，并注意**输入参数的边界范围**与**除数为零**的情况就可以啦。

```java
import java.util.List;
import java.util.ArrayList;
public class Solution {
    /**
     * 根据输入上线界返回自我整除数列表。
     * @param left 下界
     * @param right 上界
     * @return List<Integer> 结果列表
     */
    public List<Integer> selfDividingNumbers(int left, int right) {
        /* 创建结果列表 */
        List<Integer> list = new ArrayList<Integer>();
        /* 包括上下边界值 */
        for (int i = left; i <= right; ++i) {
            /* 判断是否为自整除数 */
            if(isSelfDividingNumber(i)) {
                /* 是 -> 加入列表 */
                list.add(i);
            }
        }
        /* 返回结果列表 */
        return list;
    }
    /**
     * 判断传入整数是否为自整除数。
     * @param num 待判断整数
     * @return boolean 是否为自整除数
     */
    private boolean isSelfDividingNumber(int num) {
        /* 创建副本 */
        int numCopy = num;
        /* 创建余数 */
        int remainder = 0;
        while (numCopy > 0) {
            /* 取余（基数为10） */
            remainder = numCopy % 10;
            /* 排除零值 */
            if (remainder == 0) {
                return false;
            }
            /* 是否整除 */
            if (num % remainder != 0) {
                return false;
            }
            /* 迭代下一位 */
            numCopy /= 10;
        }
        /* 符合定义：判断是自整除数 */
        return true;
    }
}
/* EOF */
```
> 代码清单：自整除数（`Java`实现）

# 参考资料

- 「LeetCode（Self Dividing Numbers）」：https://leetcode.com/problems/self-dividing-numbers/

