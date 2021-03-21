---
title: Java 基础教程系列 - 字符串操作符
subtitle: Java Tutorial for Language Basics - String Operator
catalog: true
date: 2019-02-02
tags: ["Java", "Java Tutorial"]
---

# Java 字符串连接操作符

在 Java 中，字符串`java.lang.String`的`+`操作符被重载（Overload）过了。

两枚字符串，如`"abc"`和`"xyz"`，可以使`+`操作符连接起来，生成一枚新的字符串`"abcxyz"`。

```java
public class Main {
    public static void main(String args[]) {
        String str1 = "abc";
        String str2 = "xyz";
        String str3 = str1 + str2;
        System.out.println(str3);
    }
}
```
> 代码清单：Java 字符串连接操作

Java 字符串连接操作符不仅可以作用于两枚字符串，还可以用来连接字符串与其他类型（基本类型、引用类型），其他类型先会被转换为字符串形式，再进行拼接。

```java
public class Main {
    public static void main(String args[]) {
        int num = 123;
        String str1 = "abc";
        String str2 = num + str1;
        System.out.println(str2);
    }
}
```
> 代码清单：Java 字符串与其他类型连接操作

# 参考资料（References）

- The Java Language Environment (HTML): https://www.oracle.com/technetwork/java/langenv-140151.html

- The Java Language Environment (PDF): https://tech-insider.org/java/research/acrobat/9505.pdf

- Stackoverflow-3559563: https://stackoverflow.com/questions/3559563/why-doesnt-java-need-operator-overloading

