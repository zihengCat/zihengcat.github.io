---
title: 整型数转换字符串 - Java 实现
subtitle: Integer to String - Java Implementation
catalog: true
date: 2019-08-18
tags: ["Java", "Algorithm"]
---

# 前言

今天看到一个面试问题：**一个整型数如何转换为字符串，请手动实现一下**。

# 整型数转换字符串

整数转字符串正是`C`语言标准库`itoa()`函数，要求不使用语言标准库，手写实现该函数。思路非常简单：**整数对其基数（Base）取余得到余数（Remainder），余数加上`'0'`（ASCII 码：48）转为字符型后拼接字符串，直至除尽。**

```java
public class IntegerToString {
    public static String itoa(int num, int base) {
        /* 处理`0`值 */
        if (num == 0) {
            return "0";
        }
        String ret = "";
        int flag = 0;
        int remainder = 0;
        /* 处理负值 */
        if (num < 0) {
            num *= -1;
            flag = -1;
        }
        /* 取余数转字符拼接 */
        while (num > 0) {
            remainder = num % base;
            num /= base;
            /* '0' -> 48 */
            ret = (char)(remainder + '0') + ret;
        }
        return flag == 0 ? ret : '-' + ret;
    }
}
/* EOF */
```
> 代码清单：整型数转字符串`itoa()` - `Java` 实现

<!-- EOF -->
