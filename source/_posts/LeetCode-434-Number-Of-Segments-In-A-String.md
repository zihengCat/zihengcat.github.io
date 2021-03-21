---
title: LeetCode - 434. 统计字符串字符段（Number of Segments in a String）
date: 2018-11-19
catalog: true
tags: ["LeetCode", "Data Structure", "Algorithm"]
---

# 前言

本文记录*LeetCode - 434. 统计字符串数据段（Number of Segments in a String）*问题。

# 问题描述

对字符串中字符段数量统计计数，字符段的定义是：一段连续不为空白符的字符序列。

> 注：输入字符串中不包含任何不可打印（non-printable）的字符。

举例说明：

```plain
Input: "Hello, my name is John"
Output: 5
```
> 注：程序输入输出样例

# 问题解答

```c
/* Micro Inline Function */
#define IS_WHITE_CHARS(c) \
    ((c) == ' ')
int countSegments(char* s) {
    int i = 0, count  = 0;
    while(*s != '\0') {
        if(!IS_WHITE_CHARS(*s) &&
          (IS_WHITE_CHARS(*(s+1)) || *(s+1) == '\0'))
        {
            ++count;
        }
        ++s;
    }
    return count;
}
```
> 代码清单：统计字符串字符段`C/C++`实现

```java
class Solution {
    public int countSegments(String s) {
        int cnt = 0;
        for(int i = 0; i < s.length(); ++i) {
            if((s.charAt(i) != ' ') &&
               (i == 0 || s.charAt(i - 1) == ' ')) {
                ++cnt;
            }
        }
        return cnt;
    }
}
```
> 代码清单：统计字符串字符段`Java`实现

# 参考资料

- LeetCode: https://leetcode.com/problems/number-of-segments-in-a-string/

