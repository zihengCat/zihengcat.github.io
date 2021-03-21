---
title: LeetCode - 430. 多层双向链表展开
subtitle: LeetCode - 430. Flatten a Multilevel Doubly Linked List
catalog: true
date: 2019-09-02
tags: ["LeetCode", "LinkedList"]
---

# 前言

本文记录 *LeetCode - 430. 多层双向链表展开（Flatten a Multilevel Doubly Linked List）* 问题。

# 问题描述

给出一个多层双向链表：除前驱后继节点指针外，额外还有一枚子节点指针`child`，该指针可能指向另一个双向链表，子链表节点也可能有一个或多个子链表。

请你将多层双向链表展平为单层双向链表，输入输出均为链表首层头节点。

```plain
Input:
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL

Output:
1-2-3-7-8-11-12-9-10-4-5-6-NULL
```
> 注：输入输出样例

![MultilevelLinkedList](./multilevel_linkedlist.png)

> 图：多层双向链表（输入）

![MultilevelLinkedListFlattened](./multilevel_linkedlist_flattened.png)

> 图：展开后单层双向链表（输出）

# 问题解答

输入的是一个异化双向链表，在双向链表数据结构中额外增加一个孩子节点`child`，节点可能指向另一子链表，需要我们将多层双向链表展平为单层。

解决该问题的核心思路：**遍历父链表（最上层链表），一旦发现节点存在子链表，即将子链表与父链表连接起来**。

父子链表连接操作步骤已给出。连接完成后，父链表后继节点变为子链表头节点，子链表尾节点的后继节点变为父链表的原后继节点，遍历继续。

1. 找到子链表「尾节点」

2. 连接子链表「尾节点」与父链表「后继节点」

3. 连接子链表「头节点」与父链表「当前节点」

父链表遍历结束后，多层双向链表就已被展开为单层双向链表，返回展开后的链表头节点。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;

    public Node() {}

    public Node(int _val, Node _prev, Node _next, Node _child) {
        val = _val;
        prev = _prev;
        next = _next;
        child = _child;
    }
}
*/
public class Solution {
    public Node flatten(Node head) {
        if (head == null) {
            return head;
        }
        /* Using a pointer */
        Node p = head;
        while (p != null) {
            /* CASE: the node has child */
            if (p.child != null) {
                Node childNode = p.child;
                /* Find the tail node of child doubly linked list */
                while (childNode.next != null) {
                    childNode = childNode.next;
                }
                /* Connect tail with p.next, if it is not null */
                childNode.next = p.next;
                if (p.next != null) {
                    p.next.prev = childNode;
                }
                /* Connect p with p.child, and remove p.child */
                p.next = p.child;
                p.child.prev = p;
                p.child = null;
            } else {
                /* CASE: if no child, just move forward */
                p = p.next;
            }
        }
        return head;
    }
}
/* EOF */
```
> 代码清单：多层双向链表展开（`Java`实现）

# 参考资料

- 「LeetCode（Flatten a Multilevel Doubly Linked List）」：https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/

<!-- EOF -->
