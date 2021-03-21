---
title: Java 基础教程系列 - 变量与数据类型
subtitle: Java Tutorial for Language Basics - Variable and Data Types
catalog: true
date: 2019-01-22
tags: ["Java", "Java Tutorial"]
---

# 什么是 Java 变量（Variable）

Java 变量具有三种属性：

- 变量具有数据类型

- 一块内存空间存储变量值

- 通过标示符可以引用到内存空间

Java 支持两种数据类型：

- 基本（Primitive）数据类型

- 引用（Reference）数据类型

# Java 标识符（Identifier）

简而言之，Java 标识符（Identifier）是指程序员给予 Java 程序各元素（如：变量、类、方法...）起的独特名称。

一枚 Java 标识符由一系列字符序列构成，长度没有限制。

构成 Java 标识符的字符序列可以包括字母与数字，首字符不能为数字，不能与 Java 保留关键字（Reserved Keywords）冲突。

- Java 标识符字符序列长度没有限制

- Java 标识符字符集为 Unicode 字符集，不仅仅是 ASCII 字符集

- Java 标识符大小写敏感（Case Sensitive）

- Java 标识符中不允许出现空白符（如：空格）

- `A-Z`、`a-z`、`0-9`、`_`（下划线）、`$`（美元符号）都是常用的 Java 标识符字符

| 标识符               | 解释                               |
| :------------------- | :--------------------------------- |
| `num1`               | 可以使用`a-z`、`0-9`的任意字符组合 |
| `letter`             | 仅包含小写字母                     |
| `aLetter`            | 混合使用大小写字母                 |
| `a1Letter`           | 混合使用大小写字母与数字           |
| `_aaa`               | 以下划线开头                       |
| `_`                  | 单下划线                           |
| `sum_of_two_numbers` | 下划线与字母的组合                 |
| `Outer$Inner$Level`  | 字母与美元符号的组合               |
| `$var`               | 以美元符号开头                     |

> 表：合法 Java 标识符示例

| 标识符      | 解释               |
| :---------- | :----------------- |
| `2num`      | 不允许以数字开头   |
| `my name`   | 不允许空白符分隔   |
| `num1+num2` | 不允许带有操作符   |
| `null`      | 不允许与关键字冲突 |

> 表：非法 Java 标识符示例

# Java 保留关键字（Reserved Keywords）

Java 语言定义了一系列保留关键字（Reserved Keywords）实现 Java 语义，这些关键字无法被用作标识符。

| -          | -         | -            | -           | -              |
| :--------: | :-------: | :----------: | :---------: | :------------: |
| `abstract` | `do`      | `if`         | `package`   | `synchronized` |
| `boolean`  | `double`  | `implements` | `private`   | `this`         |
| `break`    | `else`    | `import`     | `protected` | `throw`        |
| `byte`     | `extends` | `instanceof` | `public`    | `throws`       |
| `case`     | `false`   | `int`        | `return`    | `transient`    |
| `catch`    | `final`   | `interface`  | `short`     | `true`         |
| `char`     | `finally` | `long`       | `static`    | `try`          |
| `class`    | `float`   | `native`     | `strictfp`  | `void`         |
| `const`    | `for`     | `new`        | `super`     | `volatile`     |
| `continue` | `goto`    | `null`       | `switch`    | `while`        |
| `default`  |           |              |             |                |

> 表：Java 语言保留关键字（Reserved Keywords）

其中，`const`与`goto`并没有实现 Java 语义，仅作保留。

# Java 基本（Primitive）数据类型

Java 有 8 个基本（Primitive）数据类型。

| 类别    | 类型                                   |
| ------- | -------------------------------------- |
| 布尔型  | `boolean`                              |
| 整数型  | `char`、`byte`、`short`、`int`、`long` |
| 浮点型  | `float`、`double`                      |

> 表：Java 基本（Primitive）数据类型分类表

| 类型      | Bits   | 取值范围                                                 | 默认值   |
| --------- | ------ | -------------------------------------------------------- | -------- |
| `boolean` | `1`    | `true` / `false`                                         | `false`  |
| `char`    | `16`   | `\u0000` ~ `\uFFFF`                                      | `\u0000` |
| `byte`    | `8`    | `-128` ~ `127`                                           | `0`      |
| `short`   | `16`   | `-32768` ~ `32767`                                       | `0`      |
| `int`     | `32`   | `-2147483648` ~ `2147483647`                             | `0`      |
| `long`    | `64`   | `-9233372036854477808` ~ `9233372036854477807`           | `0`      |
| `float`   | `32`   | `-3.40292347E+38` ~ `3.40292347E+38`                     | `0.0f`   |
| `double`  | `64`   | `-1.79769313486231570E+308` ~ `1.79769313486231570E+308` | `0.0d`   |

> 表：Java 基本（Primitive）数据类型详情表

```java
public class Main {
	public static void main(String args[]) {
        boolean aBoolean = true;
        char aChar = 'A';
        byte aByte = 99;
        short aShort = -902;
        int anInt = 100;
        long aLong = 200L;
        float aFloat = 99.98F;
        double aDouble = 999.89;
        /* print values of the variables */
        System.out.println("aBoolean = " + aBoolean);
        System.out.println("aChar = " + aChar);
        System.out.println("aByte = " + aByte);
        System.out.println("aShort = " + aShort);
        System.out.println("anInt = " + anInt);
        System.out.println("aLong = " + aLong);
        System.out.println("aFloat = " + aFloat);
        System.out.println("aDouble = " + aDouble);
    }
}
```
> 代码清单：Java 基本（Primitive）数据类型

