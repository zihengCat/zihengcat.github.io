---
title: Java 高级教程系列 - 死锁示例及解决
subtitle: Java Tutorial for Language Adavanced - Deadlock Example and Solution
catalog: true
date: 2019-08-09
tags: ["Java", "Java Tutorial"]
---

# 前言

本文讲解 Java 死锁的基本概念，并给出一个 Java 死锁的简单示例，也讲解如何发现死锁，解决死锁问题。

# Java 死锁（Deadlock）

在 Java 中，死锁（Deadlock）情况是指：**两个或两个以上的线程持有不同系统资源的锁，线程彼此都等待获取对方的锁来完成自己的任务，但是没有让出自己持有的锁，线程就会无休止等待下去。线程竞争的资源可以是：锁、网络连接、通知事件，磁盘、带宽，以及一切可以被称作“资源”的东西。**

![java_deadlock](./java_deadlock.png)

> 图：Java 死锁示意图

如上图所示，`Thread-1`持有资源`Object1`但是需要资源`Object2`完成自身任务，同样的，`Thread-2`持有资源`Object2`但需要`Object1`，双方都在等待对方手中的资源但都不释放自己手中的资源，从而进入死锁。

```java
public class DeadLockExample {
    public Object resourceA = new Object();
    public Object resourceB = new Object();
    public static void main(String[] args) {
        DeadLockExample deadLockExample = new DeadLockExample();
        Runnable runnableA = new Runnable() {
            @Override
            public void run() {
                synchronized(deadLockExample.resourceA) {
                    System.out.printf(
                        "[INFO]: %s get resourceA" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.printf(
                        "[INFO]: %s trying to get resourceB" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                    synchronized(deadLockExample.resourceB) {
                        System.out.printf(
                            "[INFO]: %s get resourceB" + System.lineSeparator(),
                            Thread.currentThread().getName()
                        );
                    }
                    System.out.printf(
                        "[INFO]: %s has done" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                }
            }
        };
        Runnable runnableB = new Runnable() {
            @Override
            public void run() {
                synchronized(deadLockExample.resourceB) {
                    System.out.printf(
                        "[INFO]: %s get resourceB" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.printf(
                        "[INFO]: %s trying to get resourceA" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                    synchronized(deadLockExample.resourceA) {
                        System.out.printf(
                            "[INFO]: %s get resourceA" + System.lineSeparator(),
                            Thread.currentThread().getName()
                        );
                    }
                    System.out.printf(
                        "[INFO]: %s has done" + System.lineSeparator(),
                        Thread.currentThread().getName()
                    );
                }
            }
        };
        new Thread(runnableA).start();
        new Thread(runnableB).start();
    }
}
/* EOF */
```
> 代码清单：Java 死锁（Deadlock）样例

通过上述 Java 死锁（Deadlock）样例程序，可以更好地理解死锁情况。

```plain
[INFO]: Thread-0 get resourceA
[INFO]: Thread-1 get resourceB
[INFO]: Thread-0 trying to get resourceB
[INFO]: Thread-1 trying to get resourceA
```
> 代码清单：程序输出

# Java 死锁侦测

`JDK`自带了一些简单好用的工具，可以帮助我们检测死锁（如：`jstack`）。

```bash
$ jstack $(jps -l | grep 'DeadLockExample' | cut -f1 -d ' ')
```
> 代码清单：使用`jstack`侦测目标 JVM 进程

```plain
...
Java stack information for the threads listed above:
===================================================
"Thread-1":
	at DeadLockExample$2.run(DeadLockExample.java:58)
	- waiting to lock <0x000000076ab660a0> (a java.lang.Object)
	- locked <0x000000076ab660b0> (a java.lang.Object)
	at java.lang.Thread.run(Thread.java:748)
"Thread-0":
	at DeadLockExample$1.run(DeadLockExample.java:28)
	- waiting to lock <0x000000076ab660b0> (a java.lang.Object)
	- locked <0x000000076ab660a0> (a java.lang.Object)
	at java.lang.Thread.run(Thread.java:748)

Found 1 deadlock.
```
> 代码清单：命令输出

可以看到，`jstack`已经侦测出 JVM 进程中存在线程死锁的情况，并为我们打印出了相关信息。

JVM 图形化分析工具`jconsole`也可以帮助我们分析死锁情况。

# Java 死锁预防

1. 以确定的顺序获锁

2. 超时放弃

3. 死锁检测

# 关联文章（Related Topics）

- [Java 高级教程系列 - 线程创建](https://zihengcat.github.io/2019/07/14/java-tutorial-for-language-adavanced-create-thread/)

# 参考资料（References）

- 掘金：https://juejin.im/post/5aaf6ee76fb9a028d3753534

