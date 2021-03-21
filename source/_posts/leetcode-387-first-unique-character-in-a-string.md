---
title: LeetCode - 387. 寻找字符串首个唯一字符
subtitle: LeetCode - 387. First Unique Character in a String
catalog: true
date: 2019-09-09
tags: ["LeetCode", "Data Structure", "String"]
---

# 前言

本文记录 *LeetCode - 387. 字符串首个唯一字符（First Unique Character in a String）* 问题。

# 问题描述

给出一枚字符串，请寻找字符串中第一个不重复字符，返回该字符在字符串中的索引下标。如果字符不存在，返回`-1`。

```plain
输入: "leetcode"
输出: 0
```
> 注：示例 1

```plain
输入: "loveleetcode"
输出: 2
```
> 注：示例 2

注意，可以假定输入字符串仅包含英文小写字符。

# 问题解答

字符串问题，寻找字符串中第一个不重复字符。很容易想到的思路是：**对于字符串中的每一个字符，再次遍历字符串判断该字符是否重复，无重复则返回字符索引下标**。

```java
public class Solution {
    public int firstUniqChar(String s) {
        char[] arr = s.toCharArray();
        for (int i = 0; i < arr.length; ++i) {
            if (findCharacter(arr[i], arr, i + 1, arr.length - 1) == -1 &&
                findCharacter(arr[i], arr, 0, i - 1) == -1) {
                return i;
            }
        }
        return -1;
    }
    private static int findCharacter(char c, char[] arr, int left, int right) {
        for (int i = left; i <= right; ++i) {
            if (arr[i] == c) {
                return i;
            }
        }
        return -1;
    }
}
/* EOF */
```
> 代码清单：寻找字符串首个唯一字符（`Java`实现）

但是，遍历两遍字符串的算法效率不高：$O(n^2)$。

另一种实现思路是：**引入「有序哈希表」，存储字符串中每个字符的出现次数，只需遍历字符串一遍，即可得到结果，算法复杂度为：$O(n + m)$**。

```java
import java.util.Map;
import java.util.LinkedHashMap;
public class Solution {
    public int firstUniqChar(String s) {
        Map<Character, Integer> map = new LinkedHashMap<Character, Integer>();
        char[] arr = s.toCharArray();
        for (int i = 0; i < arr.length; ++i) {
            map.put(arr[i],
                map.get(arr[i]) == null ? 1 : map.get(arr[i]) + 1
            );
        }
        for (Character c : map.keySet()) {
            if (map.get(c) == 1) {
                return s.indexOf(c);
            }
        }
        return -1;
    }
}
/* EOF */
```
> 代码清单：寻找字符串首个唯一字符（`Java`实现）

# 参考资料

- 「LeetCode（First Unique Character in a String）」：https://leetcode.com/problems/first-unique-character-in-a-string/

<!-- EOF -->
