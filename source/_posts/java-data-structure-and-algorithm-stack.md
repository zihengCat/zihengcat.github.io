---
title: Java 数据结构与算法 - 栈
subtitle: Java Data Structure and Algorithm - Stack
catalog: true
date: 2019-05-18
tags: ["Java", "Data Structure", "Algorithm", "Stack"]
---

# 栈（Stack）数据结构概览

栈（英语：Stack）是一种**后进先出（LIFO，Last In First Out）**的线性数据结构，在计算机科学中被广为运用。栈只允许在线性表的一端（栈顶，英语：Top）添加和移除数据，遵循后进先出的运行规律。

栈数据结构通常使用**一维数组**或**链表**来实现，栈包含两种基本操作：**入栈（压栈，Push），出栈（弹栈，Pop）**。

- 入栈：将元素压入栈顶，栈顶指针前移

- 出栈：从栈顶移除元素，栈顶指针后移

![Stack-Overview](./stack-overview.png)

> 图：栈（Stack）结构示意图

# 栈（Stack）数据结构操作接口

我们将使用 Java 语言手写栈（Stack）数据结构，首先，定义好栈所具有的操作接口方法（Interface）。

| 接口方法            | 解释说明               |
| ------------------- | ---------------------- |
| `void push(E e)`    | 将元素压入栈中。       |
| `E pop()`           | 将栈顶元素出栈。       |
| `E peek()`          | 查看栈顶元素。         |
| `int size()`        | 返回栈已存储元素数量。 |
| `boolean isEmpty()` | 判断栈是否为空。       |
| `void clear()`      | 清空栈。               |
| `String toString()` | 返回栈的字符串形式。   |

> 表：栈（Stack）数据结构接口方法表

我们可以将栈（Stack）数据结构的接口方法写成一枚 Java `interface`，如下所示。

```java
import StackEmptyException;
import StackOverflowException;
public abstract interface Stack<E> {
    public abstract void push(E e)
        throws StackOverflowException;
    public abstract E pop()
        throws StackEmptyException;
    public abstract E peek()
        throws StackEmptyException;
    public abstract int size();
    public abstract boolean isEmpty();
    public abstract void clear();
}
```
> 代码清单：栈（Stack）数据结构操作接口（Java Interface）

注意，在这里我们额外引入两枚栈异常，用于处理与栈方法相关的异常。

- `StackEmptyException`：栈为空

- `StackOverflowException`：栈溢出

> 注：著名的程序员问答社区 *Stack Overflow* 的名字便取典于**栈溢出**。

```java
@SuppressWarnings("all")
public class StackEmptyException extends Exception {
    public StackEmptyException() {
        super();
    }
    public StackEmptyException(String message) {
        super(message);
    }
}
```
> 代码清单：`StackEmptyException`异常定义

```java
@SuppressWarnings("all")
public class StackOverflowException extends Exception {
    public StackOverflowException() {
        super();
    }
    public StackOverflowException(String message) {
        super(message);
    }
}
```
> 代码清单：`StackOverflowException`异常定义

# Java 顺序栈（数组实现）

栈（Stack）数据结构通常有一维数组与链表两种实现方式。使用数组实现的栈被称为**顺序栈**，因为数组元素在内存中的排列方式是连续紧挨的。我们先使用数组实现 Java 顺序栈。

Java 顺序栈将数据存储在连续的数组中，定义一枚**一维数组**，作为存放栈元素的空间，为了简单起见，我们暂时将栈容量固定，不考虑数组扩容的情况。栈还需要一枚**栈顶指针**，指向栈顶元素，我们以**数组索引下标**作为栈顶指针。

```java
import Stack;
import StackEmptyException;
import StackOverflowException;
public class ArrayStack<E> implements Stack<E> {
    private static final int DEFAULT_CAPACITY = 1024;
    private int capacity;
    private E[] data;
    private int top;
    @SuppressWarnings("unchecked")
    public ArrayStack(int capacity) {
        this.capacity = capacity;
        this.data = (E[]) new Object[capacity];
        this.top = -1;
    }
    public ArrayStack() {
        this(DEFAULT_CAPACITY);
    }
}
```
> 代码清单：Java 顺序栈（数组实现）

