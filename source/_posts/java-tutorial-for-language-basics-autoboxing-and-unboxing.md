---
title: Java 基础教程系列 - 自动装箱与拆箱
subtitle: Java Tutorial for Language Basics - Autoboxing and Unboxing
catalog: true
date: 2019-04-11
tags: ["Java", "Java Tutorial"]
---

# Java 基本类型包装类（Primitive Wrapper Classes）

在 Java 中，一切皆对象。Java 类库提供了基本类型（Primitive Types）的对象包装类（Primitive Wrapper Classes），位于`java.lang`包中。

| 基本类型（Primitive Type） | 包装类型（Wrapper Class） |
| -------------------------- | ------------------------- |
| `byte`                     | `Byte`                    |
| `short`                    | `Short`                   |
| `int`                      | `Integer`                 |
| `long`                     | `Long`                    |
| `float`                    | `Float`                   |
| `double`                   | `Double`                  |
| `char`                     | `Character`               |
| `boolean`                  | `Boolean`                 |

> 表：Java 基本类型包装类型对照表

所有包装类型都是不可变类型（Immutable），有两种创建包装类型的方式。

- 使用构造方法（Constructors）

- 使用`valueOf()`静态工厂方法

数值型包装类提供至少两枚构造方法：相应基本类型构造、字符串构造。字符型（Character）包装类提供一枚构造方法：接受一枚字符进行构造。

```java
public class Main {
    public static void main(String args[]) {
        /* Creates an Integer object from an int */
        Integer intObj1 = new Integer(100);

        /* Creates an Integer object from a String */
        Integer intObj2 = new Integer("1234");

        /* Creates a Double object from a double */
        Double doubleObj1 = new Double(10.45);

        /* Creates a Double object from a String */
        Double doubleObj2 = new Double("1234.56");

        /* Creates a Character object from a char */
        Character charObj1 = new Character('A');

        /* Creates a Boolean object from a boolean */
        Boolean booleanObj1 = new Boolean(true);

        /* Creates Boolean objects from Strings */
        Boolean booleanTrue = new Boolean("true");
        Boolean booleanFalse = new Boolean("false");
    }
}
```
> 代码清单：Java 包装类型构造方法示例

静态工厂方法`valueOf()`是另一种创建包装类的方式。

```java
public class Main {
    public static void main(String[] args) {
        /* Creates an Integer object from an int */
        Integer intObj1 = Integer.valueOf(100);

        /* Creates an Integer object from a String */
        Integer intObj2 = Integer.valueOf("1234");

        /* Creates a Double object from a double */
        Double doubleObj1 = Double.valueOf(10.45);

        /* Creates a Double object from a String */
        Double doubleObj2 = Double.valueOf("1234.60");

        /* Creates a Character object from a char */
        Character charObj1 = Character.valueOf('A');

        /* Creates a Boolean object from a boolean */
        Boolean booleanObj1 = Boolean.valueOf(true);

        /* Creates Boolean objects from Strings */
        Boolean booleanTrue = Boolean.valueOf("true");
        Boolean booleanFalse = Boolean.valueOf("false");
    }
}
```
> 代码清单：Java 包装类型静态工厂方法示例

为了取得更好的空间与时间效率，`JDK 9`以后，官方建议我们使用静态工厂方法`valueOf()`创建基本类型包装类，并将包装类型的构造方法（Constructors）都标注上了`@Deprecated(since="9")`。

```plain
Deprecated:
It is rarely appropriate to use this constructor.
The static factory valueOf(int) is generally a better choice,
as it is likely to yield significantly better space and time performance.
```
> 注：`JDK 9`对于包装类构造方法（Constructors）的说明

# Java 数值类型包装类（Numeric Wrapper Classes）

Java 基本类型包装类中，`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`被称为数值类型包装类（Numeric Wrapper Classes）。它们都继承自`java.lang`包下的抽象类`Number`。

所有 Java 数值类型包装类都包含着一些有用的常量（Constants）。`MIN_VALUE`与`MAX_VALUE`标示出该类型的上下界，例如：`Byte.MIN_VALUE`为`-128`，`Byte.MAX_VALUE`为`127`。`SIZE`标示出该类型所占用的比特（Bits）数，例如：`Byte.SIZE`为`8`，`Integer.SIZE`为`32`。

Java 数值类型包装类包含六个方法，形如`xxxValue()`，`xxx`是六种基本数值类型之一（`byte`、`short`、`int`、`long`、`float`、`double`），方法返回基本数值类型。例如：`byteValue()`返回一枚`byte`，`intValue()`返回一枚`int`。

```java
public class Main {
    public static void main(String[] args) {
        /* Creates an Integer object */
        Integer intObj = Integer.valueOf(100);

        /* Gets byte from Integer */
        byte b = intObj.byteValue();

        /* Gets double from Integer */
        double dd = intObj.doubleValue();
        System.out.println("intObj = " + intObj);
        System.out.println("byte from intObj = " + b);
        System.out.println("double from intObj = " + dd);

        /* Creates a Double object */
        Double doubleObj = Double.valueOf("12345.67");

        /* Gets different types of primitive values from Double */
        double d = doubleObj.doubleValue();
        float f = doubleObj.floatValue();
        int i = doubleObj.intValue();
        long l = doubleObj.longValue();

        System.out.println("doubleObj = " + doubleObj);
        System.out.println("double from doubleObj = " + d);
        System.out.println("float from doubleObj = " + f);
        System.out.println("int from doubleObj = " + i);
        System.out.println("long from doubleObj = " + l);
    }
}
```
> 代码清单：Java 数值类型包装类方法示例

# Java 自动装箱与拆箱（Autoboxing and Unboxing）

Java 自动装箱与拆箱（Autoboxing and Unboxing）作用于基本类型与其对应的包装类。将基本类型自动转换为其对应的包装类型，被称为自动装箱；将包装类型自动转换为其对应的基本类型，被称为自动拆箱。

Java 编译器会将代码语句转换为其对应方法形式。

```java
public class Main {
    public static void main(String[] args) {
        Integer intObj = 200; /* Boxing */
        /* Integer intObj = Integer.valueOf(200); */

        int intVal = intObj;  /* Unboxing*/
        /* int intVal = intObj.intValue(); */

        System.out.println("intObj = " + intObj);
        System.out.println("intVal = " + intVal);
    }
}
```
> 代码清单：Java 自动装箱与拆箱示例

