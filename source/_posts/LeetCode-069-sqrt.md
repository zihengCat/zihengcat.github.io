---
title: LeetCode - 069. 求平方根（Sqrt）
date: 2018-03-07
tags: [Algorithm, LeetCode]
---

# 前言

本文记录*LeetCode - 069. 求平方根（Sqrt）*问题。

# 问题描述

请你手动编程实现`sqrt()`函数：输入一个整型数，求该整数的平方根。

> 注：给出的整型数保证大于`0`。

```
输入：4
输出：2
```
> 注：样例1

```
输入：8
输出：2
解释：8 的平方根为 2.82842... 取其整数部分。
```
> 注：样例2

# 问题解答

平方根函数`sqrt()`是最常用的数学函数之一，在这个问题中，需要我们手动实现`sqrt`函数，而不应使用语言标准库自带的数学函数。

## 二分求解法

第一个思路是使用**二分法**，猜测一个数，平方后与原数进行比对，确定一个精度范围后即可求解。

```
/* 绝对值函数 */
#define ABS(x) \
    ((x) < 0 ? -(x) : (x))
/* 设置精度 */
#define LIMIT 0.01
int mySqrt(int x) {
    /* 处理非正数情况 */
    if(x <= 0) { return 0; }
    double hig = double(x);
    double low = 0;
    double lst = 0;
    double guess = (low + hig)/2;
    do {
       lst = guess;
       if(guess * guess > x) {
           hig = guess;
           guess = (low + hig)/2;
       }
       else if(guess * guess < x) {
           low = guess;
           guess = (low + hig)/2;
       }
       else if(guess * guess == x) {
           break;
       }
    } while(ABS(guess - lst) > LIMIT);

    return (int)guess;
}
```
> 代码清单：`C/C++`实现二分法

## 牛顿迭代法

另一种方法就是使用**牛顿迭代法**，要求根号`n`的近似值，先随便猜测一个数`x`，然后**不断令`x`等于`x`和`a/x`的平均数**，迭代六七次后，结果就已经相当精确了。

```c
/* 绝对值函数 */
#define ABS(x) \
    ((x) < 0 ? -(x) : (x))
/* 设置精度 */
#define LIMIT 0.01
int mySqrt(int x) {
    /* 处理非正数情况 */
    if(x <= 0) { return 0; }
    double guess = x;
    double last  = 0;
    do {
        last  = guess;
        guess = (guess + guess/x)/2;
    } while(ABS(guess - last) > LIMIT);
    return (int)guess;
}
```
> 代码清单：`C/C++`实现牛顿迭代法

# 参考资料

- 一个 sqrt 函数引发的血案: http://diducoder.com/sotry-about-sqrt.html

