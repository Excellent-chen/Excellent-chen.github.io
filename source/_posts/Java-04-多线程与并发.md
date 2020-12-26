---
title: Java多线程与并发
date: 2020-12-07 11:18:41
categories:
- Java
tags:
- 多线程与并发
---

<!-- more -->

#### Day 001

> <!-- Part 001 -->
>
> ※ 在计算机中，我们把一个任务称为一个<span style="color:red">进程</span>；某些进程内部还需要同时执行多个子任务，我们把这些子任务称为<span style="color:red">线程</span>。进程与线程之间的关系为：一个进程可以包含多个线程，但至少会有一个线程。**注**：操作系统调度的最小任务单位不是进程，而是线程！
>
> ※ 同一个应用程序中，既可以有多个进程，又可以有多个线程，因此，实现多任务的方法有以下几种：<span style="color:blue">多进程模式</span>（每个进程只有一个线程）；<span style="color:blue">多线程模式</span>（一个进程有多个线程）；<span style="color:blue">多进程加多线程模式</span>（每个进程有多个线程）。
>
> ※ 与多线程相比，多进程的缺点在于：创建进程比创建线程开销大，尤其是在`Windows`系统上；进程间通信比线程间通信慢，因为线程间通信是读写同一个变量，速度很快。与多进程相比，多线程的缺点在于：稳定性较差，因为在多进程的情况下，一个进程崩溃不会影响其它进程，而在多线程的情况下，任何一个线程崩溃都将导致整个进程崩溃。

> <!-- Part 002 -->
>
> ※ <span style="color:blue">并发</span>是指同一时间应对多件事情的能力；<span style="color:blue">并行</span>是指同一时间动手做多件事情的能力。

> <!-- Part 003 -->
>
> ※ 从方法调用的角度来讲，如果需要等待结果返回，才能继续运行就是<span style="color:blue">同步</span>；不需要等待结果返回，就能继续运行就是<span style="color:blue">异步</span>。

#### Day 002

