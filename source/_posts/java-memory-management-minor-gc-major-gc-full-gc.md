---
title: Java 内存管理 - Minor GC，Major GC，Full GC
subtitle: Java Memory Management - Minor GC, Major GC, Full GC
catalog: true
date: 2019-08-15
tags: ["Java", "JVM"]
---

# 前言

对于 Java 程序员而言，了解 JVM 内存管理机制是非常重要的。今天，我们来了解 Minor GC，Major GC，Full GC 之间的区别与联系。

# JVM 堆（Heap）内存模型

如图所示，JVM 将堆内存被划分为两块不同的部分：新生代（Young Generation）与老年代（Old Generation）。

![java-heap-memory-model](./java-heap-memory-model.png)

> 图：JVM 堆（Heap）内存模型 - 示意图

# 新生代（Young Generation）内存管理

JVM 新生代内存一般存放新出生的对象，也就是通过`new`运算符创建出来的新对象。当新生代内存空间将要被用完时，垃圾回收器（GC）就会进行内存回收。这一垃圾回收的过程被称为：**Minor GC。**

新生代又被划分为3个部分: **Eden 区与两块 Survivor 区。**

JVM 的新生代内存空间有如下一些要点。

- 大部分新生对象都存放在 Eden（伊甸园）区。

- 大对象（需要大量连续内存空间的对象，如：字符串、数组）直接进入老年代，这是为了避免为大对象在分配内存时由于分配担保机制带来的复制而降低效率。

- Eden 区内存将要被填满时，会发生 Minor GC 垃圾回收，将该区域内所有存活对象移动至一块 Survivor（幸存者）区，并将对象的“年龄”增加1。

- Minor GC 垃圾回收时还会检查 Survivor 区，将区域内所有存活对象移动至另一块 Survivor 区。所以在同一时刻，总是有一块 Survivor 区是空的。Hotspot 虚拟机默认 Eden 与 Survivor 比例是8-1，即每次可用整个新生代的90%，只有一个 Survivor，即1/10的内存空间会被“浪费”。

- 经过多次 GC 周期（默认为 15 次）后，仍然存活下来的对象会被转移到年老代（Old Generation）内存空间。可以通过 JVM 年龄阈值参数`-XX:MaxTenuringThreshold`来指定新生代提升至老年代的最大 GC 周期。

# 老年代（Old Generation）内存管理

老年代内存空间里包含了长期存活的对象与经过多次 Minor GC 依然存活下来的对象。在老年代内存被占满时，就会进行垃圾回收。在老年代区域发生的垃圾回收被称为：**Major GC**。这一 GC 过程通常也会花费更长的时间（慢 10 倍以上）。

# Major GC 与 Full GC

JVM 技术规范与垃圾收集领域内的论文都没有对 Major GC 与 Full GC 作出相关的定义或描述，这也启示我们，比起 GC 各阶段分别叫什么，我们更应该关注 GC 垃圾回收给应用程序带来的影响。

- Major GC：清理老年代内存空间。

- Full GC：针对新生代、老生代、元空间（Metaspace，Java 8 版本取代 PermGen ）的全局范围 GC 。

# 参考资料（References）

- Minor GC vs Major GC vs Full GC: https://plumbr.io/blog/garbage-collection/minor-gc-vs-major-gc-vs-full-gc

