---
title: Read-03-Hadoop权威指南
date: 2020-12-30 09:47:27
categories:
- Read
tags:
- Hadoop
---

-----

#### Chapter 002

> <!-- Section 005 -->
>
> ※ 不同版本的`Hadoop`之间存在哪些区别？
>
> ※  `Hadoop`主要由`HDFS`，`YARN`和`MapReduce`三部分组成。`HDFS`包括`NameNode`，`DataNode`和`SecondaryNameNode`；`YARN`包括`ResourceManager`，`NodeManager`，`ApplicationMaster`和`Container`；`MapReduce`包括`Map`和`Reduce`两个阶段，前者对输入数据进行并行处理，后者对前者的结果进行汇总。

> <!-- Scetion 006 -->
>
> ※ 大数据技术生态体系：数据来源层（结构化，半结构化，非结构化数据），数据传输层（`Sqoop`，`Flume`，`Kafka`），数据存储层（`HDFS`，`HBase`），资源管理层（`YARN`），数据计算层（`MapReduce`，`Spark`，`Storm`），任务调度层（`Azkaban`），业务模型层。

#### Chapter 003

> <!-- Section 001 -->
>
> ※ `etc`是`Etcetera`的缩写，是用于存放所有管理所需要的配置文件和子目录；`opt`是`optional`的缩写，是用于给主机额外安装软件所摆放的目录；`tmp`是`temporary`的缩写，是用于存放一些临时文件的。。

> <!-- Section 004 -->
>
> ※ `Hadoop`目录结构：`bin`，`etc`，`include`，`lib`，`libexec`，`sbin`和`share`。

#### Chapter 004

> ※ `Hadoop`运行模式包括：<span style="color:blue">本地模式</span>，<span style="color:blue">伪分布式模式</span>和<span style="color:blue">完全分布式模式</span>。
>

> <!-- Section 002 -->
>
> ※ 伪分布式模式下的配置信息完全是按照完全分布式模式搭建的，但是只有一台主机。
>
> ※ `Hadoop`配置文件分为两类：默认配置文件和自定义配置文件，只有用户想修改某一默认配置值时，才需要修改默认配置文件，更改相应属性值。在`core-site.xml`配置文件中，配置了`NameNode`的主机名端口号和存放临时数据的路径；在`hdfs-site.xml`配置文件中，配置了副本数；在`yarn-site.xml`配置文件中，配置了`ResourceManager`和`NodeManager`；在`mapred-site.xml`配置文件中，配置了程序的运行位置。

> <!-- Section 003 -->
>
> ※ `scp`安全拷贝，`rsync`远程同步，`xsync`集群分发。

#### Chapter 001

> <!-- Section 001 -->
>
> ※ `HDFS`是一个分布式文件系统，适用于<span style="color:red">一次写入，多次读出</span>的场景，且不支持文件修改。注：常见的文件系统还有`FAT`，`NTFS`。

> <!-- Section 002 -->
>
> ※ `HDFS`具有”高容错性，适合处理大数据，可构建在廉价机器上“的优点，”不适合低延时数据访问，无法高效对大量小文件进行存储，不支持并发写入、文件随机修改“的缺点。

> <!-- Section 004 -->
>
> ※ `HDFS`的块的大小设置主要取决于磁盘传输速率。若块设置过小，寻址开销将会增加；若块设置过大，作业的运行速度将会变慢。

#### Chapter 003

> <!-- Section 002 -->
>
> ※ 参数优先级排序：客户端代码设置的值；`ClassPath`下用户自定义的配置文件；服务器的默认配置。

#### Chapter 004

> <!-- Section 001 -->
>
> ※ <span style="color:blue">节点距离</span>：两个节点到达最近的共同祖先的距离之和。
>
> ※ <span style="color:blue">机架感知</span>：`For the common case, when the replication factor is three, HDFS’s placement policy is to put one replica on the local machine if the writer is on a datanode, otherwise on a random datanode in the same rack as that of the writer, another replica on a node in a different (remote) rack, and the last on a different node in the same remote rack.`

#### Chapter 005

> 

-----

