---
title: Java 高级教程系列 - 线程生命周期
subtitle: Java Tutorial for Language Adavanced - Life Cycle of Thread in Java
catalog: true
date: 2019-07-16
tags: ["Java", "Java Tutorial"]
---

# Java 线程生命周期（Life Cycle）

在 Java 中，一枚线程可处于如下几种状态，线程也可以进行状态切换。

| States          | Descriptions                                                                                                          |
| --------------- | --------------------------------------------------------------------------------------------------------------------- |
| `NEW`           | A thread that has not yet started is in this state.                                                                   |
| `RUNNABLE`      | A thread executing in the Java virtual machine is in this state.                                                      |
| `BLOCKED`       | A thread that is blocked waiting for a monitor lock is in this state.                                                 |
| `WAITING`       | A thread that is waiting indefinitely for another thread to perform a particular action is in this state.             |
| `TIMED_WAITING` | A thread that is waiting for another thread to perform an action for up to a specified waiting time is in this state. |
| `TERMINATED`    | A thread that has exited is in this state.                                                                            |

> 表：Java 线程状态表

![Thread_LifeCycle_in_Java](./thread_lifecycle_in_java.jpg)

> 图：Java 线程生命周期（Life Cycle）示意图

```java
...
    public enum State {
        /**
         * Thread state for a thread which has not yet started.
         */
        NEW,

        /**
         * Thread state for a runnable thread.  A thread in the runnable
         * state is executing in the Java virtual machine but it may
         * be waiting for other resources from the operating system
         * such as processor.
         */
        RUNNABLE,

        /**
         * Thread state for a thread blocked waiting for a monitor lock.
         * A thread in the blocked state is waiting for a monitor lock
         * to enter a synchronized block/method or
         * reenter a synchronized block/method after calling
         * {@link Object#wait() Object.wait}.
         */
        BLOCKED,

        /**
         * Thread state for a waiting thread.
         * A thread is in the waiting state due to calling one of the
         * following methods:
         * <ul>
         *   <li>{@link Object#wait() Object.wait} with no timeout</li>
         *   <li>{@link #join() Thread.join} with no timeout</li>
         *   <li>{@link LockSupport#park() LockSupport.park}</li>
         * </ul>
         *
         * <p>A thread in the waiting state is waiting for another thread to
         * perform a particular action.
         *
         * For example, a thread that has called {@code Object.wait()}
         * on an object is waiting for another thread to call
         * {@code Object.notify()} or {@code Object.notifyAll()} on
         * that object. A thread that has called {@code Thread.join()}
         * is waiting for a specified thread to terminate.
         */
        WAITING,

        /**
         * Thread state for a waiting thread with a specified waiting time.
         * A thread is in the timed waiting state due to calling one of
         * the following methods with a specified positive waiting time:
         * <ul>
         *   <li>{@link #sleep Thread.sleep}</li>
         *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
         *   <li>{@link #join(long) Thread.join} with timeout</li>
         *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
         *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
         * </ul>
         */
        TIMED_WAITING,

        /**
         * Thread state for a terminated thread.
         * The thread has completed execution.
         */
        TERMINATED;
    }
...
```
> 代码清单：`Thread.States`源码

# 关联文章（Related Topics）

- [了解「多线程」技术](https://zihengcat.github.io/2019/05/12/understanding-multithreading-technology/)

- [Java 高级教程系列 - 线程创建](https://zihengcat.github.io/2019/07/14/java-tutorial-for-language-adavanced-create-thread/)

# 参考资料（References）

- The Java™ Tutorials(Concurrency): https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html

- Enum `Thread.State`: https://docs.oracle.com/javase/8/docs/api/index.html?java/lang/Thread.State.html

