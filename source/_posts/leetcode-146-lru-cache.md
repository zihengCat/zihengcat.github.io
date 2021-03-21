---
title: LeetCode - 146. 设计 LRU 缓存
subtitle: LeetCode - 146. LRU Cache
catalog: true
date: 2019-08-19
tags: ["LeetCode", "Data Structure", "Algorithm", "LinkedList", "HashMap"]
---

# 前言

本文记录 *LeetCode - 146. 设计 LRU 缓存（LRU Cache）* 问题。

# 问题描述

请你设计并实现“最近最少使用”（Least Recently Used, LRU）缓存，支持如下操作：`get()`与`put()`。

- `get(key)`：根据键获取值。如果该键位于缓存中，返回该键对应的值，否则返回`-1`。

- `put(key, value)`：设置缓存键值对。如果该键已存在缓存中，更新其值，如果不存在，直接插入。如果缓存容量达到上限，淘汰最近最少使用的键值对。

- 缓存容量初始化为：正整数。

- 获取与设置缓存两种操作的算法复杂度为：$O(1)$。

```plain
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```
> 例：样例说明

# 问题解答

LRU（Least Recently Used，最近最少使用）是一种缓存算法，根据数据的历史访问记录来淘汰数据，其核心思想是：**如果数据最近被访问过，那么将来被访问的几率也更高**。

该题要求我们设计一个 LRU 缓存，支持缓存数据的获取与设置，缓存容量达到上限时，淘汰**最近最少使用**的缓存数据。题目还要求，缓存数据的获取与设置算法复杂度应该为$O(1)$。

为了实现获取与设置算法复杂度均为$O(1)$的 LRU 缓存，需要**双向链表（LinkedList）+ 哈希表（HashMap）**两种数据结构配合使用。哈希表用于以$O(1)$的算法复杂度获取与插入数据，双向链表使原本无序的哈希表有序排列，最新插入与访问的数据会被移动到双向链表头部`head`，缓存容量满时淘汰数据即淘汰链表尾部`tail`数据。

```java
import java.util.Map;
import java.util.HashMap;
/**
 * LRU 缓存
 */
public class LRUCache {
    /* 内部缓存数据节点 */
    private class DLNode {
        /* 键值对 -> 键 */
        private int key;
        /* 键值对 -> 值 */
        private int val;
        /* 前驱节点 */
        private DLNode prev;
        /* 后继节点 */
        private DLNode next;
        /* 构造函数 */
        public DLNode() {}
        public DLNode(int key, int val) {
            this(key, val, null, null);
        }
        public DLNode(int key, int val, DLNode prev, DLNode next) {
            this.key = key;
            this.val = val;
            this.prev = prev;
            this.next = next;
        }
        /* Setter & Getter */
        public int getKey() {
            return this.key;
        }
        public void setKey(int key) {
            this.key = key;
        }
        public int getVal() {
            return this.val;
        }
        public void setVal(int val) {
            this.val = val;
        }
        public DLNode getPrev() {
            return this.prev;
        }
        public void setPrev(DLNode prev) {
            this.prev = prev;
        }
        public DLNode getNext() {
            return this.next;
        }
        public void setNext(DLNode next) {
            this.next = next;
        }
    }
    /* 哈希表数据结构 */
    private Map<Integer, DLNode> cache;
    /* 缓存容量 */
    private int capacity;
    /* 双向链表头节点 */
    private DLNode head;
    /* 双向链表尾节点 */
    private DLNode tail;
    /**
     * 构造函数
     * @param capacity
     */
    public LRUCache(int capacity) {
        /* 初始化缓存容量 */
        this.capacity = capacity;
        /* 初始化哈希表 */
        this.cache = new HashMap<Integer, DLNode>(capacity);
        /* 初始化双向链表头尾节点 */
        this.head = new DLNode();
        this.tail = new DLNode();
        /* 连接头尾节点 */
        head.setNext(tail);
        tail.setPrev(head);
    }
    /**
     * 获取缓存值，缓存未命中返回{@code -1}。
     * @param key
     * @return value
     */
    public int get(int key) {
        DLNode node = cache.get(key);
        if (null == node) {
            /* 无缓存命中 */
            return -1;
        }
        /* 移动节点至双向链表头部 */
        moveToHead(node);
        /* 缓存命中 */
        return node.getVal();
    }
    /**
     * 设置/更新缓存。
     * @param key
     * @param value
     * @return void
     */
    public void put(int key, int value) {
        DLNode node = cache.get(key);
        /* 缓存节点已存在 -> 更新数据
           缓存节点不存在 -> 插入数据 */
        if (null == node) {
            /* 缓存容量已满，淘汰数据 */
            if (cache.size() >= capacity) {
                /* 移除哈希表/双向链表尾部缓存数据节点 */
                removeTail();
            }
            /* 建立新缓存数据节点 */
            DLNode newNode = new DLNode(key, value);
            /* 加入到链表头部 */
            addToHead(newNode);
        } else {
            /* 更新节点数据 */
            node.setVal(value);
            /* 移动节点至双向链表头部 */
            moveToHead(node);
        }
    }
    /**
     * 将缓存数据节点移动到链表头部。
     * @param node
     * @return void
     */
    private void moveToHead(DLNode node) {
        removeNode(node);
        addToHead(node);
    }
    /**
     * 从链表/哈希表中移除缓存数据节点。
     * @param node
     * @return void
     */
    private void removeNode(DLNode node) {
        /* 从双向链表中移除节点 */
        DLNode prev = node.getPrev();
        DLNode next = node.getNext();
        prev.setNext(next);
        next.setPrev(prev);
        node.setPrev(null);
        node.setNext(null);
        /* 从哈希表中移除节点 */
        cache.remove(node.getKey());
    }
    /**
     * 将缓存数据节点加入到链表/哈希表。
     * @param node
     * @return void
     */
    private void addToHead(DLNode node) {
        /* 插入到双向链表头部 */
        node.setPrev(head);
        node.setNext(head.getNext());
        head.getNext().setPrev(node);
        head.setNext(node);
        /* 插入或更新哈希表 */
        cache.put(node.getKey(), node);
    }
    /**
     * 移除链表尾部缓存数据节点。
     * @param void
     * @return void
     */
    private void removeTail() {
        /* 移除链表尾部节点 */
        removeNode(tail.getPrev());
    }
}
/* EOF */
```
> 代码清单：LRU 缓存（`Java`实现）

# 参考资料

- 「漫画：什么是 LRU 算法？」：https://www.tuicool.com/articles/yyu2quN

- 「LeetCode（LRU Cache）」：https://leetcode.com/problems/lru-cache/

