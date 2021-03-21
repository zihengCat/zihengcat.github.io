---
title: LeetCode - 326. 3的幂次（Power of Three）
date: 2018-04-15
tags: [Algorithm, LeetCode]
---

# 前言

本文记录*LeetCode - 326. 3的幂次（Power of Three）*问题。

# 问题描述

给出一个整型数，请你写个功能函数判断，该数是否是`3`的幂次。结果返回一个布尔值。

例:
```
输入: 4
输出: false

输入: 9
输出: true
```

# 问题解法

只考虑是否为`3`的**非负整数**次幂。

`3`的幂次形如：`3 * 3 * 3 * ... * 3`，均可被`3`无限整除。利用这个特性，我们就可以解决此问题。

```
bool isPowerOfThree(int n) {
    /* 排除掉负数和0 */
    if(n <= 0) {
        return 0;
    }
    /* 设置商和余数 */
    int quotient  = n,
        remainder = 0;
    /* 循环退出条件: 商为1（除尽） */
    while(quotient != 1) {
        remainder = quotient % 3;
        quotient /= 3;
        /* 如果有余数（除不尽） */
        if(remainder) {
            return 0;
        }
    }
    return 1;
}
```

> C/C++ 代码（常规解法）

# 特殊解法（不使用迭代/递归）

