---
title: LeetCode - 125. 判断回文串
subtitle: LeetCode - 125. Valid Palindrome
catalog: true
date: 2019-05-21
tags: ["LeetCode", "Data Structure", "Algorithm", "Array", "String"]
---

# 前言

本文记录 *LeetCode - 125. 判断回文串（Valid Palindrome）* 问题。

# 问题描述

给出一枚字符串，判断其是否为回文字符串，只考虑字符串中的字母与数字字符，并忽略字母大小写。

注意：为了符合题意，我们定义空字符串`""`也属于回文串。

```plain
Input: "A man, a plan, a canal: Panama"
Output: true
```
> 注：样例1

```plain
Input: "race a car"
Output: false
```
> 注：样例2

```plain
Input: " "
Output: true
```
> 注：样例3

# 问题解答

回文字符串（英文：Palindrome）的含义为：从头至尾遍历（顺序）和从头至尾遍历（逆序）都相同的字符串。

该题让我们判断，输入字符串是否为回文串，我们只需要按照**回文串定义**进行判断即可。但是需要考虑到的是，输入字符串中可能包含一些非字母数字字符，也要忽略字母大小写，所以执行实际回文判断之前，我们需要先**过滤清洗**输入字符串。

```java
class Solution {
    public boolean isPalindrome(String s) {
        String actual = s.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
        String reversed = new StringBuffer(actual).reverse().toString();
        return actual.equals(reversed);
    }
}
```
> 代码清单：判断回文串（`Java`实现）

我们可以在不借助标准库函数的情况下，手动实现出判断回文串的代码逻辑。

```java
class Solution {
    public boolean isPalindrome(String s) {
        char[] arr = s.toCharArray();
        int head = 0;
        int tail = arr.length - 1;
        char cHead, cTail;
        while (head <= tail) {
            cHead = arr[head];
            cTail = arr[tail];
            if (!Character.isLetterOrDigit(cHead)) {
                ++head;
            } else if (!Character.isLetterOrDigit(cTail)) {
                --tail;
            } else {
                if (Character.toLowerCase(cHead) !=
                    Character.toLowerCase(cTail)) {
                    return false;
                }
                ++head;
                --tail;
            }
        }
        return true;
    }
}
```
> 代码清单：判断回文串（`Java`实现）

# 参考资料

- LeetCode: https://leetcode.com/problems/valid-palindrome/

