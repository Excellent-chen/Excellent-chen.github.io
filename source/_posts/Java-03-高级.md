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

> <!-- Part 009 -->
>
> ※ 多线程协调运行的原则为：当条件不满足时，线程进入等待状态；当条件满足时，线程被唤醒，继续执行任务。
>
> ※ `wait`和`notify`用于多线程协调运行，在`synchronzied`内部可以调用`wait()`使线程进入等待状态，但必须在已获得的锁对象上调用`wait()`方法；在`synchronzied`内部可以调用`notify()`或`notifyAll()`唤醒其它等待线程，但必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法。**注**：已唤醒的线程需要重新获得锁后才能继续执行。<!-- 存疑（你给我解释解释，什么TM的叫TM的已获得的锁对象） -->

> <!-- Part 010 -->
>
> ※ `ReentrantLock`可以用于替代`synchronized`进行同步，与`synchronized`相比，`ReentrantLock`可以在指定时间范围内尝试获取锁，一旦超过指定时间范围，程序可以做一些额外处理，而不是无限等待下去，这样可以有效避免死锁：

```java
public class Counter {
    private int count;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
}
```

```java
public class Counter {
    private final Lock lock = new ReentrantLock();
    private int count;

    public void add(int n) {
        lock.lock(); // lock() 方法表示当前线程占用 lock 对象
        try {
            count += n;
        } finally {
            lock.unlock(); // 必须调用 unlock() 方法进行手动释放
        }
    }
}
```

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        ...
    } finally {
        lock.unlock();
    }
}
```

> <!-- Part 011 -->
>
> ※ `ReentrantLock`可以替代`synchronized`进行同步，那么该如果实现`synchronized`下的`wait`和`notify`呢？可以使用`Condition`：

```java
class TaskQueue {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

> 可见，使用`Condition`时，引用的`Condition`对象必须从`Lock`实例的`newCondition()`返回，这样才能获得一个绑定了`Lock`实例的`Condition`对象。
>
> ※ `Condition`提供的`await()`、`signal()`、`signalAll()`方法在原理上和`synchronized`锁对象提供的`wait()`、`notify()`、`notifyAll()`方法是一致的，并且行为也是一样的：`await()`用于释放当前锁并进入等待状态；`signal()`用于唤醒某个等待线程；`signalAll()`用于唤醒所有等待线程。**注**：与`trylock()`类似，`await()`可以在等待指定时间后自己醒来。

>  <!-- Part 012 -->
>
> ※ 在读多写少的场景中，利用`ReadWriteLock`可以提高读取效率：

```java
public class Counter {
    private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
    private final Lock rlock = rwlock.readLock();
    private final Lock wlock = rwlock.writeLock();
    private int[] counts = new int[10];

    public void inc(int index) {
        wlock.lock(); // 加写锁
        try {
            counts[index] += 1;
        } finally {
            wlock.unlock(); // 释放写锁
        }
    }

    public int[] get() {
        rlock.lock(); // 加读锁
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            rlock.unlock(); // 释放读锁
        }
    }
}
```

> **注**：在论坛中，回复可以视为写入操作，它是不频繁的，浏览可以视为读取操作，它是频繁的，这种情况就可以使用`ReadWriteLock`。

> <!-- Part 013 -->
>
> ※ 在`Java 8`中，引入了新的读写锁`StampedLock`，与`ReadWriteLock`相比，前者的改进之处在于：读的过程中也允许获取写锁后写入。但是这样可能会导致读的数据不一致，因此需要一点儿额外的代码来判断读的过程中是否有写入：

```java
public class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock(); // 获取写锁
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp); // 释放写锁
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
        // 注意下面两行代码不是原子操作
        // 假设x,y = (100,200)
        double currentX = x;
        // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
        double currentY = y;
        // 此处已读取到y，如果没有写入，读取是正确的(100,200)
        // 如果有写入，读取是错误的(100,400)
        if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
            stamp = stampedLock.readLock(); // 获取一个悲观读锁
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp); // 释放悲观读锁
            }
        }
        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```

> 与`ReadWriteLock`相比，写入的加锁是完全一致的，不同的是读取。首先，通过`tryOptimisticRead()`获取一个乐观读锁，并返回版本号；接着，待读取完成后，通过`validate()`去验证版本号，若在读取过程中没有写入，版本号不变，验证成功；若在读取过程中有写入，版本号改变，验证失败，此时将再通过获取悲观读锁进行读取。可见，`StampedLock`将读锁细分为乐观读锁和悲观读锁，其中，<span style="color:blue">乐观读锁</span>是指乐观地估计读的过程中大概率不会有写入；<span style="color:blue">悲观读锁</span>是指读的过程中拒绝有写入。然而，如此细分也是有代价的：一是代码更加复杂，二是`StampedLock`为不可重入锁，不能在一个线程中反复获取同一个锁！

> <!-- Part 014 -->
>
> ※ 针对`List`、`Map`、`Set`、`Queue`、`Deque`等，`java.util.concurrent`包提供了对应的并发集合类：
>
> | interface |     non-thread-safe     |               thread-safe                |
> | :-------: | :---------------------: | :--------------------------------------: |
> |  `List`   |       `ArrayList`       |          `CopyOnWriteArrayList`          |
> |   `Map`   |        `HashMap`        |           `ConcurrentHashMap`            |
> |   `Set`   |    `HashSet/TreeSet`    |          `CopyOnWriteArraySet`           |
> |  `Queue`  | `ArrayDeque/LinkedList` | `ArrayBlockingQueue/LinkedBlockingQueue` |
> |  `Deque`  | `ArrayDeque/LinkedList` |          `LinkedBlockingDeque`           |
>
> 使用这些并发集合与使用非线程安全的集合类完全相同。
>
> ※ `java.util.Collections`工具类还提供了一个旧的线程安全集合转换器：

```java
Map unsafeMap = new HashMap();
Map threadSafeMap = Collections.synchronizedMap(unsafeMap);
```

> 该转换器实际上只是用一个包装类包装了非线程安全的`Map`，然后对所有读写方法都用`synchronized`加锁，这样获取的线程安全集合的性能比`java.util.concurrent`集合要低很多，不推荐使用！

> <!-- Part 015 -->
>
> ※ `java.util.concurrent`包除了提供底层锁、并发集合外，还提供了一组原子操作的封装类，这些封装类位于`java.util.concurrent.atomic`包中。
>
> ※ `Atomic`类是通过**无锁**的方式实现的线程安全访问，它的主要原理是利用了`CAS(Compare and Set)`。其中，`CAS`是指若`AtomicInteger`的当前值为`prev`，那么就更新为`next`，返回`true`；若`AtomicInteger`的当前值不为`prev`，就什么都不干，返回`false`。通过`CAS`操作并配合`do ... while`循环，即使其它线程修改了`AtomicInteger`的值，最终的结果也是正确的。**注**：该类适用于计数器，累加器等。

> <!-- Part 016 -->
>
> ※ `Java`语言虽然内置了多线程支持，启动一个新线程非常方便，但是，创建线程需要操作系统资源（线程资源，栈空间等），频繁创建和销毁线程需要消耗大量时间。如果可以复用一组线程，那么就可以把很多小任务让一组线程来执行，而不是一个任务对应一个新线程，这种能接收大量小任务并进行分发处理的就是<span style="color:blue">线程池</span>。换句话说，线程池内部维护了若干个线程，没有任务的时候，这些线程均处于等待状态，有新任务的时候，就分配一个空闲线程执行。若所有线程均处于忙碌状态，新任务要么放入队列等待，要么增加一个新线程进行处理。
>
> ※ `Java`标准库提供了`ExecutorService`接口表示线程池，其典型用法如下：

```java
// 创建固定大小的线程池:
ExecutorService executor = Executors.newFixedThreadPool(3);
// 提交任务:
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.submit(task4);
executor.submit(task5);
```

> 对于`ExecutorService`接口，`Java`标准库提供的几个常用实现类有：`FixedThreadPoll`，线程数固定的线程池；`ChchedThreadPoll`，线程数根据任务动态调整的线程池；`SingleThreadExecutor`，仅单线程执行的线程池。
>
> ※ 线程池在程序结束的时候要关闭！使用`shutdown()`方法关闭线程池时，会等待正在执行的任务完成后再关闭；使用`shutdownNow()`方法关闭线程池时，会立即停止正在执行的任务；使用`awaitTermination()`方法关闭线程池时，会等待指定的时间让线程池关闭。
>
> ※ 还有一种任务，需要定期反复执行，如每秒刷新证券的价格。这种任务本身固定，需要反复执行，可以使用`ScheduledThreadPool`，放入其中的任务可以定期反复执行：

```java
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
// 1秒后执行一次性任务:
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);
// 2秒后开始执行定时任务，每3秒执行:
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);
// 2秒后开始执行定时任务，以3秒为间隔执行:
ses.scheduleWithFixedDelay(new Task("fixed-delay"), 2, 3, TimeUnit.SECONDS);
```

> 注意`FixedRate`和`FixedDelay`的区别：前者是指任务总是以固定时间间隔触发，不管执行时间有多长；后者是指上一次任务执行完毕后，再等待固定的时间间隔执行任务。<span style="color:red">**注**</span>：在`FixedRate`模式下，假设任务的执行时间超过了其运行周期，则后续执行可能会延迟开始，但不会并发执行；此外，若任务的任何执行遇到了异常，将禁止后续任务的执行。
>
> ※ `Java`标准库提供的`java.util.Timer`类同样可以定期执行任务，但是，一个`Timer`只能定期执行一个任务，多个定时任务必须启动多个`Timer`，而一个`ScheduledThreadPoll`可以调度多个定时任务，因此完全可以用`ScheduledThreadPoll`取代旧的`Timer`。

> <!-- Part 017 -->
>
> ※ 使用`Java`标准库提供的线程池执行多个任务是非常方便的，我们提交的任务只需要实现`Runnable`接口即可让线程池去执行：

```java
class Task implements Runnable {
    public String result;

    public void run() {
        this.result = longTimeCalculation(); 
    }
}
```

> 但是，`Runnable`接口没有返回值，如果任务需要一个返回结果，那么只能保存到变量，并提供额外的方法读取。为此，`Java`标准库提供了一个`Callable`接口，与`Runnable`接口相比，它多了一个返回值：

```java
class Task implements Callable<String> {
    public String call() throws Exception {
        return longTimeCalculation(); 
    }
}
```

> 并且`Callable`接口是一个泛型接口，可以返回指定类型的结果。那么问题来了，该如何获取异步执行的结果呢？如果仔细查看`ExecutorService.submit()`方法，可以看到，它返回了一个`Future`类型的实例，该实例代表一个未来能获取结果的对象：

```java
ExecutorService executor = Executors.newFixedThreadPool(4); 
// 定义任务:
Callable<String> task = new Task();
// 提交任务并获得Future:
Future<String> future = executor.submit(task);
// 从Future获取异步执行返回的结果:
String result = future.get(); // 可能阻塞
```

> 当我们提交一个`Callable`任务后，同时会获得一个`Future`对象，然后，我们在主线程某个时刻调用该对象的`get()`方法，即可获得异步执行的结果。**注**：在调用`get()`时，如果异步任务已经执行完成，则可以直接获得结果；如果异步任务尚未执行完成，那么`get()`会阻塞，直到任务完成后才返回结果。
>
> ※ 一个`Future<V>`接口表示一个未来可能会返回的结果，它定义的方法有：`get()`，获取结果，但可能会等待；`get(long timeout, TimeUnit unit)`，获取结果，但只等待指定的时间；`isDone()`，判断任务是否已经完成；`cancel(boolean mayInterruptIfRunning)`，取消当前任务。

> <!-- Part 018 -->
>
> ※ 使用`Future`获得异步执行结果时，要么调用阻塞方法`get()`，要么轮询查看`isDone()`是否为`true`，这两种方法均不是很好，因为主线程也会被迫等待。从`Java 8`引入了`CompletableFuture`，它对`Future`进行了改进，可以传入回调对象，当异步任务完成或者发生异常时，将自动调用回调对象的回调方法。
>
> ※ 创建一个`CompletableFuture`是通过`CompletableFuture.supplyAsync()`实现的，它需要一个实现了`Supplier`接口的对象。紧接着，`CompletableFuture`已经被提交给默认的线程池执行了，我们需要定义的是`CompletableFuture`完成时和异常时需要回调的实例。完成时，`CompletableFuture`会调用`Consumer`对象；异常时，`CompletableFuture`会调用`Function`对象。
>
> ※ 与`Future`相比，`CompletableFuture`更大的优势在于：既可以串行化执行，又可以并行化执行。具体地，`thenApplyAsync()`方法用于串行化另一个`CompletableFuture`；`anyOf()`和`allOf()`方法用于并行化多个`CompletableFuture`。

> <!-- Part 019 -->
>
> ※ `Java 7`开始引入了一种新的`Fork/Join`线程池，它可以执行一种特殊的任务：将一个大任务拆成多个小任务并行执行。**注**：任务类必须继承自`RecursiveTask`或`RecursiveAction`。

> <!-- Part 020 -->
>
> ※ `Thread`对象代表一个线程，在代码中调用`Thread.currentThread()`方法可以获取当前线程。
>
> ※ 在一个线程中，横跨若干种方法调用，需要传递的对象，通常称之为上下文，它是一种状态，可以是用户身份，也可以是任务信息等。给每个方法增加一个上下文参数非常麻烦，而且有些时候，如果调用链有无法修改源码的第三方库，上下文参数就传不进去了。`Java`标准库提供了一个特殊的`TheadLocal`，它可以在一个线程中传递同一个对象。
>
> ※ `ThreadLocal`实例通常以静态字段初始化，其典型使用方式如下：

```java
static ThreadLocal<User> threadLocalUser = new ThreadLocal<>();

void processUser(user) {
    try {
        threadLocalUser.set(user);
        step1();
        step2();
    } finally {
        threadLocalUser.remove();
    }
}
```

> 通过设置一个`User`实例关联到`ThreadLocal`中，在移除之前，所有方法都可以随时获取到该`User`实例：

```java
void step1() {
    User u = threadLocalUser.get();
    log();
    printUser();
}

void log() {
    User u = threadLocalUser.get();
    println(u.name);
}

void step2() {
    User u = threadLocalUser.get();
    checkUser(u.id);
}
```

> **注**：`ThreadLocal`一定要在`finally`中清除，因为当前线程执行完相关代码后，很可能会被重新放入线程池中，若`ThreadLocal`没有被清除，会把上一次的状态带进去。
>
> ※ 为了保证能释放`ThreadLocal`关联的实例，可以通过`AutoCloseable`接口配合`try (resource) {...}`结构，让编译器自动为我们关闭：

```java
public class UserContext implements AutoCloseable {

    static final ThreadLocal<String> ctx = new ThreadLocal<>();

    public UserContext(String user) {
        ctx.set(user);
    }

    public static String currentUser() {
        return ctx.get();
    }

    @Override
    public void close() {
        ctx.remove();
    }
}
```

> 使用的时候，借助`try (resource) {...}`结构，可以这么写：

```java
try (var ctx = new UserContext("Bob")) {
    // 可任意调用UserContext.currentUser():
    String currentUser = UserContext.currentUser();
} // 在此自动调用UserContext.close()方法释放ThreadLocal关联对象
```

> 这样就在`UserContext`中完全封装了`ThreadLocal`，外部代码在`try (resource) {...}`内部可以随时调用`UserContext.currentUser()`获取当前线程绑定的用户名。

#### Day 002

> <!-- Part 001 -->
>
> ※ `Maven`是一个`Java`项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建。
>
> ※ 使用`Maven`管理的普通`Java`项目的目录结构默认如下：

```shell
a-maven-project
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
│       ├── java
│       └── resources
└── target
```

> 其中，项目的根目录`a-maven-project`为项目名，它有一个<span style="color:blue">项目描述文件</span>`pom.xml`，存放源码的目录为`src/main/java`，存放资源文件的目录为`src/main/resources`，存放测试源码的目录为`src/test/java`，存放测试资源文件的目录为`src/tset/resources`，最后，所有编译、打包生成的文件都放在`target`目录里。
>
> ※ 通常情况下，项目描述文件`pom.xml`的内容如下所示：

```xml
<project ...>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itranswarp.learnjava</groupId>
	<artifactId>hello</artifactId>
	<version>1.0</version>
	<packaging>jar</packaging>
	<properties>
        ...
	</properties>
	<dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
	</dependencies>
</project>
```

> 其中，`groupId`类似于`Java`的包名，通常是公司或组织名称，`artifactId`类似于`Java`的类名，通常是项目名称，再加上`version`，即可作为唯一标识。在引用其它第三方库时，也是通过这三个变量确定的，如`commons-logging`：

```xml
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```

> 使用`<dependency>`声明一个依赖后，`Maven`就会自动下载这个依赖包并把它方法`classpath`中。

> <!-- Part 002 -->
>
> ※ `Maven`定义了几种依赖关系，分别是`compile`、`test`、`runtime`和`provided`：其中，默认的`compile`依赖最常用，`Maven`会把这种类型的依赖直接放入`classpath`中；`test`依赖表示仅在测试时使用，正常运行时不需要，如`JUnit`；`runtime`依赖表示编译时不需要，但运行时需要，如`MySQL`；`provided`依赖表示编译时需要，但运行时不需要，如`servle-api`。
>
> ※ `Maven`通过对`jar`包进行`PGP`签名确保任何一个`jar`包一经发布就无法修改，修改`jar`包的唯一方法为发布一个新版本。因此，某个`jar`包一旦被`Maven`下载过，即可永久地安全缓存在本地。**注**：只有以`-SNAPSHOT`结尾的版本号会被`Maven`视为开发版本，开发版本每次都会重复下载，这种`SNAPSHOT`版本只能用于内部私有的`Maven repo`，公开发布的版本不允许出现。
>
> ※ `Maven`并不会每次都从中央仓库下载`jar`包，一个`jar`包一旦被下载过，就会被`Maven`自动缓存在本地目录（用户主目录下的`.m2`目录）。因此，除第一次编译时因为下载需要时间会比较慢外，后续过程因为有本地缓存，并不会很慢。
>
> ※ 使用`Maven`镜像仓库需要一个配置，在用户主目录下进入`.m2`目录，创建一个`setting.xml`配置文件，内容如下：

```xml
<settings>
    <mirrors>
        <mirror>
            <id>aliyun</id>
            <name>aliyun</name>
            <mirrorOf>central</mirrorOf>
            <!-- 国内推荐阿里云的Maven镜像 -->
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
    </mirrors>
</settings>
```

> 配置镜像仓库后，`Maven`的下载速度会很快。
>
> ※ 如果需要引用一个第三方组件，可以通过<a href="[Maven Central Repository Search](https://search.maven.org/)">search.maven.org</a>搜索关键字，找到对应的组件后直接复制即可。

> <!-- Part 003 -->
>
> ※ `Maven`的<span style="color:blue">生命周期</span>由一系列阶段（`phase`）构成，以内置的生命周期`default`为例，它包含以下<a href="[构建流程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1309301196980257)">阶段</a>。如果我们运行`mvn package`，`Maven`就会执行`default`生命周期，它会从开始一直运行到`package`这个阶段为止；如果我们运行`mvn compile`，`Maven`也会执行`default`生命周期，但这次它只会运行到`compile`。此外，`Maven`另一个常用的生命周期为`clean`，它会执行三个阶段：`pre-clean`、`clean`、`post-clean`。
>
> ※ 在实际开发中，常用的命令有：`mvn clean`，清理所有生成的`class`和`jar`；`mvn clean compile`，先清理再执行到`compile`；`mvn clean test`，先清理再执行到`test`；`mvn clean package`：先清理再执行到`package`。**注**：大多数阶段在执行过程中，因为通常没有在`pom.xml`中配置相关设置，因此什么事情都不做，经常用到的阶段其实只有几个，如`clean`、`compile`、`test`、`package`。

> <!-- Part 004 -->
>
> ※ 实际上，执行每个阶段，都是通过某个插件来执行的，`Maven`本身并不知道如何执行`compile`，它只是负责找到对应的`compiler`插件，然后执行默认的`compiler:compile`这个`goal`来完成编译。因此，使用`Maven`实际上就是配置好需要使用的插件，然后通过阶段调用它们。**注**：若标准插件无法满足需求，还可以使用自定义插件。使用自定义插件时，需要声明。

> <!-- Part 005 -->
>
> ※ `Maven`可以有效管理多个模块，只需要把每个模块当作一个独立的`Maven`项目，每个项目有各自独立的`pom.xml`。
>
> ※ 不同模块的`pom.xml`高度相似，因此，可以提取出共同部分作为`parent`：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itranswarp.learnjava</groupId>
    <artifactId>parent</artifactId>
    <version>1.0</version>
    <packaging>pom</packaging>

