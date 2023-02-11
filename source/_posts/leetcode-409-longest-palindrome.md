---
title: LeetCode - 409. 最大可构建的回文串
subtitle: LeetCode - 409. Longest Palindrome
catalog: true
date: 2021-06-30
tags: ["LeetCode", "String", "Hash Table"]
---

# 前言

本文记录**LeetCode - 409. 最大可构建的回文串**问题。

# 描述

给出一枚包含英文大小写字符的字符串`s`，请使用该字符串包含的字符构建回文串，返回**最大**可构建回文串的长度。

字符区分大小写，例如：`Aa`不被认为是回文串。

```plain
输入：s = "abccccdd"
输出：7
解释：
其中一枚最大可构建的回文串是"dccaccd"，最大可构建回文串的长度为7。
```
> 示例 1

```plain
输入：s = "a"
输出：1
```
> 示例 2

```plain
输入：s = "bb"
输出：2
```
> 示例 3

约束条件：

- `1 <= s.length <= 2000`

- 字符串`s`仅包含英文大小写字符。

# 解法

考虑回文串的特点：两枚相同的字符可配对出一对回文串，单个字符也可独立成为一枚回文串。解题思路即是：寻找字符串中有多少对可配对的回文串，到最后配无可配，即得到了最大可构建的回文串，按题目要求返回最大回文串的长度。

遍历字符串，使用一枚哈希表记录下等待配对的字符，发现配对成功，将配对成功的字符从哈希表中移除，遍历完成就找全了所有可配对的回文串。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$

## 代码

```cpp
#include <string>
#include <unordered_set>

using namespace std;

class Solution {
public:
    int longestPalindrome(string s) {
        int cnt = 0;
        unordered_set<char> hset;
        unordered_set<char>::iterator iter;
        int n = s.size();
        for (int i = 0; i < n; i++) {
            iter = hset.find(s[i]);
            if (iter != hset.end()) {
                hset.erase(iter);
                cnt++;
            } else {
                hset.insert(s[i]);
            }
        }
        return hset.size() > 0
            ? cnt * 2 + 1
            : cnt * 2;
    }
};
/* EOF */
```
> 代码片段：C++

```java
import java.util.Set;
import java.util.HashSet;

class Solution {
    public int longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int cnt = 0;
        Set<Character> hSet = new HashSet<>();
        char c = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            c = s.charAt(i);
            if (hSet.contains(c)) {
                hSet.remove(c);
                cnt++;
            } else {
                hSet.add(c);
            }
        }
        return hSet.size() > 0
            ? 2 * cnt + 1
            : 2 * cnt;
    }
}
/* EOF */
```
> 代码片段：Java

```go
func longestPalindrome(s string) int {
    cnt := 0
    hset := make(map[rune]bool)
    for _, c := range s {
        if _, ok := hset[c]; ok {
            cnt++
            delete(hset, c)
        } else {
            hset[c] = true
        }
    }
    var n int = cnt * 2
    if len(hset) > 0 {
        n += 1
    }
    return n
}
// EOF
```
> 代码片段：Go

```typescript
function longestPalindrome(s: string): number {
    let cnt: number = 0;
    let hset: Set<string> = new Set<string>();
    let c: string = "";
    for (let i: number = 0; i < s.length; i++) {
        c = s.charAt(i);
        if (hset.has(c)) {
            hset.delete(c);
            cnt++;
        } else {
            hset.add(c);
        }
    }
    return hset.size > 0
        ? cnt * 2 + 1
        : cnt * 2;
};
// EOF
```
> 代码片段：TypeScript

```python
from typing import Set

class Solution:
    def longestPalindrome(self, s: str) -> int:
        cnt: int = 0
        hset: Set[str] = set()
        for c in s:
            if c in hset:
                hset.remove(c)
                cnt += 1
            else:
                hset.add(c)
        n: int = cnt * 2
        if len(hset) > 0:
           n += 1
        return n
# EOF
```
> 代码片段：Python

# 参考资料

- LeetCode 409. Longest Palindrome: https://leetcode.com/problems/longest-palindrome/

<!-- EOF -->

