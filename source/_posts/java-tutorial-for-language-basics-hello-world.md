---
title: Java 基础教程系列 - Hello, World
subtitle: Java Tutorial for Language Basics - Hello, World
catalog: true
date: 2019-01-20
tags: ["Java", "Java Tutorial"]
---

# 第一个 Java 程序：打印文本

学习一门程序设计语言，写出的第一个程序往往是 Hello, World，这已成为惯例。

```java
public class Main {
   /* main method begins execution of Java application */
   public static void main(String args[]) {
       System.out.println("Hello, World, Welcome to Java Programming!");
   }
}
```
> 代码清单：Java 打印文本

# Java 打印多行文本

```java
public class Main {
   public static void main(String args[]) {
       System.out.println("Hello, World!");
       System.out.println("Welcome to Java Programming!");
   }
}
```
> 代码清单：Java 打印文本

# Java 使用转义字符（Escape Sequence）打印文本

| 转义字符 | 解释说明 |
| :------- | :------- |
| `\n`     | 换行符   |
| `\r`     | 回车符   |
| `\t`     | 制表符   |
| `\\`     | 反斜杠   |
| `\"`     | 双引号   |

> 表：转义字符说明表

```java
public class Main {
   public static void main(String args[]) {
       System.out.println("Welcome\nto\nJava\nProgramming!");
   }
}
```
> 代码清单：Java 使用转义字符打印文本

# Java 使用`printf()`格式化打印文本

```java
public class Main {
   public static void main(String args[]) {
       System.out.printf("%s\n%s\n",
           "Welcome to", "Java Programming!"
       );
   }
}
```
> 代码清单：Java 使用`printf()`格式化打印函数

# Java 在自定义方法中打印文本

```java
public class Main {
   public static void main(String args[]) {
       sayHello();
   }
   public static void sayHello(){
       System.out.println("Hello, World!");
   }
}
```
> 代码清单：Java 在自定义方法中打印文本

# Java 使用`JOptionPane`获取文本并打印

```java
import javax.swing.JOptionPane;
public class DialogApp {
    public static void main(String args[]) {
        String s = JOptionPane.showInputDialog("Enter a string: ");
        System.out.println("You entered: " + s + ".");
    }
}
```
> 代码清单：Java 使用`JOptionPane`获取文本并打印

