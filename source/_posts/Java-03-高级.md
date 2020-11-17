---
title: Java-03-高级
date: 2020-11-16 22:58:04
categories:
- Java
tags:
- 高级
---

#### Day 001

> <!-- Part 001 -->
>
> ※ 在计算机中，我们把一个任务称为一个<span style="color:red">进程</span>；某些进程内部还需要同时执行多个子任务，我们把这些子任务称为<span style="color:red">线程</span>。进程与线程之间的关系为：一个进程可以包含多个线程，但至少会有一个线程。**注**：操作系统调度的最小任务单位不是进程，而是线程！
>
> ※ 同一个应用程序中，既可以有多个进程，又可以有多个线程，因此，实现多任务的方法有以下几种：<span style="color:blue">多进程模式</span>（每个进程只有一个线程）；<span style="color:blue">多线程模式</span>（一个进程有多个线程）；<span style="color:blue">多进程加多线程模式</span>（每个进程有多个线程）。
>
> ※ 与多线程相比，多进程的缺点在于：创建进程比创建线程开销大，尤其是在`Windows`系统上；进程间通信比线程间通信慢，因为线程间通信是读写同一个变量，速度很快。与多进程相比，多线程的缺点在于：稳定性较差，因为在多进程的情况下，一个进程崩溃不会影响其它进程，而在多线程的情况下，任何一个线程崩溃都将导致整个进程崩溃。
>
> ※ `Java`语言内置了多线程支持：一个`Java`程序实际上是一个`JVM`进程，`JVM`进程用一个主线程来执行`main()`方法，在`main()`方法内部，还可以启动多个线程。此外，`JVM`还有负责垃圾回收的其它工作线程等。**注**：多线程模型是`Java`程序最基本的并发模型，后继读写网络、数据库、`Web`开发等都依赖于`Java`多线程模型。

> <!-- Part 002 -->
>
> ※ 创建新线程有以下三种方式：
>
> 👉 继承`Thread`类，并重写`run()`方法；

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start(); // 启动新线程
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

> 👉 创建实现`Runnable`接口的类，并在创建`Thread`实例时传入；

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start(); // 启动新线程
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

