---
title: LeetCode - 101. 判断对称二叉树
subtitle: LeetCode - 101. Symmetric Tree
catalog: true
date: 2019-07-24
tags: ["LeetCode", "Data Structure", "Algorithm", "BinaryTree"]
---

# 前言

本文记录 *LeetCode - 101. 判断对称二叉树（Symmetric Tree）* 问题。

# 问题描述

给出一颗二叉树（Binary Tree），请你判断其是否为对称二叉树（Symmetric Tree）。

> 对称二叉树：以根节点`root`为中心，左右对称的二叉树。

举例说明，二叉树`[1, 2, 2, 3, 4, 4, 3]`是一颗对称二叉树。

```plain
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
> 样例：对称二叉树

但是，二叉树`[1, 2, 2, null, 3, null, 3]`不是一颗对称二叉树。

```plain
    1
   / \
  2   2
   \   \
   3    3
```
> 样例：非对称二叉树

# 问题解答

使用递归法（Recursive）解决。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null || isSymmetric0(root.left, root.right);
    }
    private boolean isSymmetric0(TreeNode leftNode, TreeNode rightNode) {
        if (leftNode == null || rightNode == null) {
            return leftNode == rightNode;
        }
        if (leftNode.val != rightNode.val) {
            return false;
        }
        return isSymmetric0(leftNode.left, rightNode.right) &&
               isSymmetric0(leftNode.right, rightNode.left);
    }
}
```
> 代码清单：判断对称二叉树（Symmetric Tree）`Java`实现

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        return root == None or self.isSymmetric0(root.left, root.right)
    def isSymmetric0(self, leftNode: TreeNode, rightNode: TreeNode) -> bool:
        if (leftNode == None or rightNode == None):
            return leftNode == rightNode
        if (leftNode.val != rightNode.val):
            return False
        return self.isSymmetric0(leftNode.left, rightNode.right) and \
               self.isSymmetric0(leftNode.right, rightNode.left)
```
> 代码清单：判断对称二叉树（Symmetric Tree）`Python`实现

# 参考资料

- LeetCode: https://leetcode.com/problems/symmetric-tree/

