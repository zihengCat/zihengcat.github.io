---
title: LeetCode - 461. 汉明距离（Hamming distance）
date: 2018-02-24
tags: [LeetCode, Algorithm, Data Structure]
---

# 前言

本文记录*LeetCode - 461. 汉明距离（Hamming distance）*问题。

两个等长字符串之间的汉明距离（英语：Hamming distance）是指两个字符串对应位置不同字符的个数。

> 维基百科：https://zh.wikipedia.org/wiki/%E6%B1%89%E6%98%8E%E8%B7%9D%E7%A6%BB

# 问题描述

两个整型数的汉明距离是指：这两个整型数对应比特位不同值的个数。
给出两个整型数`x`与`y`，计算它们的汉明距离。

> 整型范围：$[0, 2^{31}]$

举例说明：
```
输入：x = 1, y = 4

输出：2

解释：
-------------
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
-------------
注意箭头所指比特位，位置相同值不同。
```

# 问题解法

输入的整型数均为`int`类型，这可以免去考虑整型数比特数不相同的情况。
先使用异或*XOR*操作，问题转化为计算得到数中比特值为`1`的个数。
判断最右比特位是否为`1`，循环移位即可。

```
int hammingDistance(int x, int y) {
    int ret = x ^ y;
    int cnt = 0;
    while(ret) {
        cnt += ret & 0x01;
        ret >>= 1;
    }
    return cnt;
}
```
> 代码清单：`C/C++`代码实现