Java 顺序栈使用**数组索引下标**作为栈顶指针，那么栈为空`isEmpty()`的情况可以定义为：栈顶指针取负（数组索引下标元素不存在），这里指定为`-1`，使用`size()`计算栈已存储的元素数量也只需将栈顶指针加`1`即可。

```java
...
    /**
     * 计算栈已存储的元素数量。
     * @param void
     * @return Size of the stack.
     */
    public int size() {
        return top + 1;
    }
    /**
     * 判断栈是否为空。
     * @param void
     * @return Boolean
     */
    public boolean isEmpty() {
        return top == -1;
    }
...
```
> 代码清单：Java 顺序栈`isEmpty()`与`size()`方法实现

元素入栈的操作分为如下三个步骤，元素入栈后，会**堆叠在栈顶位置**，**栈顶指针也前移为新元素所在的索引下标**。

1. 检查数组可用容量

2. 栈顶指针前移

3. 装入新元素

```java
...
    /**
     * 将元素压入栈中。
     * @param element
     * @return void
     * @throws StackOverflowException
     */
    public void push(E e)
    throws StackOverflowException {
        if (size() >= data.length) {
            throw new StackOverflowException();
        }
        ++top;
        data[top] = e;
    }
...
```
> 代码清单：Java 顺序栈`push()`方法实现

元素出栈的操作分为如下步骤，元素出栈后，**栈顶指针后移**。由于 Java 顺序栈使用数组实现，没有必要将栈顶元素置空，直接后移栈顶指针即可，元素入栈会自动覆盖先前已出栈的元素位置。

1. 检查栈是否为空

2. 取得当前栈顶元素

3. 栈顶指针后移

4. 返回栈顶元素

```java
...
    /**
     * 将栈顶元素出栈。
     * @param void
     * @return Element at the top.
     * @throws StackEmptyException
     */
    public E pop()
    throws StackEmptyException {
        if (isEmpty()) {
            throw new StackEmptyException();
        }
        --top;
        return data[top + 1];
    }
...
```
> 代码清单：Java 顺序栈`pop()`方法实现

使用`peek()`方法可以查看当前栈顶元素，方法实现与元素出栈相似，只是**不需要移动栈顶指针**。

```java
...
    /**
     * 查看当前栈顶元素。
     * @param void
     * @return Element at the top.
     * @throws StackEmptyException
     */
    public E peek()
    throws StackEmptyException {
        if (isEmpty()) {
            throw new StackEmptyException();
        }
        return data[top];
    }
...
```
> 代码清单：Java 顺序栈`peek()`方法实现

我们为顺序栈提供一枚`clear()`方法，用于清空栈元素。方法具体实现为：**重新创建数组，重置栈顶指针**。为了帮助 Java 垃圾收集器更好地做内存回收工作，这里我们显式遍历旧数组，置空旧数组中存在的元素。

```java
...
    /**
     * 清空栈。
     * @return void
     * @param void
     */
    @SuppressWarnings("unchecked")
    public void clear() {
        for (int i = 0; i <= top; ++i) {
            data[i] = null;
        }
        this.data = (E[]) new Object[capacity];
        this.top = -1;
    }
...
```
> 代码清单：Java 顺序栈`clear()`方法实现

最后，我们来覆写 Java 顺序栈`toString()`方法，更加方便地查看栈元素。

```java
...
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (int i = 0; i <= top; ++i) {
            sb.append(data[i].toString());
            sb.append(", ");
        }
        sb.append(']');
        return sb.toString();
    }
...
```
> 代码清单：Java 顺序栈`toString()`方法实现

# Java 链式栈（链表实现）

栈（Stack）数据结构的另一种实现方式是链表（Linked List），使用链表存储栈元素，使用链表实现的栈被称为**链式栈**，因为链表节点在内存中的排列方式是分散的，各个节点之间使用指针相连。现在，我们使用链表实现 Java 链式栈。