> 👉 借助匿名类。

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

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("start new thread!");
        });
        t.start(); // 启动新线程
    }
}
```

> ※ 调用`run()`方法是无法启动新线程的，只是相当于调用了一个普通的`Java`方法。若想启动新线程，必须调用`start()`方法。**注**：在`Thread`类的源码中可以看到，`start()`方法内部调用了一个`private native void start0()`方法，`native`修饰符表示该方法是由`JVM`虚拟机内部的`C`代码实现的，而非`Java`代码。
>
> ※ 可以对线程设定优先级，设定优先级的方法为：

```java
Thread.setPriority(int n) // 1~10, 默认值5
```

> **注**：操作系统对优先级高的线程调度更为频繁，但并不能保证优先级高的线程一定会先执行。

> <!-- Part 003 -->
>
> ※ `Java`线程的状态有以下几种：<span style="color:blue">`New`</span>，新创建的线程，尚未运行；<span style="color:blue">`Runnable`</span>，运行中的线程，正在执行`run()`方法；<span style="color:blue">`Blocked`</span>：运行中的线程，因为某些操作被阻塞而挂起；<span style="color:blue">`Waiting`</span>：运行中的线程，因为某些操作在等待中；<span style="color:blue">`Timed Waiting`</span>：运行中的线程，因为执行`sleep()`方法在计时等待；<span style="color:blue">`Terminated`</span>：线程已终止。
>
> ※ 线程终止的原因有以下三种：正常终止，即`run`方法执行到`return`语句返回；意外终止，即`run()`方法因未捕获的异常导致线程终止；对某个线程的`Thread`实例调用`stop()`方法强制终止（强烈不推荐使用）。
>
> ※ 通过对另一个线程对象调用`join()`方法可以等待其执行结束，也可以指定等待时间，待超过等待时间后便不再等待。此外，对已经运行结束的线程调用`join()`方法会立即返回。

> <!-- Part 004 -->
>
> ※ 线程在执行一个长时间的任务时，可能需要被中断。<span style="color:blue">中断线程</span>就是其它线程给目标线程发送一个请求，目标线程在收到请求后，结束执行`run()`方法，使得自身线程能立刻结束运行。中断线程非常简单，只需要在其它线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身是否是`interrupted`状态，若是则立刻结束运行。**注**：目标线程能否正确响应其它线程发来的中断请求，需要看具体代码。
>
> ※ 若对处于等待状态的线程调用`interrupt()`方法，线程的`join()`方法将会抛出`InterruptedException`异常。换句话说，若线程捕获到`join()`方法抛出的`InterruptedException`异常，就说明有其它线程对其调用了`interrupt()`方法，通常情况下该线程应立刻结束运行。
>
> ※ 另一个常用的中断线程的方法为设置标志位，即用一个`running`标志位来标识线程是否应该继续运行，在外部线程中，通过把该标志位设置为`false`，便可以让线程结束。**注**：标志位应为线程间共享变量，用关键字`volatile`标记，从而确保每个线程都能读取到更新后的变量值。
>
> ※ 为什么要对线程间共享的变量用`volatile`声明？这涉及到`Java`的内存模型。在`Java`虚拟机中，变量的值保存在主内存中，但是，当线程访问变量时，会先获取一个副本，并保存在自己的工作内存中。若线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时刻是不确定的。这会导致如果一个线程更新了某个变量，另一个线程读取的值可能还是更新前的。因此，`volatile`关键字的目的是告诉虚拟机：每次访问变量时，总是获取内存的最新值；每次修改变量后，立刻回写到主内存。<!-- 存疑 -->

> <!-- Part 005 -->
>
> ※ <span style="color:blue">守护线程</span>是指为其它线程服务的线程。在`JVM`中，待所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。**注**：虚拟机退出后，非守护线程自然也就结束了，换句话说，非守护线程不用刻意去关闭。
>
> ※ 守护线程不能持有任何需要关闭的资源，如打开文件等，因为虚拟机退出时，守护线程没有任何机会来关闭文件，这会导致数据丢失。
>
> ※ 创建守护线程，只需要在调用`start()`方法前，调用`setDaemon(true)`方法将该线程标记为守护线程。

> <!-- Part 006 -->
>
> ※ 当多个线程同时运行时，线程的调度由操作系统决定，程序本身无法决定。因此，任何一个线程都有可能在任何指令处被操作系统暂停，然后在某个时间段后继续执行。这时，若多个线程同时读写共享变量，便会出现数据不一致的问题。对共享变量进行读取和写入时，必须保证是<span style="color:blue">原子操作</span>，即不能被中断的一个或一系列操作。
>
> ※ `Java`程序使用`synchronized`关键字对一个对象进行加锁：

```java
synchronized(lock) { // 获取锁
    ...
} // 释放锁
```

> **注**：在使用`synchronized`的时候，不用担心抛出异常，因为无论是否有异常，都会在`synchronized`结束处正确释放锁。
>
> ※ 虽然`synchronized`解决了多线程同步访问共享变量的正确性问题，但是带来了性能下降，这不但是因为代码块无法并发执行，还因为加锁和解锁需要消耗一定的时间。
>
> ※ `JVM`规范定义了几种原子操作：基本类型赋值（`long`、`double`除外）；引用类型赋值。对于单条原子操作来说并不需要同步！**注**：`long`、`double`是64位数据，`JVM`没有明确规定64位赋值操作是不是同一个原子操作，但是在`X64`平台的`JVM`是把`long`和`double`赋值作为原子操作实现的。

> <!-- Part 007 -->
>
> ※ 让线程自己选择锁对象往往会使代码逻辑混乱，并且不利于封装，更好的方法是把`synchronized`逻辑封起来，即：

```java
public class Counter {
    private int count = 0;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
}
```

> 这样一来，线程调用`add()`方法时，将不必关心同步逻辑，因为`synchronized`代码块在该方法内部。此外，`synchronized`锁住的对象为`this`，即当前实例，这使得创建多个`Counter`实例时，它们之间互不影响。
>
> ※ 若一个类被设计为允许多线程正确访问，则称该类为<span style="color:blue">线程安全</span>的。`Java`标准库中的`StringBuilder`是线程安全的；一些不变类，如`String`，`Integer`，`LocalDate`，它们的所有成员变量都是`final`，多线程访问时只能读不能写，也是线程安全的；此外，类似`Math`这些只提供静态方法，没有成员变量的类，也是线程安全的。
>
> ※ 当锁住的是`this`实例时，实际上可以用`synchronized`修饰这个方法，即：

```java
public synchronized void add(int n) { // 锁住this
    count += n;
} // 解锁
```

> 该方法也被称为同步方法，表示整个方法都必须用`this`实例加锁。
>
> ※ 对于`static`实例，是没有`this`实例的，因为`static`方法是针对类而不是实例。但是注意到任何一个类都有一个由`JVM`自动创建的`Class`实例，因此，对`static`方法添加`synchronized`，锁住的是该类的`Class`实例。<!-- 存疑 -->

> <!-- Part 008 -->
>
> ※ `JVM`允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，称为<span style="color:blue">可重入锁</span>。
>
> ※ 当多线程各自持有不同的锁，并互相试图获取对方已持有的锁时，将导致无限等待，此时便陷入了<span style="color:blue">死锁</span>。
>
> ※ 在编写多线程应用时，要特别注意防止死锁，因为死锁一旦形成，就只能强制结束`JVM`进程。避免死锁的方法为多线程获取锁的顺序要一致！

