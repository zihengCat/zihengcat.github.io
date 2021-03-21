---
title: Java 高级教程系列 - 构造器反射
subtitle: Java Tutorial for Language Adavanced - Constructor Reflection
catalog: true
date: 2019-03-26
tags: ["Java", "Java Tutorial"]
---

# 构造器反射（Constructor Reflection）

Java 类`java.lang.reflect.Constructor`实例是对类构造器（Constructor）的反射。`Constructor`类继承自通用抽象父类`Executable`，其自身是不可变（Immutable）类。

```java
public final class Constructor<T>
extends Executable
```
> 代码清单：`Constructor`类声明

`Class`类提供了获取类构造器的方法，如下所示。

```plain
Constructor<?>[] getConstructors()
Constructor<?>[] getDeclaredConstructors()
Constructor<T> getConstructor(Class<?>... parameterTypes)
Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)
```
> 代码清单：构造器（Constructor）反射 API

| 方法                                                                | 描述                                                          |
| ------------------------------------------------------------------- | ------------------------------------------------------------- |
| `Constructor<?>[] getConstructors()`                                | 返回目标类中所有公开的构造方法。                              |
| `Constructor<?>[] getDeclaredConstructors()`                        | 返回目标类中所有构造方法。                                    |
| `Constructor<T> getConstructor(Class<?>... parameterTypes)`         | 根据参数类型获取公开构造方法。                                |
| `Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)` | 根据参数类型获取构造方法。                                    |

> 表：构造器（Constructor）反射 API 表

`getConstructors()`方法返回目标类中所有公开（`public`）构造方法，返回值为构造器对象数组`Constructor<?>[]`。

`getDeclaredConstructors()`方法返回目标类中所有（`public`、`private`、`protected`、`default`）构造方法，返回值为构造器对象数组`Constructor<?>[]`。

`getConstructor(Class<?>... parameterTypes)`与`getDeclaredConstructor(Class<?>... parameterTypes)`方法根据构造方法名与参数类型取得目标构造方法对象`Constructor`，如果构造方法不存在，则抛出`NoSuchMethodException`异常。

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.lang.reflect.Modifier;
import java.lang.reflect.Parameter;
public class Main {
    public static void main(String[] args) {
        for (Constructor c : TestClass.class.getDeclaredConstructors()) {
            System.out.printf(
                /*
                 * Modifier
                 * ConstructorName(Parameter arg0, Parameter arg1, ...)
                 * throws Exception
                 */
                "%s %s %s(%s) %s\n",
                Modifier.toString(c.getModifiers()),
                " ",
                c.getName(),
                getParameterString(c),
                getExceptionString(c).equals("") ?
                getExceptionString(c) : "throws " + getExceptionString(c)
            );
        }
    }
    public static String getParameterString(Constructor c) {
        StringBuilder sb = new StringBuilder();
        for (Parameter p : c.getParameters()) {
            sb.append(
                p.getType().getSimpleName()
            );
            sb.append(" ");
            sb.append(
                p.getName()
            );
            sb.append(", ");
        }
        return sb.toString();
    }
    public static String getExceptionString(Constructor c) {
        StringBuilder sb = new StringBuilder();
        for (Class<?> cls : c.getExceptionTypes()) {
            sb.append(cls.getSimpleName());
            sb.append(", ");
        }
        return sb.toString();
    }
}
class TestSuperClass {
    public TestSuperClass() {
        // ...
    }
    public void sayHello() {
        System.out.println("Hello!");
    }
}
class TestClass<T> extends TestSuperClass {
    private static Integer _id = 0;
    public TestClass() throws Exception {
        _id++;
        System.out.println("Public constructor Void called: " + _id);
    }
    public TestClass(Integer i) {
        _id = i;
        System.out.println("Public constructor Integer called: " + _id);
    }
    public TestClass(T t) {
        // ...
    }
    private TestClass(Double d) {
        _id = d.intValue();
        System.out.println("Private constructor Double called: " + _id);
    }
}
```
> 代码清单：反射获取类构造器（Constructor）信息

# 反射生成类实例

对于构造器（Constructor）反射，我们主要关心如何调用构造方法创建类实例。我们可以使用反射动态创建类实例，有以下两种方式。

- 使用无参构造函数

- 使用有参构造函数

如果已有一枚`Class`对象，我们可以使用`newInstance()`方法创建类实例，该方法参数列表置空，等同于使用`new`操作符调用类无参构造方法创建类实例。

```java
public T newInstance(Object... initargs)
throws InstantiationException,
       IllegalAccessException,
       IllegalArgumentException,
       InvocationTargetException
```
> 代码清单：`newInstance()`方法声明

```java
...
    public static void main(String[] args) {
        try {
            /* Create class instance using `new` operator */
            TestClass t1 = new TestClass();
            System.out.println(t1);
            /* Create class instance using `Constructor` reflection
             * without arguments.
             */
            TestClass t2 = TestClass.class
                                    .getDeclaredConstructor()
                                    .newInstance();
            System.out.println(t2);
            /* Create class instance using `Constructor` reflection
             * with arguments.
             */
            TestClass t3 = TestClass.class
                                    .getDeclaredConstructor(Integer.class)
                                    .newInstance(100);
            System.out.println(t3);
            /* Create class instance using `Constructor` reflection
             * with private constructor.
             */
            Constructor<TestClass> doubleConstructor =
                TestClass.class.getDeclaredConstructor(Double.class);
            doubleConstructor.setAccessible(true);
            TestClass t4 = doubleConstructor
                .newInstance(Double.valueOf(1.414));
            System.out.println(t4);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
...
```
> 代码清单：使用`newInstance()`方法动态创建对象

另外，在`JDK 9`之后，`Class`类的`newInstance()`方法被弃用（Deprecated），因其无法很好捕捉无参构造时发生的异常，我们应该使用`Constructor.newInstance()`方法替换。

# 参考资料

- OpenJDK 8 官方文档：https://devdocs.io/openjdk~8/java/lang/reflect/constructor