在 *Java 数据结构与算法 - 链表* 中，我们已经学习了如何手写一枚 Java 链表，现在我们使用自己写的链表来实现 Java 链式栈，实现思路为：**只允许从链表一端添加/删除元素，链表另一端封闭**，链表的头插/尾插法任取其一（以下选择尾插法），把链表当做栈来使用，栈顶指针可以省略。

```java
import Stack;
import StackEmptyException;
import StackOverflowException;
import LinkedList;
public class LinkedStack<E> implements Stack<E> {
    private static final int DEFAULT_CAPACITY = 1024;
    private int capacity;
    private LinkedList<E> data;
    public static void main(String[] args) {
        // ...
    }
    public LinkedStack(int capacity) {
        this.capacity = capacity;
        this.data = new LinkedList<E>();
    }
    public LinkedStack() {
        this(DEFAULT_CAPACITY);
    }
    /**
     * 将元素压入栈中。
     * @param element
     * @return void
     * @throws StackOverflowException
     */
    public void push(E e)
    throws StackOverflowException {
        if (size() >= capacity) {
            throw new StackOverflowException();
        }
        data.addLast(e);
    }
    /**
     * 将栈顶元素出栈。
     * @param void
     * @return Element at the top.
     * @throws StackEmptyException
     */
    public E pop()
    throws StackEmptyException {
        if (isEmpty()) {
            throw new StackEmptyException();
        }
        return data.removeLast();
    }
    /**
     * 查看当前栈顶元素。
     * @param void
     * @return Element at the top.
     * @throws StackEmptyException
     */
    public E peek()
    throws StackEmptyException {
        if (isEmpty()) {
            throw new StackEmptyException();
        }
        return data.getLast();
    }
    /**
     * 计算栈已存储的元素数量。
     * @param void
     * @return Size of the {@code Stack}.
     */
    public int size() {
        return data.size();
    }
    /**
     * 判断栈是否为空。
     * @param void
     * @return Boolean
     */
    public boolean isEmpty() {
        return data.isEmpty();
    }
    /**
     * 清空栈。
     * @return void
     * @param void
     */
    public void clear() {
        data.clear();
    }
    @Override
    public String toString() {
        return data.toString();
    }
}
```
> 代码清单：Java 链式栈（链表实现）

# 栈测试（Stack Test）

我们来写个栈单元测试程序，检测栈能否正常工作。

```java
import Stack;
import ArrayStack;
import LinkedStack;
public class StackTest {
    public static void main(String[] args) {
        Stack<String> aStack = new ArrayStack<String>();
        System.out.println("[INFO]: Testing push()...");
        for (int i = 0; i < 10; ++i) {
            try {
                aStack.push(String.valueOf(i));
            } catch (Exception e) {
                e.printStackTrace();
            }
            System.out.println(aStack);
        }
        System.out.println("[INFO]: Testing pop()...");
        for (int i = 0; i < 10; ++i) {
            try {
                aStack.pop();
            } catch (Exception e) {
                e.printStackTrace();
            }
            System.out.println(aStack);
        }
    }
}
```
> 代码清单：栈单元测试（Stack Unit Test）

# 关联文章（Related Topics）

- [Java 数据结构与算法 - 链表](https://zihengcat.github.io/2019/05/15/java-data-structure-and-algorithm-linked-list/)

- [Java 高级教程系列 - Deque 接口及其实现](https://zihengcat.github.io/2019/05/06/java-tutorial-for-language-adavanced-deque-interface-and-implementations/)

# 参考资料

- Wikipedia: https://en.wikipedia.org/wiki/Stack_(abstract_data_type)

- Wikipedia: https://en.wikipedia.org/wiki/Stack_overflow

- Introduction to Java Programming: Fundamental Data Structures and Algorithms: https://courses.edx.org/courses/course-v1:UC3Mx+IT.1.3x+1T2019/course/

- Fundamental Data Structures and Algorithms: https://www.bilibili.com/video/av52559737

