---
title: Java 高级教程系列 - List 接口及其实现
subtitle: Java Tutorial for Language Adavanced - List Interface and Implementations
catalog: true
date: 2019-05-04
tags: ["Java", "Java Tutorial"]
---

# Java 集合框架 `List` 接口

Java 集合框架存在`List`接口，表示：**元素有序排列的集合序列（Sequence）**，也被称为**线性表**、**列表**。线性表内部的元素按先后顺序依次排列，相同元素可以重复出现。

Java 集合框架`List`接口继承了`Collection`接口，并添加了对列表元素以**索引方式**进行访问的方法，你可以将一枚元素添加到列表的指定位置，**列表索引从零开始（Zero-Based）**。

```java
public interface List<E>
extends Collection<E>
```
> 代码清单：`List`接口声明

下面对`List`接口方法作一个整理。

| 方法                                                   | 解释                                               |
| ------------------------------------------------------ | -------------------------------------------------- |
| `void add(int index, E element)`                       | 将新元素插入到列表指定索引位置，列表元素后移。     |
| `E remove(int index)`                                  | 移除指定索引位置的列表元素。                       |
| `boolean addAll(int index, Collection<? extends E> c)` | 将新元素序列插入到列表指定索引位置，列表元素后移。 |
| `E get(int index)`                                     | 取得指定索引位置的列表元素。                       |
| `E set(int index, E element)`                          | 将指定位置的列表元素设为新值，返回旧值。           |
| `int indexOf(Object o)`                                | 顺序查询指定元素的索引位置，不存在则返回`-1`。     |
| `int lastIndexOf(Object o)`                            | 逆序查询指定元素的索引位置，不存在则返回`-1`。     |
| `ListIterator<E> listIterator()`                       | 返回列表迭代器。                                   |
| `ListIterator<E> listIterator(int index)`              | 返回指定索引位置的列表迭代器。                     |
| `List<E> subList(int fromIndex, int toIndex)`          | 返回指定索引子列表视图，包含上界，不包含下界。     |

> 表：`List`接口方法表

# `List` 接口实现类

Java 集合框架对`List`接口有两类实现：**数组（Resizable Array）**与**链表（Linked List）**。

- `ArrayList`：数组实现

- `LinkedList`：链表实现

两种实现各有优势，`ArrayList`在随机存/取元素（`get/set`）方面性能优异，但在添加/移除（`add/remove`）元素方面速度较慢；`LinkedList`在添加/移除（`add/remove`）元素方面性能出色，在随机存/取元素（`get/set`）方面速度较慢，根据实际使用场景，选择合适的实现类。

# Java 集合 `List` 使用示例

我们通过一个 Demo 代码示例来熟悉`List`接口。

```java
import java.util.ArrayList;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        /* Create a list of strings */
        List<String> aList = new ArrayList<String>();
        /* Print list details */
        showList("List", aList);
        /* Add a few elements to list */
        aList.add("Java");
        aList.add("Python");
        aList.add("JavaScript");
        aList.add("C/C++");
        /* Print list details */
        showList("List", aList);
        /* Add an element at index 1 */
        aList.add(1, "Ruby");
        /* Print list details */
        showList("List", aList);
        /* Get list index and element */
        for (int i = 0; i < aList.size(); ++i) {
            System.out.printf(
                "Index = %d, Element = %s" +
                System.getProperty("line.separator"),
                i, aList.get(i)
            );
        }
        /* Change element at index 0 */
        aList.set(0, "Golang");
        /* Print list details */
        showList("List", aList);
        /* Remove an element at index 3 */
        aList.remove(3);
        /* Print list details */
        showList("List", aList);
        /* Get a sublist by indexes */
        showList("SubList[1:3]", aList.subList(1, 3));
    }
    public static <T> void showList(String name, List<T> list) {
        System.out.printf(
            "%s(%s): %s" +
            System.getProperty("line.separator"),
            name, list.size(), list.toString()
        );
    }
}
```
> 代码清单：`List`使用示例

# `ListIterator` 列表迭代器

`ListIterator`列表迭代器继承自`Iterator`，在集合迭代器的基础上添加了对列表元素的次序访问方法，可以获取某元素的列表索引位置，以及插入、替换、移除某索引位置的列表元素。列表迭代器可手动指定迭代方向（Forward / Backward）。

```java
public interface ListIterator<E>
extends Iterator<E>
```
> 代码清单：`ListIterator`声明

```plain
Element(0)   Element(1)   Element(2)   ... Element(n-1)   Element(n)
cursor positions:  ^            ^                ^              ^
```
> 注：`ListIterator`示意

```java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;
public class Main {
    public static void main(String[] args) {
        /* Create a list of strings */
        List<String> aList = new ArrayList<String>();
        /* Add a few elements to list */
        aList.add("Java");
        aList.add("Python");
        aList.add("JavaScript");
        aList.add("C/C++");
        /* Get the list iterator */
        ListIterator<String> it = aList.listIterator();
        System.out.println("List Iterator in forward direction: ");
        while (it.hasNext()) {
            Integer i = it.nextIndex();
            String s = it.next();
            System.out.printf(
                "Index = %d, Element = %s" +
                System.getProperty("line.separator"),
                i, s
            );
        }
        //it = aList.listIterator();
        System.out.println("List Iterator in backward direction: ");
        /* Reuse the iterator to iterate from the end to the beginning */
        while (it.hasPrevious()) {
            Integer i = it.previousIndex();
            String s = it.previous();
            System.out.printf(
                "Index = %d, Element = %s" +
                System.getProperty("line.separator"),
                i, s
            );
        }
    }
}
```
> 代码清单：`ListIterator`使用示例

# 关联内容（Related Topics）

- [Java 高级教程系列 - Collection 接口与 Iterator 迭代器](https://zihengcat.github.io/2019/05/02/java-tutorial-for-language-adavanced-collection-and-iterator/)

# 参考资料（References）

- OpenJDK Documents for `List`: https://devdocs.io/openjdk~8/java/util/list

- OpenJDK Documents for `ListIterator`: https://devdocs.io/openjdk~8/java/util/listiterator