    <name>parent</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.28</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.5.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

> `parent`的`<packing>`是`pom`而不是`jar`，因为`parent`本身不含任何`Java`代码。编写`parent`的`pom.xml`只是为了在各个模块中减少重复的配置。

> <!-- Part 006 -->
>
> ※ 安装完`Maven`之后，系统所有项目都会使用全局安装的这个`Maven`版本，但是对于某些项目来说，可能必须使用某个特定的`Maven`版本，这个时候就可以使用`Maven Wrapper`（<span style="color:blue">`mvnw`</span>），它可以负责给这些项目安装指定版本的`Maven`，而其它项目不受影响。`mvnw`的另一个作用是把项目的`mvnw`、`mvnw.cmd`和`.mvn`提交到版本库中，可以使所有开发人员使用统一的`Maven`版本。

> <!-- Part 007 -->
>
> ※ 使用`Maven`发布一个`Artifact`时，可以发布到本地，然后由静态服务器提供`repo`服务，使用方必须声明`repo`地址；可以发布到`central.sonatype.org`，并自动同步到`Maven`中央仓库，需要前期申请账号以及本地配置；可以发布到`Github Packages`作为私有仓库使用，必须提供`Token`以及正确的权限才能使用和发布。

#### Day 003

> <!-- Part 001 -->
>
> ※ 使用`Java`进行网络编程时，由虚拟机实现了底层复杂的网络协议，`Java`程序只需要调用`Java`标准库提供的接口，即可简单高效地编写网络程序。
>
> ※ `TCP/IP`协议泛指互联网协议，其中最重要的两个协议是`TCP`协议和`IP`协议。只有使用`TCP/IP`协议的计算机才能够联入互联网，使用其它网络协议是无法联入互联网的。
>
> ※ 在互联网中，一个`IP`地址用于唯一标识一个网络接口。一台连入互联网的计算机肯定有一个`IP`地址，但也可能有多个`IP`地址。`IP`地址既可以分为`IPv4`和`IPv6`，又可以分为公网`IP`地址和内网`IP`地址。公网`IP`地址可以直接被访问，内网`IP`地址只能在内网访问。内网`IP`地址类似于：`192.168.*.*`，`10.*.*.*`。
>
> ※ 如果一台计算机有两块网卡，那么除了本机地址`127.0.0.1`，它还可以有两个`IP`地址，这两个`IP`地址可以分别接入两个网络，使得它们连接起来。**注**：通常连接两个网络的设备是路由器或者交换机。
>
>  ※ 每台计算机都需要正确配置`IP`地址和子网掩码，根据它们可以计算网络号。若两台计算机计算出的网络号相同，说明两台计算机在同一个网络，可以直接通信；若两台计算机计算出的网络号不同，说明两台计算机不在同一个网络，不能直接通信，它们之间必须通过路由器或者交换机这样的网络设备间接通信，这种设备通常被称为<span style="color:blue">网关</span>。网关的作用就是连接多个网络，负责把来自一个网络的数据包发到另一个网络，这个过程叫做<span style="color:blue">路由</span>。
>
> ※ 域名解析服务器`DNS`负责把域名翻译成对应的`IP`，客户端再根据`IP`地址访问服务器。**注**：用`nslookup`可以查看域名对应的`IP`地址。
>
> ※ `OSI`网络模型是`ISO`组织定义的一个计算机互联标准模型，目的是为了简化网络各层的操作，提供标准接口便于实现和维护，该模型从上到下依次是：应用层，提供应用程序之间的通信；表示层，处理数据的格式，加解密等；会话层，负责建立和维护对话；传输层，负责提供端到端的可靠传输；网络层，负责根据目标地址选择路由来传输数据；链路层和物理层负责把数据进行分片并且真正通过物理网络传输，如无线网和光纤等。互联网使用的`TCP/IP`模型并不是对应到`OSI`的`7`层模型，而是大致对应`OSI`的`5`层模型。
>
> ※ `IP`协议是一种分组交换传输协议；`TCP`协议是一种面向连接，可靠传输的协议；`UDP`协议是一种非面向连接，不可靠传输的协议。

> <!-- Part 002 -->
>
> ※ `Socket`是一个抽象概念，应用程序通过一个`Socket`来建立一个远程连接，而`Socket`内部通过`TCP/IP`协议把数据传输到网络。
>
> ※ 一个`Socket`就是由`IP`地址和端口号组成，端口号总是由操作系统分配，它是`0 ~ 65535`之间的数字，其中，小于`1024`的端口属于特权端口，需要管理员权限，大于`1024`的端口可以由任意用户的应用程序打开。
>
> ※ 使用`Socket`进行网络编程时，本质上就是两个进程之间的网络通信。其中一个进程必须充当服务器，它会主动监听某个指定的端口，另一个进行必须充当客户端，它必须主动连接服务器的`IP`地址和指定端口，如果连接成功，服务器和客户端就成功地建立了一个`TCP`连接，双方后继就可以随时发送和接收数据。因此，当`Socket`连接成功地再服务器和客户端之间建立后，对服务器来说，它的`Socket`是指定的`IP`地址和指定的端口号；对客户端来说，它的`Socket`是它所在计算机的`IP`地址和一个由操作系统分配的随机端口号。
>
> ※ 当`Socket`连接建立成功后，无论是服务器，还是客户端，都使用`Socket`实例进行网络通信。因为`TCP`是一种基于流的协议，因此，`Java`标准库使用`InputStream`和`OutputStream`来封装`Socket`的数据流。
>
> ※ 使用`Java`进行`TCP`编程时，需要使用`Socket`模型：服务器用`ServerSocket`监听指定端口；客户端使用`Socket(InetAddress, port)`连接服务器；服务器用`accept()`接收连接并返回`Socket`；双方通过`Socket`打开`InputStream/OutputStream`读写数据。**注**：服务器通常使用多线程同时处理多个客户端连接，利用线程池可大幅提升效率。
>
> ※ 在写入网络数据时，要调用`flush()`方法，若不调用，可能会发现客户端和服务器都收不到数据，这是因为我们以流的形式写入数据时，并不是一写入就立刻发送到网络，而是先写入到内存缓冲区，直到缓冲区满了以后，才会一次性真正发送到网络，这样设计的目的是提高传输效率。若缓冲区的数据很少，而我们又想把这些数据发送到网络，就必须调用`flush()`强制把缓冲区数据发送出去。

> <!-- Part 003 -->
>
> ※ 与`TCP`相比，`UDP`没有创建连接，数据包也是一次收发一个，因此没有流的概念。
>
> ※ 在`Java`中使用`UDP`编程，仍然需要使用`Socket`，因为应用程序在使用`UDP`时必须指定`IP`地址和端口号。**注**：`UDP`端口和`TCP`端口虽然都使用`0 ~ 65535`，但他们属于两套独立的端口，即一个应用程序用`TCP`占用了端口`1234`，不影响另一个应用程序用`UDP`占用端口`1234`。
>
> ※ 服务器使用`DatagramSocket(port)`监听端口；客户端使用`DatagramSocket.connect`指定远程地址和端口；双方通过`receive()`和`send()`读写数据；`DatagramSocket`没有`IO`流接口，数据被直接写入`byte[]`缓冲区。

> <!-- Part 004 -->
>
> ※ `SMTP`协议是一个建立在`TCP`协议之上的协议，任何程序发送邮件都必须遵守`SMTP`协议。

> <!-- Part 005 -->
>
> ※ `HTTP`是目前`	Web`应用程序使用最广泛的基础协议，翻译为超文本传输协议。
>
> ※ 当浏览器希望访问某个网站时，浏览器和网站服务器之间首先建立`TCP`连接，并且服务器总是使用`80`端口和加密端口`443`；然后，浏览器向服务器发送一个`HTTP`请求，服务器收到后返回一个`HTTP`响应，并且在响应中包含了`HTML`网页内容，这样，浏览器解析`HTML`后就可以给用户显示网页了。
>
> ※ `HTTP`请求的格式是固定的，由`HTTP Header`和`HTTP Body`两部分构成。第一行总是“请求方法 路径 `HTTP`版本”，后继的每一行都是固定的`Header: Value`格式，称之为`Header`，服务器依靠某些特定的`Header`来识别客户端请求：`Host`，表示请求的域名，因为一台服务器上可能有多个网站，有必要依靠`Host`来识别请求；`User-Agent`，表示客户端自身标识信息，不同的浏览器有不同的标识，服务器依靠其判断客户端类型；`Accept`，表示客户端能处理的`HTTP`响应格式，`*/*`表示惹你格式，`text/*`表示任意文本，`image/png`表示`PNG`格式的图片；`Accept-Language`，表示客户端接收的语言，多种语言按照优先级排序，服务器依靠该字段给用户返回特定语言的网页版本。**注**：如果是`GET`请求，那么该`HTTP`请求只有`Header`，没有`Body`；如果是`POST`请求，那么该`HTTP`请求带有`Body`，以一个空行分隔。一个典型的带有`Body`的`HTTP`请求如下：

```http
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

username=hello&password=123456
```

> `POST`请求通常要设置`Content-Type`表示`Body`类型，`Content-Length`表示`Body`长度，这样服务器就可以根据请求的`Header`和`Body`做出正确的响应。此外，`GET`请求的参数必须附加在`URL`上，并以`URLEncode`方式编码。
>
> ※ `HTTP`响应同样是由`Header`和`Body`两部分构成。第一行总是“`HTTP`版本 响应代码 响应说明”。客户端只依赖响应代码判断`HTTP`响应是否成功：`1**`，表示一个提示性响应，如`101`表示将切换协议，常见于`WebSocket`连接；`2**`，表示一个成功响应，如`200`表示成功，`206`表示只发送了部分内容；`3**`，表示一个重定向响应，如`301`表示永久重定向，`303`表示客户端应该按照指定路径重新发送请求；`4**`，表示一个因为客户端问题导致的错误响应，如`400`表示因为`Content-Type`等各种原因导致的无效请求，`404`表示指定的路径不存在；`5**`，表示一个因为服务器问题导致的错误响应，如`500`表示服务器内部故障，`503`表示服务器暂时无法响应。