---
title: LeetCode - 049. 相同字母异序词归类
subtitle: LeetCode - 049. Group Anagrams
catalog: true
date: 2019-05-13
tags: ["LeetCode", "Data Structure", "Algorithm", "Array", "HashMap"]
---

# 前言

本文记录 *LeetCode - 049. 相同字母异序词归类* 问题。

# 问题描述

输入一个字符串数组，请你将数组中的字符串按**相同字母异序词**归类并输出。

```plain
Input: ["eat", "tea", "tan", "ate", "nat", "bat"]
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
> 注：输入输出样例

注意：

- 输入字符串均为小写形式

- 输出归类数组排列顺序无关紧要

# 问题解答

解决这个问题，我们需要使用到键值对（Key-Value）哈希表数据结构。输入字符串数组，要求我们按照**相同字母异序词**将字符串归类，能被归为一类的字符串都具有相同的特征：**排序后字母顺序相同**，利用这一特征，可以很容易构建出哈希表键，哈希表的值即为对应的字符串列表。

![049_group_anagrams_1](./049_group_anagrams_1.png)

> 图：算法图解

```java
import java.util.Arrays;
import java.util.List;
import java.util.LinkedList;
import java.util.Map;
import java.util.HashMap;
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        /* Return empty List<List<String>>
           if input strings array is null or empty */
        if (strs == null || strs.length == 0) {
            return new LinkedList<List<String>>();
        }
        /* Using a HashMap data structure */
        Map<String, List<String>> hashMap =
            new HashMap<String, List<String>>();
        /* For each string in strings array
           do as following... */
        for (String s : strs) {
            /* Convert string to char array */
            char[] chArr = s.toCharArray();
            /* Sort char array in order to get unique HashMap keys */
            Arrays.sort(chArr);
            /* Reconvert sorted char array to HashMap string key */
            String key = new String(chArr);
            if (hashMap.containsKey(key)) {
                /* Found the key ->
                   Add to target list */
                hashMap.get(key).add(s);
            } else {
                /* Not found key ->
                   Build a new list of strings and put it to HashMap */
                List<String> aStringList = new LinkedList<String>();
                aStringList.add(s);
                hashMap.put(key, aStringList);
            }
        }
        /* Convert HashMap values to List<List<String>> */
        return new LinkedList<List<String>>(hashMap.values());
    }
}
```
> 代码清单：相同字母异序词归类（`Java`实现）

```python
from typing import List, Dict
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans: Dict[str, List[str]] = dict()
        for s in strs:
            key: str = "".join(sorted(s))
            if(ans.get(key) == None):
                ans[key] = list()
            ans[key].append(s)
        return list(v for v in ans.values())
```
> 代码清单：相同字母异序词归类（`Python3`实现）

我们来分析一下算法复杂度。

- **时间复杂度**：设$n$为字符串数组的长度（字符串数量），$k$为数组中最长字符串的长度。最外层遍历字符串数组的复杂度为$O(n)$，内部字符串排序的复杂度为$O(k \cdot log(k))$，算法整体时间复杂度为$O(n \cdot k \cdot log(k))$。

- **空间复杂度**：设$n$为字符串数组的长度（字符串数量），$k$为数组中最长字符串的长度。所占用空间即为哈希表所存储的全部内容，空间复杂度为$O(n \cdot k)$。

# 参考资料

- LeetCode: https://leetcode.com/problems/group-anagrams/

