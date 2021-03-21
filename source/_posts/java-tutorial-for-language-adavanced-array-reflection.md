---
title: Java 高级教程系列 - 数组反射
subtitle: Java Tutorial for Language Adavanced - Array Reflection
catalog: true
date: 2019-03-28
tags: ["Java", "Java Tutorial"]
---

# 数组反射（Array Reflection）

数组（Array）是 Java 语言中一个比较特殊的存在，但其仍是对象。对于数组对象的反射操作，主要由`java.lang.reflect.Array`提供。`Array`类提供一系列静态方法允许我们动态创建与操作 Java 数组，注意，`Array`类中的所有方法都是静态方法。

```java
public final class Array
extends Object
```
> 代码清单：`Array`类声明

通过反射动态创建数组对象，我们需要使用`Array`类的`newInstance()`重载方法，该方法有两个版本，分别用来创建一维数组与多维数组。`Class`类提供`isArray()`方法检测反射类是否为数组类型。

```plain
Object newInstance(Class<?> componentType, int length)
Object newInstance(Class<?> componentType, int... dimensions)
```
> 代码清单：数组（Array）类`newInstance()`重载方法声明

| 方法                                                            | 描述                                     |
| --------------------------------------------------------------- | ---------------------------------------- |
| `Object newInstance(Class<?> componentType, int length)`        | 根据元素类型与长度动态创建数组对象。     |
| `Object newInstance(Class<?> componentType, int... dimensions)` | 根据元素类型与维度动态创建多维数组对象。 |
| `int getLength(Object array)`                                   | 获取目标数组的长度。                     |
| `Object get(Object array, int index)`                           | 获取目标数组下标元素。                   |
| `void set(Object array, int index, Object value)`               | 设置目标数组下标元素。                   |

> 表：数组（Array）反射 API 表

```java
import java.lang.reflect.Array;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        /* Create an array of int of length 2 */
        int[] arrayOfInt = (int[])Array.newInstance(int.class, 2);
        /* Print the values in array element
           Default values will be zero */
        System.out.println(Arrays.toString(arrayOfInt));
        int a_0 = (int)Array.get(arrayOfInt, 0);
        int a_1 = (int)Array.get(arrayOfInt, 1);
        System.out.printf("a[0] = %d, a[1] = %d\n", a_0, a_1);
        /* Set the values for array */
        Array.set(arrayOfInt, 0, 101);
        Array.set(arrayOfInt, 1, 102);
        /* Print the values in array element again */
        System.out.println(Arrays.toString(arrayOfInt));
    }
}
```
> 代码清单：`Array`类反射运用示例

# 参考资料

- OpenJDK 8 官方文档：https://devdocs.io/openjdk~8/java/lang/reflect/array

