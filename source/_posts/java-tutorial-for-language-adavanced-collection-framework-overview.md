---
title: Java 高级教程系列 - 集合框架概览
subtitle: Java Tutorial for Language Adavanced - Collection Framework Overview
catalog: true
date: 2019-05-01
tags: ["Java", "Java Tutorial"]
---

# Java 集合框架

集合（Collection）又被称为容器（Container），是一种可以存储与操作对象的对象，存放在集合中的对象被称为集合元素（Element）。

Java 语言标准库设计并实现了一系列集合类型，统称为 Java 集合框架（Java Collection Framework）。Java 集合框架由三部分核心组件构成，位于`java.util`包下。

- 集合接口（Collection Interfaces）

- 实现类（Implementation Classes）

- 算法类（Algorithm Classes）

以下图表是对 Java 集合框架的概览。

![java-collection-framework-overview](./java-collection-framework-overview.png)

> 图：Java 集合框架概览

| **Interface** | **Hash Table** | **Resizable Array** | **Balanced Tree** | **Linked List** | **Hash Table + Linked List** |
| :-----------: | :------------: | :-----------------: | :---------------: | :-------------: | :--------------------------: |
| **`Set`**     | `HashSet`      |                     | `TreeSet`         |                 | `LinkedHashSet`              |
| **`List`**    |                | `ArrayList`         |                   | `LinkedList`    |                              |
| **`Deque`**   |                | `ArrayDeque`        |                   | `LinkedList`    |                              |
| **`Map`**     | `HashMap`      |                     | `TreeMap`         |                 | `LinkedHashMap`              |

> 表：Java 集合框架概览表

## 集合接口（Collection Interfaces）

一种接口代表对一种集合类型统一操作方式。

- `List`接口表示**序列**数据类型

- `Set`接口表示**集合**数据类型

- `Map`接口表示**关联**数据类型

- ...

## 实现类（Implementation Classes）

Java 集合框架提供对集合接口的实现类，如：`ArrayList`实现了`List`接口。同一接口可以有多种实现，不同的实现适用于不同的使用场景。

## 算法类（Algorithm Classes）

算法类可以从外部对集合类型做出各种操作，如：

- 搜索，在集合类型中查找目标元素

- 排序，对集合按照规则排序

- 转换，将一种集合类型转换为另一种集合类型

- 拷贝，从集合中拷贝元素

- ...

# 参考资料

- Java Collection Framework：https://docs.oracle.com/javase/8/docs/technotes/guides/collections/index.html

- Runoob：http://www.runoob.com/java/java-collections.html

