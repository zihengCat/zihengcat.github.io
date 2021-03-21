---
title: Java 高级教程系列 - 方法反射
subtitle: Java Tutorial for Language Adavanced - Method Reflection
catalog: true
date: 2019-03-25
tags: ["Java", "Java Tutorial"]
---

# 方法反射（Method Reflection）

Java 类`java.lang.reflect.Method`实例是对类方法（Method）的反射。`Method`类继承自通用抽象父类`Executable`，其自身是不可变（Immutable）类。

```plain
Method[] getMethods()
Method[] getDeclaredMethods()
Method getMethod(String name, Class<?>... parameterTypes)
Method getDeclaredMethod(String name, Class<?>... parameterTypes)
```
> 代码清单：方法（Method）信息反射 API

| 方法                                                             | 描述                                                          |
| ---------------------------------------------------------------- | ------------------------------------------------------------- |
| `Method[] getMethods()`                                          | 返回目标类中所有可访问的公开方法，包括从父类继承的公开方法。  |
| `Method[] getDeclaredMethods()`                                  | 返回目标类中所有方法，不包括从父类继承的方法。                |
| `Method getMethod(String name, Class... parameterTypes)`         | 根据方法名与参数类型取得目标方法对象。                        |
| `Method getDeclaredMethod(String name, Class... parameterTypes)` | 根据方法名与参数类型取得目标方法对象，不包括从父类继承的方法。|

> 表：方法（Method）反射 API 表

`getMethods()`返回类中所有公开方法（`public`），包括从父类继承的公开方法，返回值为方法对象数组`Method[]`。

`getDeclaredMethods()`方法返回类中所有方法（`public`、`private`、`protected`），但**不包括**从父类继承的方法，返回值为方法对象数组`Method[]`。

`getMethod(String name, Class<?>... parameterTypes)`与`getDeclaredMethod(String name, Class<?>... parameterTypes)`根据**方法名**与**参数类型**取得目标方法对象`Method`，如果目标字段不存在，则抛出`NoSuchMethodException`异常。

```java
import java.lang.reflect.Method;
import java.lang.reflect.Modifier;
import java.lang.reflect.Parameter;
public class Main {
    public static void main(String[] args) {
        for (Method m : TestClass.class.getDeclaredMethods()) {
            System.out.printf(
                /*
                 * Modifier
                 * ReturnType MethodName(
                 *     ParameterType arg0, ParameterType arg1, ...
                 * )
                 * throws Exception
                 */
                "%s %s %s(%s) %s\n",
                Modifier.toString(m.getModifiers()),
                m.getReturnType().getSimpleName(),
                m.getName(),
                getParameterString(m),
                getExceptionString(m).equals("") ?
                getExceptionString(m) : "throws " + getExceptionString(m)
            );
        }
    }
    public static String getExceptionString(Method m) {
        StringBuilder sb = new StringBuilder();
        for (Class<?> c : m.getExceptionTypes()) {
            sb.append(c.getSimpleName());
            sb.append(", ");
        }
        return sb.toString();
    }
    public static String getParameterString(Method m) {
        StringBuilder sb = new StringBuilder();
        for (Parameter p : m.getParameters()) {
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
    public TestClass() {
        // ...
    }
    public TestClass(T t) {
        // ...
    }
    public static Integer getInt(String s) throws Exception {
        return Integer.valueOf(s);
    }
    public static Integer getInt(Integer i) throws Exception {
        return i;
    }
}
```
> 代码清单：反射获取方法（Method）信息

# 反射调用方法

对于方法（Method）反射，我们主要关心如何通过反射调用目标方法，`Method`类提供了`invoke(Object obj, Object... args)`方法，允许我们通过反射调用方法。

`Method.invoke(Object obj, Object... args)`方法第一个参数是一个类实例，第二个参数是目标方法的参数列表，如果第一个参数为`null`，则表示调用类静态方法。

```java
public Object invoke(Object obj, Object... args)
throws IllegalAccessException,
       IllegalArgumentException,
       InvocationTargetException
```
> 代码清单：`invoke`方法声明

```java
...
    public static void main(String[] args) {
		try {
            TestClass t = new TestClass();
            Method m = TestClass.class.getDeclaredMethod(
			    "getInt", String.class
            );
            System.out.println(
                m.invoke(t, "123")
            );
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
...
```
> 代码清单：`invoke`反射调用方法

# 参考资料

- OpenJDK 8 官方文档：https://devdocs.io/openjdk~8/java/lang/reflect/method

