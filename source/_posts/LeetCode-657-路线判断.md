---
title: LeetCode - 657. 路线判断（Judge Route Circle）
date: 2018-02-25
tags: [LeetCode, Algorithm, Data Structure]
---

# 前言

本文记录*LeetCode - 657. 路线判断（Judge Route Circle）*问题。

# 问题描述

一个机器人初始位置在$(0, 0)$。输入一系列移动指令使机器人移动起来。判断机器人最终是否回到了初始位置。
输入的移动命令是一串字符串。每个移动指令是一个字符。机器人只按照`R`（右），`L`（左），`U`（上），`D`（下）四个合法指令移动。输出为一个布尔值，判断机器人是否走回到了原点（`true`/`false`）。

**样例1：**
```
输入: "UD"
输出: true
```
**样例2：**
```
输入: "LL"
输出: false
```

# 问题解法

设置初始坐标为$(0, 0)$，遍历输入指令字符串，如果是合法指令就改变坐标，指令结束后判断改变后再坐标与$（0, 0）$点是否相等即可。

```
/*
 *
 * 输入：指令字符串
 * 输出：布尔值
 */
bool judgeCircle(char* moves) {
    int posX = 0, posY = 0;
    while(*moves) {
        switch(*moves) {
            case 'R':
                posX += 1;
                break;
            case 'L':
                posX -= 1;
                break;
            case 'U':
                posY += 1;
                break;
            case 'D':
                posY -= 1;
                break;
            default:
                break;
        }
        moves++;
    }
    return posX == 0 && posY == 0;
}
```

> 代码清单：C/C++ 代码

