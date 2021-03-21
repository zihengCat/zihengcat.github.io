---
title: Java 高级教程系列 - Collection 接口与 Iterator 迭代器
subtitle: Java Tutorial for Language Adavanced - Collection and Iterator
catalog: true
date: 2019-05-02
tags: ["Java", "Java Tutorial"]
---

# Java 集合 `Collection` 接口

Java 接口`java.util.Collection`是 Java 集合框架的根接口，它是一枚泛型接口。

`Collection`接口定义了对序列数据类型的一系列基本操作。如：

- 获取集合容器内元素数量

- 添加新元素到集合容器内

- 移除集合容器内的元素

- 检查元素是否在集合容器内

- 检查集合容器是否为空

- ...

对`Collection`接口定义的方法作一个整理。

```java
public interface Collection<E>
extends Iterable<E>
```
> 代码清单：`Collection`接口声明

| 方法                                        | 说明                                       |
| ------------------------------------------- | -------------------------------------------|
| `int size()`                                | 返回集合容器元素数量。                     |
| `boolean equals(Object o)`                  | 检查两个集合容器是否相等。                 |
| `boolean isEmpty()`                         | 检查集合容器是否为空。                     |
| `Iterator<E> iterator()`                    | 返回集合容器迭代器。                       |
| `Object[] toArray()`                        | 集合容器转换为数组。                       |
| `String toString()`                         | 返回集合容器字符串形式。                   |
| `void clear()`                              | 清空集合容器。                             |
| `boolean contains(Object o)`                | 检查集合容器中是否包含指定元素。           |
| `boolean add(E e)`                          | 添加一个新元素至集合容器。                 |
| `boolean remove(Object o)`                  | 移除集合容器内的指定元素。                 |
| `boolean containsAll(Collection<?> c)`      | 检查集合容器中是否包含指定全部元素。       |
| `boolean addAll(Collection<? extends E> c)` | 添加全部元素至集合容器。                   |
| `boolean removeAll(Collection<?> c)`        | 移除集合容器内的指定全部元素。             |
| `boolean retainAll(Collection<?> c)`        | 仅保留指定的元素，移除集合容器内其他元素。 |

> 表：`Collection`接口方法表

# Java 集合 `Collection` 使用示例

我们通过一个 Demo 示例来熟悉`Collection`接口。

```java
import java.util.ArrayList;
import java.util.Collection;
public class Main {
    public static void main(String[] args) {
        /* Create a collection of strings */
        Collection<String> c = new ArrayList<String>();
        /* Print collection details */
        System.out.printf(
            "After creation: Size = %d, Elements = %s" +
            System.getProperty("line.separator"),
            c.size(),
            c.toString()
        );
        /* Add some strings to collection */
        c.add("Java");
        c.add("Python");
        c.add("JavaScript");
        c.add("C/C++");
        /* Print collection details */
        System.out.printf(
            "After adding elements: Size = %d, Elements = %s" +
            System.getProperty("line.separator"),
            c.size(),
            c.toString()
        );
        /* Check element in collection */
        System.out.printf(
            "Collection contains: Element %s -> %s, Element %s -> %s" +
            System.getProperty("line.separator"),
            "Java", c.contains("Java"),
            "java", c.contains("java")
        );
        /* Remove element from collection */
        c.remove("JavaScript");
        /* Print collection details */
        System.out.printf(
            "After remove elements: Size = %d, Elements = %s" +
            System.getProperty("line.separator"),
            c.size(),
            c.toString()
        );
        /* Clear all elements */
        c.clear();
        /* Print collection details */
        System.out.printf(
            "After clear elements: Size = %d, Elements = %s" +
            System.getProperty("line.separator"),
            c.size(),
            c.toString()
        );
    }
}
```
> 代码清单：`Collection`使用示例

# `Iterator` 迭代器

Java 集合迭代器`Iterator<E>`提供了一种迭代遍历集合容器元素的统一方式，集合迭代器`Iterator<E>`实例是个一次性对象，如果需要对集合进行多次遍历，每一次都需要调用`iterator()`方法重新获取迭代器。

```java
public interface Iterator<E>
```
> 代码清单：`Iterator`迭代器声明

| 方法                                        | 说明                                         |
| ------------------------------------------- | -------------------------------------------- |
| `boolean hasNext()`                         | 如果集合中仍有剩余元素未迭代，则返回`true`。 |
| `E next()`                                  | 迭代并返回下一元素。                         |
| `void remove()`                             | 通过迭代器移除元素。                         |

> 表：`Iterator`迭代器接口方法表

# 遍历 `Collection` 集合

有如下几种方式可以用来遍历`Collection`集合元素。

- 转换为数组对象（Array）

- 使用迭代器（Iterator）

- 使用`for-each`循环

- 使用`forEach()`方法

以下代码示例展示了遍历`Collection`集合元素的几种方式。

```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
public class Main {
    public static void main(String[] args) {
        /* Create a collection of strings */
        Collection<String> c = new ArrayList<String>();
        /* Add some elements to collection */
        c.add("Java");
        c.add("Python");
        c.add("JavaScript");
        c.add("C/C++");
        /* Iterator traversal */
        System.out.println("Iterator traversal: ");
        Iterator<String> it = c.iterator();
        while (it.hasNext()) {
            String e = it.next();
            System.out.println(e);
        }
        /* for-each traversal */
        System.out.println("for-each traversal: ");
        for (String e : c) {
            System.out.println(e);
        }
        /* forEach() traversal */
        System.out.println("forEach() traversal: ");
        c.forEach(System.out::println);
    }
}
```
> 代码清单：遍历`Collection`集合元素

需要注意的是，虽然这几种方式在写法上各不相同，但其本质都是**使用迭代器遍历集合**，殊途同归。

# 参考资料

- OpenJDK Documents for `Collection`: https://devdocs.io/openjdk~8/java/util/collection

- OpenJDK Documents for `Iterator`: https://devdocs.io/openjdk~8/java/util/iterator