> <!-- Part 001 -->
>
> ※ 创建线程有以下多种方式：
>
> 👉 继承`Thread`类，并重写`run()`方法；

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread() {
            public void run() {
                System.out.println("start new thread!");
            }
        };
        t.start(); // 启动新线程
    }
}
```

> 👉 实现`Runnable`接口，并在创建`Thread`实例时传入；

```java
public class Main {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            public void run() {
                System.out.println("start new thread!");
            }
        };
        Thread t = new Thread(runnable);
        t.start(); // 启动新线程
    }
}
```

> 👉 实现`Callable`接口，并在创建`Thread`实例时传入；

```java
public class Main {
    public static void main(String[] args) {
        FutureTask<Integer> task = new FutureTask<>(new Callable<Integer>() {
            public Integer call() throws Exception {
                System.out.println("start new thread!");
                return 666;
            }
        });
        Thread t = new Thread(task);
        t.start(); // 启动新线程
    }
}
```

> <!-- Part 002 -->
>
> ※ 在`Windows`下，查看所有进程的命令为：`tasklist`；查看与`Java`有关的所有进程的命令为：`tasklist | findstr java`或`jps`；杀死指定进程的命令为：`taskkill /F /PID ***`，其中，`/F`表示强制杀死，`/PID`后跟想要杀死的进程编号。
>
> ※ 在`Linux`下，查看所有进程的命令为：`ps -fe`或`top`；查看与`Java`有关的所有进行的命令为：`ps -fe | grep java`或`jps`；杀死指定进程的命令为：`kill ***`；利用`top`命令查看某个进程下线程的信息的命令为：`top -H -p ***`，其中，`-H`表示查看线程；此外，也可以利用`jstack ***`命令查看某个进程下线程的信息，只是它是静态的。

> <!-- Part 003 -->
>
> ※ 线程的栈内存是相互独立的，它们之间互不干扰。

> <!-- Part 004 -->
>
> ※ 从`Java API`的层面来看，线程可以分为以下六种状态：`NEW`、`BLOCKED`、`WAITING`、`TIMED_WAITING`、`RUNNABLE`、`TERMINATED`。

#### Day 003

> <!-- Part 001 -->
>
> ※ 一段代码块内如果存在对共享资源的多线程读写操作，称这段代码为<span style="color:blue">临界区</span>。
>
> ※ 多个线程在临界区内执行，由于代码的执行序列不同而导致结果无法预测，称之为发生了<span style="color:blue">竞态条件</span>。

> <!-- Part 002 -->
>
> ※ 为了避免临界区竞态条件的发生，有多种手段可以达到目的：阻塞式解决方案，`synchronized`、`Lock`；非阻塞式方案：原子变量。

> <!-- Part 003 -->
>
> ※ `synchronized`加在方法上相当于锁住了`this`对象，加载静态方法上相当于锁住了类对象。**注**：锁住两者相当于锁住了不同的对象！

> <!-- Part 004 -->
>
> ※ <span style="color:blue">线程安全类</span>是指多个线程调用同一个实例的某个方法时，是线程安全的。

> <!-- Part 005 -->
>
> ※ `Java`普通对象在`32`位虚拟机中的对象头大小为`8`个字节，数组对象在`32`位的虚拟机中的对象头大小为`12`个字节。
>
> ※ 每个`Java`对象都可以关联一个`Monitor`对象，在使用`synchronized`给对象上锁（重量级）后，该对象头的`Mark Word`就被设置指向`Monitor`对象的指针。

> <!-- Part 006 -->
>
> ※ 若一个对象虽然有多个线程访问，但是访问的时间是错开的（也就是没有竞争），那么可以使用<span style="color:blue">轻量级锁</span>来优化。**注**：轻量级锁对使用者是透明的，语法仍然是`synchronized`。

> <!-- Part 007 -->
>
> ※ `obj.wait()`可以让进入`object`监视器的线程到`waitSet`中等待；`obj.notify()`可以在`object`上正在`waitSet`中等待的线程中挑一个唤醒；`obj.notifyAll()`可以让`object`上正在`waitSet`等待的线程全部唤醒。**注**：它们都是线程之间进行协作的手段，都属于`Object`对象的方法，必须获得此对象的锁，才能调用这几个方法！

> <!-- Part 008 -->
>
> ※ `sleep(long n)`与`wait(long n)`的区别：`sleep`是`Thread`的方法，而`wait`是`Object`的方法；`sleep`不需要强制配合`synchronized`使用，而`wait`需要强制配合`synchronized`使用；`sleep`在睡眠时不会释放对象锁，只是放弃了`CPU`的使用，而`wait`在等待的同时会释放对象锁。**注**：在`sleep`/`wait`后，均会进入`TIMED_WAITING`状态。
>
> ※ `wait`、`notify`模板：

```java
synchronized(lock) {
    while (条件不成立) {
        lock.wait();
    }
    // do something
}

synchronized(lock) {
    lock.notifyAll();
}
```

> <!-- Part 009 -->
>
> ※ 模式：同步模式之保护性暂停；异步模式之生产者消费者。

> <!-- Part 010 -->
>
> ※ `park`、`unpark`与`wait`、`notify`的功能类似，但是存在以下不同：`wait`，`notify`必须配合对管程一起使用，而`park`、`unpark`不必；`park`、`unpark`是以线程为单位来“阻塞”和“唤醒”线程，而`notify`则没有那么精确；`park`、`unpark`可以先`unpark`，而`wait`，`notify`不能先`notify`。

> <!-- Part 011 -->
>
> ※ 线程之间的<span style="color:red">状态转换</span>！

#### Day 005

> <!-- Part 001 -->
>
> ※ <span style="color:red">`Java`内存模型</span>定义了工作内存（线程私有的数据）、主存（线程共享的数据）抽象概念，底层对应着`CPU`寄存器、缓存、硬件内存、`CPU`指令优化等。

> <!-- Part 002 -->
>
> ※ `volatile`关键字用于修饰成员变量和静态成员变量，能够避免线程从自己的工作内存中查找变量的值。**注**：`synchronized`既可以保证代码块的原子性，也可以保证代码块内变量的可见性，但缺点是其属于重量级操作，性能相对较低。

> <!-- Part 003 -->
>
> ※ `volatile`的底层实现原理是<span style="color:blue">内存屏障</span>，对`volatile`变量的写指令后会加入写屏障，对`volatile`变量的读指令前会加入读屏障。其中，写屏障保证在该屏障之前的，对共享变量的改动，都会同步到主存当中；读屏障保证在该屏障之后，对共享变量的读取，加载的是主存中的最新数据。

#### Day 008

> <!-- Part 001 -->
>
> ※ 

