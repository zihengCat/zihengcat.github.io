---
title: Java 高级教程系列 - Map 接口及其实现
subtitle: Java Tutorial for Language Adavanced - Map Interface and Implementations
catalog: true
date: 2019-05-07
tags: ["Java", "Java Tutorial"]
---

# Java 集合框架 `Map` 接口

Java 集合框架`Map`结构代表**键值对（Key-Value）集合**，一枚键值对组`<Key, Value>`在`Map`中被称为`Entry`。`Map`中的键与值必须为引用类型（Reference Type），基本数据类型（如：`int`、`double`）不能直接装入`Map`，但可以通过自动装拆箱转换为包装类型后再装入。

一枚`Map`容器，键不允许重复，至多允许一枚`null`键（不推荐往`Map`中插入`null`键）；值可以重复，多枚键可以映射同一个值。

`Map`数据结构以`Map<K,V>`接口表示，`Map`接口**不继承`Collection`接口**，**不保证迭代顺序**。

```java
public interface Map<K,V>
```
> 代码清单：`Map`接口声明

下面对`Map`接口方法作一个整理。

| 方法                                           | 说明                            |
| ---------------------------------------------- | ------------------------------- |
| `int size()`                                   | 返回 Map 中存放键值对的数量。   |
| `boolean isEmpty()`                            | 判断 Map 是否为空。             |
| `boolean containsKey(Object key)`              | 判断 Map 中是否包含目标键。     |
| `boolean containsValue(Object value)`          | 判断 Map 中是否包含目标值。     |
| `V get(Object key)`                            | 根据目标键获取值。              |
| `V put(K key, V value)`                        | 插入或更新 Map 中的键值对。     |
| `V remove(Object key)`                         | 移除 Map 中的键值对。           |
| `void clear()`                                 | 清空 Map 容器。                 |
| `void putAll(Map<? extends K, ? extends V> m)` | 批量插入或更新 Map 中的键值对。 |
| `Set<K> keySet()`                              | 返回 Map 键的视图集合。         |
| `Collection<V> values()`                       | 返回 Map 值的视图集合。         |
| `Set<Map.Entry<K,V>> entrySet()`               | 返回 Map 键值对的视图集合。     |

> 表：`Map`接口方法表

# `Map` 接口实现类

Java 集合框架对`Map`接口有几类实现。

- `HashMap`：哈希表实现

- `TreeMap`：平衡树实现

- `LinkedHashMap`：哈希表 + 双向链表实现

# Java 集合 `Map` 使用示例

我们通过一个 *Demo* 代码示例来熟悉`Map`的操作。

```java
import java.util.Set;
import java.util.HashSet;
import java.util.Map;
import java.util.HashMap;
public class Main {
    public static void main(String[] args) {
        /* Create a map of string and string */
        Map<String, String> aMap = new HashMap<String, String>();
        /* Print map details */
        showMap("Map", aMap);
        /* Put some <K, V> pairs to map */
        aMap.put("1", "Java");
        aMap.put("2", "Python");
        aMap.put("3", "JavaScript");
        aMap.put("4", "C/C++");
        /* Print map details */
        showMap("Map", aMap);
        /* Iterate map */
        for (String key : aMap.keySet()) {
            System.out.printf(
                "Key = %s, Value = %s" +
                System.getProperty("line.separator"),
                key, aMap.get(key)
            );
        }
        /* Change a pair in map */
        aMap.put("4", "Golang");
        /* Print map details */
        showMap("Map", aMap);
        /* Copy map keySet to avoid ConcurrentModificationException */
        Set<String> mapKeySet = new HashSet<String>(aMap.keySet());
        /* Remove all <K, V> pairs in map */
        for (String key : mapKeySet) {
            aMap.remove(key);
        }
        /* Print map details */
        showMap("Map", aMap);
    }
    public static <K, V> void showMap(String name, Map<K, V> map) {
        System.out.printf(
            "%s(%d): %s" +
            System.getProperty("line.separator"),
            name, map.size(), map.toString()
        );
    }
}
```
> 代码清单：`Map`使用示例





# 关联文章（Related Topics）

- [Java 高级教程系列 - 集合框架概览](https://zihengcat.github.io/2019/05/01/java-tutorial-for-language-adavanced-collection-framework-overview/)

# 参考资料（References）

- OpenJDK Documents for `Map`: https://devdocs.io/openjdk~8/java/util/map

- OpenJDK Documents for `Map.Entry`: https://devdocs.io/openjdk~8/java/util/map.entry

