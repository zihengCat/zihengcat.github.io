---
title: 使用栈实现队列
subtitle: Implement a Queue Using Two Stacks
catalog: true
date: 2019-08-07
tags: ["Data Structure", "Algorithm", "Java"]
---

# 栈与队列

栈（Stack）与队列（Queue）都是我们熟悉的线性表数据结构，栈的性质：后进先出（LIFO），队列的性质：先进先出（FIFO），性质截然不同的两种数据结构。思考这样一个问题，能否使用栈结构实现一个队列呢？

# 使用栈实现队列

实际上，使用栈结构实现队列的思路非常简单：维护两个栈，元素入队时，将元素压入一个栈中；元素出队时，将存放入队元素的栈元素全部弹出至另一个栈中，对另一个栈出栈，**使用两次栈的后进先出（LIFO）操作后，元素顺序即变为队列的先进先出（FIFO）了**。

![reverse-stack](./reverse-stack.png)

> 图：两次后进先出（LIFO）操作 - 示意图

理解思路之后，编写实现代码就非常简单了。

```java
/* 使用标准库`Deque`接口与`LinkedList`实现类作为栈结构 */
import java.util.Deque;
import java.util.LinkedList;
/**
 * 栈实现队列 - 主类
 * @param <E>
 */
public class StacksAsQueue<E> {
    private Deque<E> inputStack;
    private Deque<E> outputStack;
    public StacksAsQueue() {
        /* 入队元素栈 */
        inputStack = new LinkedList<E>();
        /* 出队元素栈 */
        outputStack = new LinkedList<E>();
    }
    /**
     * 元素入队
     * @param e
     * @return void
     */
    public void enqueue(E e) {
        inputStack.push(e);
    }
    /**
     * 元素出队
     * @param void
     * @return E
     */
    public E dequeue() {
        /* 检查两个栈是否为空 */
        if (inputStack.isEmpty() && outputStack.isEmpty()) {
            /* 两栈都为空 -> 无数据，抛出错误或返回`null`值 */
            //throw new RuntimeException("[ERROR]: Two stacks are empty!");
            return null;
        }
        /* 出队元素栈为空 -> 将入队元素栈中数据弹出至此栈 */
        if (outputStack.isEmpty()) {
            while (!inputStack.isEmpty()) {
                outputStack.push(inputStack.pop());
            }
        }
        /* 返回出队元素栈中最上层元素 */
        return outputStack.pop();
    }
}
```
> 代码清单：`Java`使用栈实现队列

```java
/* 导入实现类 */
import StacksAsQueue;
public class StacksAsQueueTest {
    public static void main(String[] args) {
        StacksAsQueue<Integer> stacksAsQueue = new StacksAsQueue<Integer>();
        /* 入队列 */
        for (int i = 0; i < 10; ++i) {
            System.out.println("Enqueue: " + i);
            stacksAsQueue.enqueue(i);
        }
        /* 出队列 */
        for (int i = 0; i < 10; ++i) {
            System.out.println("Dequeue: " + stacksAsQueue.dequeue());
        }
    }
}
```
> 代码清单：`Java`使用栈实现队列 - 测试代码

# 参考资料（References）

- StackOverflow: https://stackoverflow.com/questions/69192/how-to-implement-a-queue-using-two-stacks

