---
title: Java 高级教程系列 - 反射概览
subtitle: Java Tutorial for Language Adavanced - Reflection Overview
catalog: true
date: 2019-03-21
tags: ["Java", "Java Tutorial"]
---

# 什么是反射（Reflection）

反射机制（Reflection）是一种在程序运行时获取或修改程序运行状态的方法。

传统意义上，我们认为 Java 是一门静态语言，但是 Java 提供了反射机制，可以在程序运行时（Runtime）动态获取某个类的相关信息：字段（Fields）、修饰符（Modifiers）、方法（Methods）、父类（Superclass）等。

我们可以使用 Java 反射机制动态创建类实例，调用方法，获取/设置字段。但是我们不能改变类的数据结构（例：运行时为某类添加新字段），也不能改变类的方法实现（例：运行时为某类添加新方法）。

# 反射 API（Reflection API）

Java 语言的反射机制以一系列反射 API 的形式提供给开发者使用，大部分反射 API 类/接口 位于`java.lang.reflect`包中，核心`Class`类位于`java.lang`包中。

| 类名                            | 描述                  |
| ------------------------------- | --------------------- |
| `java.lang.Class`               | 表示 JVM 加载的某个类 |
| `java.lang.reflect.Field`       | 表示类/接口字段       |
| `java.lang.reflect.Constructor` | 表示类构造器          |
| `java.lang.reflect.Method`      | 表示类/接口方法       |
| `java.lang.reflect.Modifier`    | 表示类成员修饰符      |
| `java.lang.reflect.Array`       | 运行时创建数组        |

> 表：Java 常用反射 API 类

# 反射机制的使用场景（Example Usage）

使用 Java 反射机制，我们可以做到很多事情。

- 获取对象的类名

- 获取类全名，访问修饰符等

- 获取类字段

- 获取类方法，方法的参数、返回值类型、参数名（JDK 8）等

- 获取类所有构造方法

- 调用类构造方法创建类实例

- 调用类方法

- 运行时动态创建数组对象，修改数组元素

- ...

