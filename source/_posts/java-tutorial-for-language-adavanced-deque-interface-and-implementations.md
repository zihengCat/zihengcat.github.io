---
title: Java 高级教程系列 - Deque 接口及其实现
subtitle: Java Tutorial for Language Adavanced - Deque Interface and Implementations
catalog: true
date: 2019-05-06
tags: ["Java", "Java Tutorial"]
---

# Java 集合框架 `Deque` 接口

Java 集合框架中的`Deque`接口，其模型定义为：**一种支持从双端插入或移除元素的线性表数据结构**，是对队列结构的扩展，一般被称为双端队列（Double Ended Queue），简称 *Deque* 。

> 注：`Deque`读音为 *deck* ，不为 *de queue* 。

```java
public interface Deque<E>
extends Queue<E>
```
> 代码清单：`Deque`接口声明

Java 集合`Deque`支持从队尾（Tail）或队头（Head）插入/移除元素，定义了操作双端队列的基本方法，为每项操作分别定义两种方法：一种方法在会在操作失败时抛出异常，另一种则会在操作失败时返回一个特殊值（false或null）标识操作失败。

下面对`Deque`接口方法作一个整理，以及对`Deque`与`Queue`方法作个比较。

![summary-of-deque-methods](./summary-of-deque-methods.png)

> 表：`Deque`接口方法表

| `Queue` Method | Equivalent `Deque` Method |
| -------------- | ------------------------- |
| `add(e)`       | `addLast(e)`              |
| `offer(e)`     | `offerLast(e)`            |
| `remove()`     | `removeFirst()`           |
| `poll()`       | `pollFirst()`             |
| `element()`    | `getFirst()`              |
| `peek()`       | `peekFirst()`             |

> 表：`Deque`与`Queue`方法比较表

# `Deque`接口实现类

Java 集合框架对`Deque`接口有几类实现：数组（Resizable Array）、链表（Linked List）。

- `ArrayDeque`：数组实现

- `LinkedList`：链表实现

# Java 集合 `Deque` 使用示例

我们通过一个 *Demo* 代码示例来熟悉`Deque`的操作。

```java
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.LinkedList;
public class Main {
    public static void main(String[] args) {
        /* Create a deque of strings */
        Deque<String> aDeque = new LinkedList<String>();
        /* Print deque details */
        showDeque("Deque", aDeque);
        /* Add a few elements to deque */
        aDeque.add("Java");
        aDeque.add("Python");
        /* Print deque details */
        showDeque("Deque", aDeque);
        /* Insert element from deque head */
        aDeque.offerFirst("JavaScript");
        showDeque("Deque", aDeque);
        /* Insert element from deque tail */
        aDeque.offerLast("C/C++");
        showDeque("Deque", aDeque);
        /* Use deque peek() and poll() method */
        while (!aDeque.isEmpty()) {
            String head = aDeque.peekFirst();
            String tail = aDeque.peekLast();
            System.out.printf(
                "Head = %s, Tail = %s" +
                System.getProperty("line.separator"),
                head, tail
            );
            aDeque.pollFirst();
            aDeque.pollLast();
        }
        /* Print deque details */
        showDeque("Deque", aDeque);
    }
    public static <T> void showDeque(String name, Deque<T> deque) {
        System.out.printf(
            "%s(%d): %s" +
            System.getProperty("line.separator"),
            name, deque.size(), deque.toString()
        );
    }
}
```
> 代码清单：`Deque`使用示例

# `Deque` 用作 `Stack`

`Deque`数据结构也可以被用作`Stack`栈（LIFO），Java 官方也建议开发者使用`Deque`取代旧版`Stack`类。当`Deque`被当作栈使用时，元素都从队头（Head）出入栈。`Deque`也定义了栈的基本操作方法。

| `Stack` Method | Equivalent `Deque` Method |
| -------------- | ------------------------- |
| `push(e)`      | `addFirst(e)`             |
| `pop()`        | `removeFirst()`           |
| `peek()`       | `peekFirst()`             |

> 表：`Deque`与`Stack`方法比较表

```java
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.LinkedList;
public class Main {
    public static void main(String[] args) {
        /* Create a stack of strings based on deque */
        Deque<String> aStack = new LinkedList<String>();
        /* Push elements to stack */
        aStack.push("Java");
        aStack.push("Python");
        aStack.push("JavaScript");
        aStack.push("C/C++");
        aStack.push("Golang");
        /* Print stack details */
        showDeque("Stack", aStack);
        /* Pop elements from stack */
        while (!aStack.isEmpty()) {
            String e = aStack.pop();
            System.out.println(e);
        }
        /* Print stack details */
        showDeque("Stack", aStack);
    }
    public static <T> void showDeque(String name, Deque<T> deque) {
        System.out.printf(
            "%s(%d): %s" +
            System.getProperty("line.separator"),
            name, deque.size(), deque.toString()
        );
    }
}
```
> 代码清单：`Deque`用作`Stack`使用示例

# 关联内容（Related Topics）

- [Java 高级教程系列 - Queue 接口及其实现](https://zihengcat.github.io/2019/05/05/java-tutorial-for-language-adavanced-queue-interface-and-implementations/)

- [Java 高级教程系列 - Collection 接口与 Iterator 迭代器](https://zihengcat.github.io/2019/05/02/java-tutorial-for-language-adavanced-collection-and-iterator/)

# 参考资料（References）

- OpenJDK Documents for `Deque`: https://devdocs.io/openjdk~8/java/util/deque

