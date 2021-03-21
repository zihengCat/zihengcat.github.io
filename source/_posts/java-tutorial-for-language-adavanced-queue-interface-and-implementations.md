---
title: Java 高级教程系列 - Queue 接口及其实现
subtitle: Java Tutorial for Language Adavanced - Queue Interface and Implementations
catalog: true
date: 2019-05-05
tags: ["Java", "Java Tutorial"]
---

# Java 集合框架 `Queue` 接口

Java 集合框架中的`Queue`接口，模拟数据结构中的队列。队列有入点（Entry Point）与出点（Exit Point），队列的入点被称为队尾（Tail），队列的出点被称为队头（Head），队列元素从队尾进入，从队头流出，队列是一种**先进先出（FIFO）**的线性表数据结构。

队列一般包含三项基本操作：

- 从队尾添加元素（入队）

- 从队头移出元素（出队）

- 查看队头首元素（检查）

Java 集合框架`Queue`接口定义了`FIFO`队列的基本操作方法，并为每项操作分别定义两种方法：一种方法在会在操作失败时抛出异常，另一方法则会在操作失败时返回一个特殊值（`false`或`null`）标识操作失败。

```java
public interface Queue<E>
extends Collection<E>
```
> 代码清单：`Queue`接口声明

下面对`Queue`接口方法作一个整理。

| 操作 | 方法                 | 描述                                             |
| ---- | -------------------- | ------------------------------------------------ |
| 入队 | `boolean add(E e)`   | 将一枚元素添加到队尾，失败抛出异常。             |
| 入队 | `boolean offer(E e)` | 将一枚元素添加到队尾，失败返回`false`。          |
| 出队 | `E remove()`         | 从队头取出一枚元素，失败抛出异常。               |
| 出队 | `E poll()`           | 从队头取出一枚元素，失败或队列为空时返回`null`。 |
| 检查 | `E element()`        | 查看队头元素，失败或队列为空时抛出异常。         |
| 检查 | `E peek()`           | 查看队头元素，失败或队列为空时返回`null`。       |

> 表：`Queue`接口方法表

# `Queue` 接口实现类

Java 集合框架对`Queue`接口有几类实现：数组（Resizable Array）、链表（Linked List）、堆（Heap）。

- `ArrayDeque`：数组实现

- `LinkedList`：链表实现

- `PriorityQueue`：堆实现

# Java 集合 `Queue` 使用示例

```java
import java.util.Queue;
import java.util.ArrayDeque;
import java.util.LinkedList;
public class Main {
    public static void main(String[] args) {
        /* Create a queue of strings */
        Queue<String> aQueue = new LinkedList<String>();
        /* Print queue details */
        showQueue("Queue", aQueue);
        /* offer() work the same as add() */
        aQueue.offer("Java");
        aQueue.offer("Python");
        aQueue.offer("JavaScript");
        aQueue.offer("C/C++");
        /* Print queue details */
        showQueue("Queue", aQueue);
        /* Remove element from queue head */
        aQueue.poll();
        /* Print queue details */
        showQueue("Queue", aQueue);
        /* Add element from queue tail */
        aQueue.offer("Golang");
        /* Print queue details */
        showQueue("Queue", aQueue);
        System.out.println("Queue.peek(): ");
        /* Use queue peek() method */
        while (!aQueue.isEmpty()) {
            String e = aQueue.peek();
            System.out.println(e);
            aQueue.poll();
        }
    }
    public static <T> void showQueue(String name, Queue<T> queue) {
        System.out.printf(
            "%s(%d): %s" +
            System.getProperty("line.separator"),
            name, queue.size(), queue.toString()
        );
    }
}
```
> 代码清单：`Queue`使用示例

# 关联内容（Related Topics）

- [Java 高级教程系列 - Collection 接口与 Iterator 迭代器](https://zihengcat.github.io/2019/05/02/java-tutorial-for-language-adavanced-collection-and-iterator/)

# 参考资料（References）

- OpenJDK Documents for `Queue`: https://devdocs.io/openjdk~8/java/util/queue

