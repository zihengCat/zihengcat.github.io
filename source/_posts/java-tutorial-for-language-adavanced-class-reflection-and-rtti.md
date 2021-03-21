---
title: Java 高级教程系列 - 反射 Class 类型信息
subtitle: Java Tutorial for Language Adavanced - Class Reflection and RTTI
catalog: true
date: 2019-03-23
tags: ["Java", "Java Tutorial"]
---

# `Class` 类

Java 类`java.lang.Class`是对 Java 虚拟机（JVM）载入类型信息的抽象，一枚`Class`类实例即表示程序运行时 JVM 载入的某个类，`Class`类是 Java 反射机制（Reflection）的核心。

`Class`类是一个泛型（Generics）类，接受一个类型参数，例如：`Class<Boolean>`表示`Boolean`对象的类型信息，`Class<?>`表示某未知类型信息。

创建`Class`实例一般有如下三种方式：

- 使用类型字面量

- 使用`getClass()`方法（对象）

- 使用`forName()`方法（类）

# 创建 `Class` 类实例

每一个类都持有其对应的`Class`类的对象引用，我们可以直接使用`类型名.class`获取某类的`Class`类实例。

```java
public class Main {
    public static void main(String[] args) {
        Class<?> c = null;
        /* Get Class object
           using class literal */
        c = void.class;
        c = Void.TYPE;
        c = boolean.class;
        c = Boolean.TYPE;
        c = char.class;
        c = Character.TYPE;
        c = byte.class;
        c = Byte.TYPE;
        c = short.class;
        c = Short.TYPE ;
        c = int.class;
        c = Integer.TYPE;
        c = long.class;
        c = Long.TYPE;
        c = float.class;
        c = Float.TYPE;
        c = double.class;
        c = Double.TYPE;
        c = Test.class;
        System.out.println(c);
    }
}
class Test {
    // ...
}
```
> 代码清单：使用「类型字面量」创建`Class`类实例

Java 父类`java.lang.Object`有一枚`getClass()`方法，对于某个类对象，可以使用`getClass()`获取`Class`类实例。

```java
class Test {
    // ...
}
public class Main {
    public static void main(String[] args) {
        Test testRef = new Test();
        Class testClass = testRef.getClass();
        System.out.println(testClass);
    }
}
```
> 代码清单：使用「`getClass()`方法」创建`Class`类实例

`Class`类静态方法`forName()`接受类全名，JVM 加载类后返回`Class`类实例的引用，如果该类已经被 JVM 加载，则直接返回`Class`类实例。

```plain
Class<?> forName(String className)
Class<?> forName(String name, boolean initialize, ClassLoader loader)
```
> 代码清单：`forName()`重载方法

```java
class TestClass {
    static {
        System.out.println("Loading class TestClass...");
    }
}
public class Main {
    public static void main(String[] args) {
        try {
            /*
            String className = "TestClass";
            Boolean initialize = false;
            ClassLoader classLoader = Main.class.getClassLoader();
            Class c = Class.forName(className, initialize, classLoader);
            */
            System.out.println("About to load...");
            /* Will load and initialize the class */
            c = Class.forName(className);
            System.out.println(c);
        } catch (ClassNotFoundException e) {
            System.out.println(e.getMessage());
        }
    }
}
```
> 代码清单：使用「`forName()`方法」创建`Class`类实例

# `Class` 类反射 API

了解如何创建`Class`类实例后，我们来看一看`Class`类自带的反射 API 方法。

我们可以使用`Class`类反射 API 方法获取到类信息，如：类全名、类访问修饰符等等。

使用`getName()`方法可以获取某个类的类全名，使用`getSimpleName()`方法可以获取某个类的直接类名。

使用`getSuperclass()`方法可以获取某个类的父类，由于 Java 不支持多继承，一个类只能继承一个父类，单继承上推直至`java.lang.Object`，对`Object`类运用此方法，会返回`null`。

