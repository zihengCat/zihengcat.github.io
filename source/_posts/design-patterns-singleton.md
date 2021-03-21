---
title: 设计模式 - 单例模式
subtitle: Design Patterns - Singleton
catalog: true
date: 2019-08-01
tags: ["Design Patterns", "Java"]
---

# 关于单例模式（Singleton）

单例模式（Singleton）是一种常用的软件设计模式（Design Patterns）。单例设计模式的对象类必须保证其**有且仅有一个实例存在**。

单例模式的实现思路：

1. 一个类能返回实例对象一个引用（永远是同一个）和一个获得该实例的方法（静态方法，通常使用`getInstance()`命名）；

2. 当调用这个方法时，如果类持有的引用不为空，则返回这个引用，如果类保持的引用为空，就创建该类的实例并将实例的引用赋予该类保持的引用；

3. 将该类的构造函数定义为私有方法，其他处的代码就无法通过调用该类的构造函数来实例化该类的对象，只有通过该类提供的静态方法来得到该类的唯一实例。

# 单例模式 Java 实现

单例模式是软件设计模式中较为简单的一种。我们依据单例模式的实现思路，可以很容易地写出单例设计模式实现代码。

```java
public class Singleton extends Object {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（饿汉方式）

上述代码中，全局的单例实例是在**类装载**时构建的，称之为「饿汉方式」。

「饿汉方式」实现单例会有些浪费内存，因为单例类实例在类装载时就已经构建完成，我们可以优化为：用时加载。

```java
public class Singleton extends Object {
    private static Singleton INSTANCE;
    private Singleton() {}
    public static Singleton getInstance() {
        if (null == INSTANCE) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（懒汉方式）

上述代码中，我们将单例实例实现为在**首次被使用**时构建，称之为「懒汉方式」，可以节省内存。

但是，无论是「饿汉方式」还是「懒汉方式」实现的单例，都没有考虑到多线程问题，上述代码在单线程环境下可以正常工作，但在多线程环境下会遇到并发问题，我们可以添加「对象锁」对线程进行同步。

```java
public class Singleton extends Object {
    private static Singleton INSTANCE;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if (null == INSTANCE) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（多线程对象锁）

加入「对象锁」对线程进行同步，可以解决多线程环境下的并发问题，但是效率并不高，我们将对象锁加入到语句块中，减少「锁粒度」。

```java
public class Singleton extends Object {
    private Singleton() {}
    private static Singleton INSTANCE;
    public static Singleton getInstance() {
        synchronized(Singleton.class) {
            if (null == INSTANCE) {
                INSTANCE = new Singleton();
            }
        }
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（减少锁粒度）

上述代码中，虽然我们将对象锁加入到了语句块中减少了锁粒度，但在每次`getInstance()`时，都会有获取/释放锁的操作，效率依旧不高。有没有更高效的实现方式呢？

```java
public class Singleton extends Object {
    private Singleton() {}
    private static Singleton INSTANCE;
    public static Singleton getInstance() {
        if (null == INSTANCE) {
            synchronized(Singleton.class) {
                if (null == INSTANCE) {
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（双重检查锁）

这个世界上总是不缺聪明人，有人就想出使用「双重检查锁」（Double-Checked Locking，DCL）方法规避加减锁操作，提高运行效率。如上述代码所示，在同步语句块的上层再加入`if`判断，由于大部分`getInstance()`操作都发生在单例创建完成后，加入双重检查后便可以直接获取到实例，绕过对象锁，而创建单例时的并发问题也可以在内层对象锁中得到解决。

实际上，上述「双重检查锁」代码在并发场景下仍存在问题，由于 Java 内存模型的规则，对实例`INSTANCE`的赋值操作可能无法及时从线程内存回刷至主内存，JVM 也可能对语句进行重排序，解决方案是：使用`volatile`修饰单例，禁止指令重排序，保证变量可见性。

```java
public class Singleton extends Object {
    private Singleton() {}
    private static volatile Singleton INSTANCE;
    public static Singleton getInstance() {
        if (null == INSTANCE) {
            synchronized(Singleton.class) {
                if (null == INSTANCE) {
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（双重检查锁修正）

另一种实现单例的方法是使用「静态内部类」，利用 **Java 类加载机制**保证单实例，使用静态内部类既可以实现懒加载（Lazy-Loading）又规避了线程并发问题，效率较高，是一种不错的单例实现方法。

```java
public class Singleton extends Object {
    private Singleton() {}
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```
> 代码清单：Java 单例模式（静态内部类）

# 参考资料

- SourceMaking: https://sourcemaking.com/design_patterns/singleton

