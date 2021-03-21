---
title: Java 高级教程系列 - 类字段反射
subtitle: Java Tutorial for Language Adavanced - Field Reflection
catalog: true
date: 2019-03-24
tags: ["Java", "Java Tutorial"]
---

# 类字段反射（Field Reflection）

我们可以使用`java.lang.reflect.Field`反射取得某个类中的字段（Field）信息。

Java 核心`Class`类提供的字段类型信息的反射 API 如下所示。

```plain
Field[] getFields()
Field[] getDeclaredFields()
Field getField(String name)
Field getDeclaredField(String name)
```
> 代码清单：字段（Field）信息反射 API

| 方法                                  | 描述                                                     |
| --------------------------------------| -------------------------------------------------------- |
| `Field[] getFields()`                 | 返回目标类中**公开**的字段，**包括**从父类继承的字段。   |
| `Field[] getDeclaredFields()`         | 返回目标类中**所有**的字段，**不包括**从父类继承的字段。 |
| `Field getField(String name)`         | 根据**字段名**获取公开字段对象。                         |
| `Field getDeclaredField(String name)` | 根据**字段名**获取目标字段对象。                         |

> 表：字段（Field）反射 API 表

`getFields()`方法返回类中声明的访问属性为`public`的字段，包括从父类继承的公开字段，返回值为字段对象数组`Field[]`。

`getDeclaredFields()`方法返回类中所有声明的字段（`public`、`private`、`protected`），但**不包括**从父类继承的字段，返回值为字段对象数组`Field[]`。

`getField(String name)`与`getDeclaredField(String name)`按照字段名获取单个字段对象`Field`，如果目标字段不存在，则抛出`NoSuchFieldException`异常。

```java
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
import java.lang.util.ArrayList;
class MySuperClass {
    public Integer super_id = -1;
    public String super_name = "Unknown";
}
class MyClass extends MySuperClass {
    public Integer id = -1;
    public String name = "Unknown";
    MyClass(Integer id, String name) {
        this.id = id;
        this.name = name;
    }
    public String toString() {
        return "MyClass: id = " + this.id + ", name = " + this.name;
    }
}
public class Main {
    public static void main(String[] args) {
        Class<MyClass> cls = MyClass.class;
        System.out.println("Declared Fields: ");
        System.out.println(
            getDeclaredFieldsArray(cls)
        );
        System.out.println("Accessible Fields: ");
        System.out.println(
            getFieldsArray(cls)
        );
    }
    public static ArrayList<String> getDeclaredFieldsArray(Class cls) {
        ArrayList<String> list = new ArrayList<String>();
        for (Field field : cls.getDeclaredFields()) {
            try {
                //System.out.println(field);
                list.add(field.toString());
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return list;
    }
    public static ArrayList<String> getFieldsArray(Class cls) {
        ArrayList<String> list = new ArrayList<String>();
        for (Field field : cls.getFields()) {
            try {
                //System.out.println(field);
                list.add(field.toString());
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return list;
    }
}
```
> 代码清单：类字段（Field）反射示例

# Java `Field` 类

`java.lang.reflect.Field`类提供关联类字段信息的动态访问。反射的字段可以是类字段（静态）或实例字段。

`Field`类声明如下所示。

```java
public final class Field
extends AccessibleObject
implements Member
```
> 代码清单：`Field`类声明

对于字段（Field）信息，最主要的操作便是获取与设置，`Field`类提供了`get/set`方法，可以便捷地获取/设置字段信息。

`Field.get(Object obj)`方法用于获取某个类实例目标字段的值，`Field.set(Object obj, Object value)`方法用于设置某个类实例目标字段的值。

```java
...
    public static void main(String[] args) {
        Class<MyClass> cls = MyClass.class;
        MyClass obj = new MyClass(1, "zihengCat");
		/* Get field value from a class instance */
        try {
            System.out.println(cls.getField("id").get(obj));
            System.out.println(cls.getField("name").get(obj));
        } catch (NoSuchFieldException | IllegalAccessException e) {
            e.printStackTrace();
        }
		/* Set field value for a class instance */
        System.out.println("Before: " + obj);
        try {
            cls.getField("id").set(obj, 2);
            cls.getField("name").set(obj, "ziheng");
        } catch (NoSuchFieldException | IllegalAccessException e) {
            e.printStackTrace();
        }
        System.out.println("After: " + obj);
    }
...
```
> 代码清单：`Field`类反射 API 运用示例

# 规避可访问性检查（Accessibility Check）

Java 语言中使用可访问性标识符（`public`、`private`、`protected`）限定各类元素（类、接口、方法、字段）的可访问性，但 Java 只在编译阶段进行访问控制检查，不会在`*.class`文件中留下任何痕迹，通过反射的手段，可以规避可访问性检查。

使用`setAccessible(boolean flag)`方法（继承自`AccessibleObject`类），将字段可访问性设置为`true`，即可以访问标识为私有`private`的字段，如果不设置可访问性直接访问私有变量，会报出`IllegalAccessException`异常。

```java
import java.lang.reflect.Field;
class MyClass {
    private String name = "Unknown";
    public MyClass() {
        //...
    }
    public String toString() {
        return "MyClass: name = " + this.name;
    }
}
public class Main {
    public static void main(String[] args) {
        Class<MyClass> cls = MyClass.class;
        try {
            MyClass obj = cls.newInstance();
            Field nameField = cls.getDeclaredField("name");
            /* Bypassing accessibility check */
            nameField.setAccessible(true);
            String nameValue = (String) nameField.get(obj);
            System.out.println("Before: " + nameValue);
            nameField.set(obj, "zihengCat");
            nameValue = (String) nameField.get(obj);
            System.out.println("After: " + nameValue);
        } catch (
            InstantiationException |
            IllegalAccessException |
            NoSuchFieldException |
            SecurityException |
            IllegalArgumentException e) {
            //e.printStackTrace();
            System.out.println(e.getMessage());
        }
    }
}
```
> 代码清单：使用反射规避可访问性检查

# 参考资料

- OpenJDK 8 官方文档：https://devdocs.io/openjdk~8/java/lang/reflect/field

