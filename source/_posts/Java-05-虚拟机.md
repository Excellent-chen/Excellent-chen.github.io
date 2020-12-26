---
title: Java-05-虚拟机
date: 2020-12-24 16:32:55
categories:
- Java
tags:
- 虚拟机
---

<img src="Java-05-虚拟机/java-jvm-overview.png" style="zoom:80%;" />

#### Day 001

> <!-- Part 005 -->
>
> ※ 虚拟机可分为<span style="color:blue">系统虚拟机</span>（如`VMware`）和<span style="color:blue">程序虚拟机</span>（如`JVM`）。<span title="它们之间存在哪些区别？">**TODO**</span>

<!-- more -->

> <!-- Part 008 -->
>
> ※ 指令集架构可分为<span style="color:blue">基于栈的指令集架构</span>和<span style="color:blue">基于寄存器的指令集架构</span>。前者具有可移植性，指令集较小，指令较多，但性能较差；后者不具有可移植性，指令集较多，指令较少，但性能较高。

> <!-- Part 009 -->
>
> ※ `JVM`的生命周期包括<span style="color:blue">启动</span>，<span style="color:blue">执行</span>和<span style="color:blue">退出</span>。<span title="它们分别做了哪些事情，哪些情况能够导致JVM退出呢？">**TODO**</span>

> <!-- Part 010 -->
>
> ※ 常见的`JVM`包括`HotSpot`，`JRockit`和`J9`。<span title="历史上都出现过哪些JVM，哪些JVM至今仍在使用，它们之间存在哪些区别？">**TODO**</span>

#### Day 002

> <!-- Part 002 -->
>
> ※ 类加载包括<span style="color:blue">加载</span>，<span style="color:blue">连接</span>（验证、准备、解析）和<span style="color:blue">初始化</span>三个阶段。其中，加载，验证，准备和初始化发生的顺序是确定的，而解析在某些情况下可以在初始化完成后进行，这是为了支持`Java`语言的<span style="color:red">运行时绑定</span>。此外，上述阶段通常都是交叉运行的，在一个阶段执行的过程中调用或激活另一个阶段。
>
> ※ 在加载阶段，虚拟机需要完成以下三件事情：通过一个类的全限定名来获取其定义的二进制字节流；将该字节流所代表的静态存储结构转化为方法区的运行时数据结构；在`Java`堆中生成一个代表该类的`java.lang.Class`对象，作为对方法区中这些数据的访问入口。<span title="加载.class文件的方式有哪些？">**TODO**</span>
>
> ※ 验证作为连接阶段的第一步，用于<span style="border-bottom: 1px solid green">确保被加载的类的正确性</span>，主要包括文件格式验证，元数据验证，字节码验证和符号引用验证。准备作为连接阶段的第二步，用于<span style="border-bottom: 1px solid green">为类的静态变量（简称“类变量”）分配内存，并将其初始化为默认值</span>（在该步中存在一些需要注意的地方，详见<a href="https://www.pdai.tech/md/java/jvm/java-jvm-classload.html#%e5%88%9d%e5%a7%8b%e5%8c%96">这里</a>）。解析作为连接阶段的第三步，用于<span style="border-bottom: 1px solid green">将类中的符号引用转换为直接引用</span>，主要针对类/接口，字段，类方法，接口方法，方法类型，方法句柄和调用点限定符`7`类符号引用进行。
>
> ※ 在初始化阶段，虚拟机需要对类变量进行初始化，初始化的方式有两种：声明类变量是指定初始值；使用静态代码块为类变量指定初始值。 

