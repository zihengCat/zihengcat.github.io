---
title: Java 高级教程系列 - 反射效率
subtitle: Java Tutorial for Language Adavanced - Reflection Efficiency
catalog: true
date: 2019-04-28
tags: ["Java", "Java Tutorial"]
---

# 前言

通过 Java 反射（Reflection）机制，我们可以动态地构造实例，获取对象信息，调用对象方法等。我们通过几个测试用例，来探索反射调用与直接调用的效率差异。

# 反射机制的效率测量

准备一个简单的反射测试类，包含两个属性字段，以及它们的`getter/setter`方法，另外还有一个自定义方法。

```java
public class UserClass {
    private Integer id;
    private String name;
    public TestUser() {
        // ...
    }
    public String sayHi() {
        return "Hi";
    }
    public Integer getId() {
        return this.id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return this.name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```
> 代码清单：反射测试类

接下来编写测试主类，迭代1亿次，测试**直接构造/反射构造**、**直接调用/反射调用**各自的运行时间。

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
public class Main {
    /* number of iterations */
    public static final long LIMIT = 1000 * 1000 * 1000;
    public static void main(String[] args) {
        try {
            testCommonConstructor();
            testReflectionConstructor();
            testCommonMethod();
            testReflectionMethod();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public static void testReflectionConstructor() throws Exception {
        UserClass user = null;
        Constructor<UserClass> ctor = UserClass.class.getConstructor();
        //ctor.setAccessible(true);
        long startTime = System.currentTimeMillis();
        for (long i = 0L; i < LIMIT; ++i) {
            //user = UserClass.class.getConstructor().newInstance();
            user = ctor.newInstance();
        }
        long endTime = System.currentTimeMillis();
        System.out.println(
            "Reflection constructor: " +
            (endTime - startTime) +
            "ms"
        );
    }
    public static void testCommonConstructor() throws Exception {
        UserClass user = null;
        long startTime = System.currentTimeMillis();
        for (long i = 0L; i < LIMIT; ++i) {
            user = new UserClass();
        }
        long endTime = System.currentTimeMillis();
        System.out.println(
            "Common constructor: " +
            (endTime - startTime) +
            "ms"
        );
    }
    public static void testReflectionMethod() throws Exception {
        UserClass user = new UserClass();
        Method method = UserClass.class.getMethod("sayHi");
        //method.setAccessible(true);
        long startTime = System.currentTimeMillis();
        for (long i = 0L; i < LIMIT; ++i) {
            method.invoke(user);
        }
        long endTime = System.currentTimeMillis();
        System.out.println(
            "Reflection method: " +
            (endTime - startTime) +
            "ms"
        );
    }
    public static void testCommonMethod() throws Exception {
        UserClass user = new UserClass();
        long startTime = System.currentTimeMillis();
        for (long i = 0L; i < LIMIT; ++i) {
            user.sayHi();
        }
        long endTime = System.currentTimeMillis();
        System.out.println(
            "Common method: " +
            (endTime - startTime) +
            "ms"
        );
    }
}
```
> 代码清单：反射主类

测试结果显示，反射调用的确比直接调用慢，运行效率相差有 4 ~ 6 倍。

```plain
Common constructor: 329ms
Reflection constructor: 2059ms
Common method: 485ms
Reflection method: 1693ms
```
> 注：测试代码运行结果

# 提高反射效率

有一些方法可以用来提升反射的效率，缓存`Class`、`Field`、`Constructor`、`Method`对象，避免每次反射操作重复获取；设置`setAccessible(true)`关闭安全访问检查；如果对性能极致追求，可以考虑使用第三方库，直接操作字节码。

- 缓存`Class`、`Field`、`Constructor`、`Method`对象

- 设置`setAccessible(true)`关闭安全访问检查

- 使用第三方库（如：ReflectASM）

# 参考资料

- ReflectASM：https://github.com/EsotericSoftware/reflectasm

