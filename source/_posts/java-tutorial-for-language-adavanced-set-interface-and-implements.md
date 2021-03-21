---
title: Java 高级教程系列 - Set 接口及其实现
subtitle: Java Tutorial for Language Adavanced - Set Interface and Implements
catalog: true
date: 2019-05-03
tags: ["Java", "Java Tutorial"]
---

# Java 集合 `Set` 接口

Java 集合框架存在`Set`接口，其模型正如数学上对*集合*的定义：**由一个或多个确定不重复元素所构成的整体。**

Java 集合容器不允许包含重复的元素，允许存在至多一个`null`元素，不保证集合内元素的排列顺序。

Java 集合容器以其内部存储对象的`equals()`方法判定两个元素是否相等。如果我们试图将重复元素加入到集合容器中，集合容器会静默忽略（Ignored Silently）。

```java
public interface Set<E>
extends Collection<E>
```
> 代码清单：`Set`接口声明

# Java 集合 `Set` 使用示例

我们通过一个 Demo 代码示例来熟悉`Set`接口。

```java
import java.util.HashSet;
import java.util.Set;
public class Main {
    public static void main(String[] args) {
        /* Create a set */
        Set<String> s1 = new HashSet<String>();
        /* Add a few elements to set */
        s1.add("Java");
        s1.add("Python");
        s1.add("JavaScript");
        s1.add("C/C++");
        /* Add a duplicate element */
        System.out.println(
            "Add a duplicate element -> [Java]: " +
            s1.add("Java")
        );
        /* Add a unique element */
        System.out.println(
            "Add a unique element -> [Golang]: " +
            s1.add("Golang")
        );
        /* Add a null element */
        s1.add(null);
        /* Print the set details */
        System.out.printf(
            "s1(%s): %s" +
            System.getProperty("line.separator"),
            s1.size(),
            s1.toString()
        );
        /* Remove an element */
        s1.remove("JavaScript");
        /* Remove a null element */
        s1.remove(null);
        /* Print the set details */
        System.out.printf(
            "s1(%s): %s" +
            System.getProperty("line.separator"),
            s1.size(),
            s1.toString()
        );
    }
}
```
> 代码清单：`Set`使用示例

# `Set` 交集、并集与补集

`Set`集合支持交集、并集与补集操作，还可以使用`a.containsAll(b)`检查某集合是否为另一集合的子集。需要注意的是，集合操作会改变原集合的状态，如果想保留原集合，可以在运用集合操作前，先备份原集合。

![set-operations](./set-operations.png)

> 图：`Set`交集、并集与补集

```java
import java.util.HashSet;
import java.util.Set;
public class Main {
    public static void main(String[] args) {
        /* Create a set */
        Set<String> s1 = new HashSet<String>();
        /* Add a few elements to set */
        s1.add("Java");
        s1.add("Python");
        s1.add("JavaScript");
        s1.add("C/C++");
        /* Create another set */
        Set<String> s2 = new HashSet<String>();
        /* Add a few elements to set */
        s2.add("Java");
        s2.add("Python");
        s2.add("Golang");
        s2.add("ASM");
        /* Print sets details */
        showSet("s1", s1);
        showSet("s2", s2);
        /* Union Set */
        showSet("UnionSet", doUnionSet(s1, s2));
        /* Intersection Set */
        showSet("IntersectionSet", doIntersectionSet(s1, s2));
        /* Subtraction Set */
        showSet("SubtractionSet", doSubtractionSet(s1, s2));
    }
    public static <T> Set<T> doSubtractionSet(Set<T> s1, Set<T> s2) {
        Set<T> s1Copy = new HashSet<T>(s1);
        s1Copy.removeAll(s2);
        return s1Copy;
    }
    public static <T> Set<T> doIntersectionSet(Set<T> s1, Set<T> s2) {
        Set<T> s1Copy = new HashSet<T>(s1);
        s1Copy.retainAll(s2);
        return s1Copy;
    }
    public static <T> Set<T> doUnionSet(Set<T> s1, Set<T> s2) {
        Set<T> s1Copy = new HashSet<T>(s1);
        s1Copy.addAll(s2);
        return s1Copy;
    }
    public static <T> void showSet(String name, Set<T> s) {
        System.out.printf(
            "%s(%s): %s" +
            System.getProperty("line.separator"),
            name,
            s.size(),
            s.toString()
        );
    }
}
```
> 代码清单：`Set`交集、并集与补集

# 参考资料

- OpenJDK Documents：https://devdocs.io/openjdk~8/java/util/set

