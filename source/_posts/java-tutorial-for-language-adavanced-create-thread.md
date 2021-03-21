---
title: Java 高级教程系列 - 线程创建
subtitle: Java Tutorial for Language Adavanced - Create Thread
catalog: true
date: 2019-07-14
tags: ["Java", "Java Tutorial"]
---

# Java 创建多线程（Multithreading）

Java 并发编程以多线程（Multithreading）为基础。Java 屏蔽了不同操作系统之间多线程 API 的差异，抽象出一套统一的多线程编程范式，为程序员减轻多线程开发的难度。

在 Java 中，线程被抽象为了`Thread`类，一枚`Thread`类实例即对应一枚操作系统线程（1：1）。Java 线程创建有如下几种方式：

- 继承`Thread`类

- 实现`Runnable`接口

- 实现`Callable`接口

# 继承`Thread`类

继承`Thread`类，覆写其`run()`方法，即可创建线程。

```java
import java.lang.String;
import java.lang.Thread;
public class MyThread extends Thread {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
    @Override
    public void run() {
        for (int i = 0; i < 10; ++i) {
            System.out.println("[INFO]: MyThread -> " + String.valueOf(i));
        }
    }
}
```
> 代码清单：继承`Thread`类

# 实现`Runnable`接口

直接继承`Thread`类创建线程，可能会受限于 Java 仅支持单继承的局限性，不够灵活。实现`Runnable`接口，实现接口`run()`方法体，是另一种创建线程的方式。在创建线程时，向`Thread`类构造函数传递一枚`Runnable`实例。

```java
import java.lang.String;
import java.lang.Runnable;
public class MyRunnable implements Runnable {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
    @Override
    public void run() {
        for (int i = 0; i < 10; ++i) {
            System.out.println("[INFO]: MyRunnable -> " + String.valueOf(i));
        }
    }
}
```
> 代码清单：实现`Runnable`接口

`Runnable`接口非常简单，只有一个待实现的抽象`run()`方法。

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```
> 代码清单：`Runnable`接口源码

# 实现`Callable`接口

`Callable`接口的出现是为了弥补`Runnable`接口实现多线程无法带回线程任务执行结果（返回值），无法抛出异常的局限。利用`Callable`、`Future`、`FutureTask`等并发组件，我们可以方便地取得多线程任务的执行结果。

```java
import java.util.concurrent.Callable;
import java.util.concurrent.Future;
import java.util.concurrent.RunnableFuture;
import java.util.concurrent.FutureTask;
public class MyCallable implements Callable<String> {
    public static void main(String[] args) {
        FutureTask<String> futureTask = new FutureTask<String>(
            new MyCallable()
        );
        Thread myCallable = new Thread(futureTask);
        myCallable.start();
        try {
            String result = futureTask.get();
            System.out.println("[INFO]: myCallable result -> " + result);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    @Override
    public String call() throws Exception {
        for (int i = 0; i < 10; ++i) {
            System.out.println("[INFO]: MyCallable -> " + String.valueOf(i));
        }
        return "MyCallable-" + String.valueOf(this.hashCode());
    }
}
```
> 代码清单：实现`Callable`接口

`Callable`接口是一枚泛型接口，有一个待实现`call()`方法。如果线程正常退出，方法可带回线程执行结果，如果线程执行出错，则抛出异常。

```java
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```
> 代码清单：`Callable`接口源码

`Future`接口是一枚泛型接口，提供了`Callable`线程任务的一些调用方法，如：获取计算结果、取消线程任务、判断任务是否完成...

```java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException;
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
```
> 代码清单：`Future`接口源码

# `Thread`类源码观察

我们观察`Thread`类源码，可以看到，在`Thread`类构造函数中，`Runnable`实例会被传递至内部`target`中，并在线程中实际调用`target.run()`。

```java
...
    /* What will be run. */
    private Runnable target;
...

    /**
     * Initializes a Thread.
     *
     * @param g the Thread group
     * @param target the object whose run() method gets called
     * @param name the name of the new Thread
     * @param stackSize the desired stack size for the new thread, or
     *        zero to indicate that this parameter is to be ignored.
     * @param acc the AccessControlContext to inherit, or
     *            AccessController.getContext() if null
     * @param inheritThreadLocals if {@code true}, inherit initial values for
     *            inheritable thread-locals from the constructing thread
     */
    private Thread(ThreadGroup g, Runnable target, String name,
                   long stackSize, AccessControlContext acc,
                   boolean inheritThreadLocals) {
...
    }
    /**
     * If this thread was constructed using a separate
     * {@code Runnable} run object, then that
     * {@code Runnable} object's {@code run} method is called;
     * otherwise, this method does nothing and returns.
     * <p>
     * Subclasses of {@code Thread} should override this method.
     *
     * @see     #start()
     * @see     #stop()
     * @see     #Thread(ThreadGroup, Runnable, String)
     */
    @Override
    public void run() {
        if (target != null) {
            target.run();
        }
    }
...
```
> 代码清单：`Thread`类部分源码

# 关联文章（Related Topics）

- [了解「多线程」技术](https://zihengcat.github.io/2019/05/12/understanding-multithreading-technology/)

# 参考资料（References）

- The Java™ Tutorials(Concurrency): https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html

