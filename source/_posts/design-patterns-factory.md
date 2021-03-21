---
title: 设计模式 - 工厂模式
subtitle: Design Patterns - Factory Pattern
catalog: true
date: 2020-01-16
tags: ["Design Patterns", "Creational Pattern", "Java"]
---

# 关于工厂模式（Factory Pattern）

工厂模式（Factory Pattern）是最常用的软件设计模式之一。这种类型的设计模式属于“创建型模式（Creational Pattern）”，它提供了一种创建对象的最佳方式。在工厂模式中，创建对象不会对客户端暴露创建逻辑，通过使用一个共同的接口来指向新创建的对象。工厂模式将一组对象的实现细节与它们的一般使用分离开来。

工厂模式的实现思路：定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。

# 工厂模式 Java 实现

以 Shape 为例，我们将创建一个`Shape`接口和实现`Shape`接口实体类，并且定义工厂类`ShapeFactory`。

```java
public interface Shape {
    public void draw();
}
/* EOF */
```
> 代码清单：`Shape`接口

```java
import Shape;
public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Rectangle::draw()");
    }
}
/* EOF */
```
> 代码清单：`Shape`接口实现类 - `Rectangle`类

```java
import Shape;
public class Triangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Triangle::draw()");
    }
}
/* EOF */
```
> 代码清单：`Shape`接口实现类 - `Triangle`类

```java
import Shape;
public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Circle::draw()");
    }
}
/* EOF */
```
> 代码清单：`Shape`接口实现类 - `Circle`类

```java
import Shape;
import Triangle;
import Rectangle;
import Circle;
public class ShapeFactory {
    public Shape getShape(String type) {
        if (type == null) {
            return null;
        }
        switch(type) {
            case "Rectangle": return new Rectangle();
            case "Triangle": return new Triangle();
            case "Circle": return new Circle();
            default: return null;
        }
        return null;
    }
}
/* EOF */
```
> 代码清单：`ShapeFactory`工厂类

```java
import Shape;
import ShapeFactory;
public class Main {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();
        Shape aTriangle = shapeFactory.getShape("Triangle");
        Shape aRectangle = shapeFactory.getShape("Rectangle");
        Shape aCircle = shapeFactory.getShape("Circle");
        aTriangle.draw();
        aRectangle.draw();
        aCircle.draw();
    }
}
/* EOF */
```
> 代码清单：`Main`主类

执行程序，输出结果：

```plain
Triangle::draw()
Rectangle::draw()
Circle::draw()
```
> 代码清单：程序输出结果

<!-- EOF -->
