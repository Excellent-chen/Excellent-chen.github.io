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

-----

