---
title: LeetCode - 476. 寻找补数
subtitle: LeetCode - 476. Number Complement
catalog: true
date: 2019-07-23
tags: ["LeetCode", "Algorithm"]
---

# 前言

本文记录 *LeetCode - 476. 寻找补数（Number Complement）* 问题。

# 问题描述

给出一个正整数，请你寻找它的补数。某数的补数可以定义为：比特位反转的正整数。

```plain
Input: 5
Output: 2
Explanation: 5 的二进制表示形式为 101（无前导0），其补数为 010，输出即 2。
```
> 样例 1

```plain
Input: 1
Output: 0
Explanation: 1 的二进制表示形式为1（无前导0），其补数为 0，输出即 0。
```
> 样例 2

注意：

- 给出正整数的取值范围保证在`32-bit`带符号数之内。

- 可以假定整型数的二进制形式无前导`0`。

# 问题解答

解决该问题的第一种思路是：**将整数二进制形式与字符串之间来回转换。**先将输入整数转化为二进制字符串形式，反转字符串中的比特位，最后将修改后的字符串再转回二进制整数。

```java
class Solution {
    public int findComplement(int num) {
        String binaryNumString = Integer.toBinaryString(num);
        char[] binaryNumStringCharArray = binaryNumString.toCharArray();
        for (int i = 0; i < binaryNumStringCharArray.length; ++i) {
            if (binaryNumStringCharArray[i] == '0') {
                binaryNumStringCharArray[i] = '1';
            } else if (binaryNumStringCharArray[i] == '1') {
                binaryNumStringCharArray[i] = '0';
            }
        }
        return Integer.parseInt(
            String.valueOf(binaryNumStringCharArray), 2
        );
    }
}
```
> 代码请单：寻找补数（Number Complement）`Java`实现

另一种思路是：**使用位操作。**取得输入整数的最高比特位（`LEFTMOST` bit of `1`），减`1`获得掩码（Mask），再将原数按位取反后与掩码做一个按位与`&`操作，即可得到结果补数。毫无疑问，使用位操作算法效率更高。

```plain
num(5)  -> 00000101            = 00000101
mask(5) -> 00000100 - 00000001 = 00000011
~num(5)                        = 11111010
~num(5) & mask(5)              = 00000010
```
> 注：位操作寻找补数（以`5`为例）

```java
class Solution {
    public int findComplement(int num) {
        return ~num & (Integer.highestOneBit(num) - 1);
    }
}
```
> 代码请单：寻找补数（Number Complement）`Java`实现

# 参考资料

- LeetCode: https://leetcode.com/problems/number-complement/