使用`getModifiers()`方法获取类修饰符（例：`final`、`abstract`、`public`），`getModifiers()`方法的返回值是整型（Integer），可以使用`java.lang.reflect.Modifier.toString(int mod)`方法取得修饰符的文本表示形式。

使用`getInterfaces()`方法获取某个类实现的所有接口（Interfaces），返回值为`Class[]`。

以下实例展示了如何使用反射机制动态获取运行时类型信息（RTTI）。

```java
import java.lang.Cloneable;
import java.io.Serializable;
import java.lang.reflect.Modifier;
import java.lang.reflect.TypeVariable;
public class Main {
    public static void main(String[] args) {
        System.out.println(getClassDescription(TestInterface.class));
        System.out.println(getClassDescription(TestClass.class));
    }
    public static String getClassDescription(Class cls) {
        StringBuilder classDesciption = new StringBuilder();
        Integer modifierBits = 0;
        String typeKeyword = "";
        if (cls.isInterface()) {
            modifierBits =
            cls.getModifiers() & Modifier.interfaceModifiers();
            if (cls.isAnnotation()) {
                typeKeyword = "@interface";
            } else {
                typeKeyword = "interface";
            }
        } else if (cls.isEnum()) {
            modifierBits =
            cls.getModifiers() & Modifier.classModifiers();
            typeKeyword = "enum";
        } else {
            modifierBits =
            cls.getModifiers() & Modifier.classModifiers();
            typeKeyword = "class";
        }
        /* Detect class Modifiers */
        String modifiers = Modifier.toString(modifierBits);
        classDesciption.append(modifiers);
        /* Detect class Type Keyword */
        classDesciption.append(" " + typeKeyword);
        /* Detect class Simple Name */
        String simpleName = cls.getSimpleName();
        classDesciption.append(" " + simpleName);
        /* Detect class Generic Parms */
        String genericParms = getGenericTypeParams(cls);
        classDesciption.append(genericParms);
        /* Detect class Super Class */
        Class superClass = cls.getSuperclass();
        if (superClass != null) {
            String superClassSimpleName = superClass.getSimpleName();
            classDesciption.append(" extends " + superClassSimpleName);
        }
        /* Detect class Interfaces */
        String interfaces = getClassInterfaces(cls);
        if (interfaces != null) {
            classDesciption.append(
			    interfaces.equals("") ?
                interfaces : " implements " + interfaces
            );
        }
        /* RTTI about class */
        return classDesciption.toString();
    }
    public static String getGenericTypeParams(Class cls) {
        StringBuilder sb = new StringBuilder();
        TypeVariable<?>[] typeParms = cls.getTypeParameters();
        if (typeParms.length > 0) {
            String[] paramNames = new String[typeParms.length];
            for (int i = 0; i < typeParms.length; i++) {
                paramNames[i] = typeParms[i].getTypeName();
            }
            sb.append('<');
            String parmsList = String.join(",", paramNames);
            sb.append(parmsList);
            sb.append('>');
        }
        return sb.toString();
    }
    public static String getClassInterfaces(Class cls) {
        Class[] interfaces = cls.getInterfaces();
        String interfacesList = "";
        if (interfaces.length > 0) {
            String[] interfaceNames = new String[interfaces.length];
            for (int i = 0; i < interfaces.length; i++) {
                interfaceNames[i] = interfaces[i].getSimpleName();
            }
            interfacesList = String.join(", ", interfaceNames);
        }
        return interfacesList;
    }
}
abstract interface TestInterface<T> {
    public Integer id = -1;
    public T sayHello(String str);
}
final class TestClass<K, V> implements Cloneable, Serializable {
    private Integer id = -1;
    private String name = "Unknown";
    public TestClass(Integer id, String name) {
        this.id = id;
        this.name = name;
    }
    public Object clone() {
        try {
            return super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e.getMessage());
        }
    }
    public String toString() {
        return "TestClass: id=" + this.id + ", name=" + this.name;
    }
}
```
> 代码清单：Java 反射动态获取类型信息（RTTI）

