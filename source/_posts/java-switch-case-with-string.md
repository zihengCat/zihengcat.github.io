---
title: Java 7 新特性 - Switch Case 使用字符串
subtitle: Java 7 - String in Switch Case Statement
catalog: true
date: 2019-08-08
tags: ["Java", "JVM"]
---

# 前言

`switch`语句在 Java 诞生之初便已存在，但只支持`int`与`enum`类型。在 Java 7 之后，`switch`语句也可以支持`String`字符串类，方便了开发者。

# Java `switch` 使用字符串

我们通过一个实例，了解如何在`switch`语句中使用`String`字符串。

```java
public class StringSupportedInSwitchStatement {
    public static void main(String[] args) {
        for (String s : "one two three".split(" ")) {
            System.out.printf(
                "%s -> %s" + System.lineSeparator(),
                s, getResult(s)
            );
        }
        System.out.printf(
            "%s -> %s" + System.lineSeparator(),
            "five", getResult("five")
        );
    }
    public static String getResult(String token) {
        String value = null;
        switch (token) {
            case "one":
                value = "Token [one] identified";
                break;
            case "two":
                value = "Token [two] identified";
                break;
            case "three":
                value = "Token [three] identified";
                break;
            case "four":
                value = "Token [four] identified";
                break;
            default:
                value = "No token was identified";
                break;
        }
        return value;
    }
}
```
> 代码清单：Java `switch` 使用字符串

该样例程序输出如下所示。

```plain
one -> Token [one] identified
two -> Token [two] identified
three -> Token [three] identified
five -> No token was identified
```
> 代码清单：Java `switch` 使用字符串 - 程序输出

# 原理剖析

在 JVM 技术规范中，规定了`switch`语句的编译方式：**`switch`语句会被编译为`tableswitch`与`lookupswitch`两条字节码指令**，之所以要使用两条字节码指令，也是为执行效率考量。

**JVM 字节码指令`tableswitch`与`lookupswitch`只能操作`int`类型的数据**，而`byte`、`char`、`short`类型数据都可以宽化转换为`int`，其他数值类型必须显式窄化转换为`int`类型才可被`switch`使用（Java 不允许隐式窄化转换），这也是`switch`语句无法直接使用`long`类型的原因。

在 Java 中，`String`是一个类，并不是基本数据类型，无法直接被`switch`语句使用。需要某种方法将`String`字符串映射为`int`类型，解决方案便是：`hashCode()`。

`hashCode()`方法返回对象实例的哈希值，返回值为`int`类型。**Java 编译器在编译带`String`的`switch`语句时，会将`String`转换为哈希整型值，再在执行代码前加入一道`if`防卫式判断。**

我们使用`javap`反编译样例程序字节码，可以看到，带`String`的`switch`语句实际上是在比较`String`的哈希值，并额外加入一道`if`判断防止哈希冲突。

实际上，带`String`的`switch`语句是 Java 编译器为我们提供的一颗“语法糖”，在 JVM 虚拟机层面并未做改变。

```plain
...
  public static java.lang.String getResult(java.lang.String);
    Code:
       0: aconst_null
       1: astore_1
       2: aload_0
       3: astore_2
       4: iconst_m1
       5: istore_3
       6: aload_2
       7: invokevirtual #12                 // Method java/lang/String.hashCode:()I
      10: lookupswitch  { // 4
                110182: 52
                115276: 66
               3149094: 94
             110339486: 80
               default: 105
          }
      52: aload_2
      53: ldc           #13                 // String one
      55: invokevirtual #14                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
      58: ifeq          105
      61: iconst_0
      62: istore_3
      63: goto          105
      66: aload_2
      67: ldc           #15                 // String two
      69: invokevirtual #14                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
      72: ifeq          105
      75: iconst_1
      76: istore_3
      77: goto          105
      80: aload_2
      81: ldc           #16                 // String three
      83: invokevirtual #14                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
      86: ifeq          105
      89: iconst_2
      90: istore_3
      91: goto          105
      94: aload_2
      95: ldc           #17                 // String four
      97: invokevirtual #14                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
     100: ifeq          105
     103: iconst_3
     104: istore_3
     105: iload_3
     106: tableswitch   { // 0 to 3
                     0: 136
                     1: 142
                     2: 148
                     3: 154
               default: 160
          }
     136: ldc           #18                 // String Token [one] identified
     138: astore_1
     139: goto          163
     142: ldc           #19                 // String Token [two] identified
     144: astore_1
     145: goto          163
     148: ldc           #20                 // String Token [three] identified
     150: astore_1
     151: goto          163
     154: ldc           #21                 // String Token [four] identified
     156: astore_1
     157: goto          163
     160: ldc           #22                 // String No token was identified
     162: astore_1
     163: aload_1
     164: areturn
    LineNumberTable:
      line 26: 0
      line 27: 2
      line 29: 136
      line 30: 139
      line 32: 142
      line 33: 145
      line 35: 148
      line 36: 151
      line 38: 154
      line 39: 157
      line 41: 160
      line 44: 163
...
```
> 代码清单：样例程序`javap`字节码反编译

# 参考资料（References）

- Java Virtual Machine Specification: https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-3.html#jvms-3.10

- StackOverflow: https://stackoverflow.com/questions/10287700/difference-between-jvms-lookupswitch-and-tableswitch

