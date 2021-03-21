---
title: LeetCode - 709. 转换小写（To Lower Case）
date: 2018-11-10
catalog: true
tags: ["LeetCode", "Algorithm"]
---

# 前言

本文记录*LeetCode - 709. 转换小写（To Lower Case）*问题。

# 问题描述

请你实现功能函数`toLowerCase()`：接受一枚**字符串**参数，将该字符串转换为**小写形式**后返回。

具体样例如下：

```
Input: "Hello"
Output: "hello"
```
> 注：样例1

```
Input: "here"
Output: "here"
```
> 注：样例2

```
Input: "LOVELY"
Output: "lovely"
```
> 注：样例3

# 问题解答

我们知道，计算机中 ASCII 编码使用`8-Bit`数字映射，英文大小写字符都是使用数字进行表示的，大小写对应字符之差为`32`，正是空格符的数字映射。

> 注：参看`man ascii`了解更多内容。

```c
#include <stdio.h>
int main(void) {
    char c = 0;
    /* 打印大写字符及其数字形式 */
    for(c = 'A'; c != 'Z' + 1; ++c) {
        printf("%c -> %d\n", c, c);
    }
    /* 打印小写字符及其数字形式 */
    for(c = 'a'; c != 'z' + 1; ++c) {
        printf("%c -> %d\n", c, c);
    }
    /* 打印对应大小写字符之差 */
    printf("%c -> %d\n", 'a' - 'A', 'z' - 'Z');
    return 0;
}
```
> 代码清单：`C/C++`打印字母表

明白了字母与数字之间的映射关系，字母大小写转换就非常容易了，加减空白符`32`即可实现字符大小写转换。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
/* 定义宏函数 */
#define IS_UPPER(c) \
    ((c) >= 65 && (c) <= 90)
#define TO_LOWER(c) \
    ((c) + 32)
/* 函数声明 */
char* toLowerCase(char* str);
/* 函数定义 */
char* toLowerCase(char* str) {
    /* 分配字符串内存 */
    char *p = (char*)malloc(sizeof(*str) * strlen(str) + 1);
    char *i = NULL, *j = NULL;
    /* 字符拷贝 */
    for(i = str, j = p; *i != '\0'; ++i, ++j) {
        if(IS_UPPER(*i)) {
            *j = TO_LOWER(*i);
        } else {
            *j = *i;
        }
    }
    /* 添加字符串结束符`\0` */
    *j = '\0';
    return p;
}
```
> 代码清单：`C/C++`实现`toLowerCase()`

```java
import java.lang.*;
class Solution {
    public String toLowerCase(String str) {
        StringBuilder b = new StringBuilder();
        for(Character c: str.toCharArray()) {
            // is upper letter
            if (c >= 65 && c <= 90) {
                // convert to lower letter
                b.append((char)(c + 32));
            } else {
                b.append(c);
            }
        }
        return b.toString();
    }
}
```
> 代码清单：`Java`实现`toLowerCase()`

```python
class Solution(object):
    def toLowerCase(self, str):
        """
        :type str: str
        :rtype: str
        """
        ret = ""
        for c in str:
            if(ord(c) >= 65 and ord(c) <= 90):
                ret += chr(ord(c) + 32)
            else:
                ret += c
        return ret
```
> 代码清单：`Python`实现`toLowerCase()`

因为要遍历整个字符串，算法复杂度为：$O(n)$。

# 拓展延伸

实现了功能函数`toLowerCase()`后，我们来考虑实现`toUpperCase()`。原理是相同的，找出字符串中的小写字母，将其转换为大写形式即可。

# 参考资料

- LeetCode: https://leetcode.com/problems/to-lower-case/

