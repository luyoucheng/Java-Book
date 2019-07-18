
#Undo 

 currentHashMap   ,TreeMap , 基础知识


## 目录

- [Java](#java)
    - [基础](#list/基础/java基础.md)
    - [容器](#容器)
    - [并发](#并发)
    - [JVM](#jvm)
    - [I/O](#io)    【以下未实现】
    - [Java 8](#java-8)
    - [编程规范](#编程规范)
- [网络](#网络)
- [操作系统](#操作系统)
    - [Linux相关](#linux相关)
- [数据结构与算法](#数据结构与算法)
    - [数据结构](#数据结构)
    - [算法](#算法)
- [数据库](#数据库)
    - [MySQL](#mysql)
    - [Redis](#redis)
- [系统设计](#系统设计)
    - [设计模式(工厂模式、单例模式 ... )](#设计模式)
    - [常用框架(Spring、Zookeeper ... )](#常用框架)
    - [数据通信(消息队列、Dubbo ... )](#数据通信)
    - [网站架构](#网站架构)
- [面试指南](#面试指南)
    - [备战面试](#备战面试)
    - [常见面试题总结](#常见面试题总结)
    - [面经](#面经)
- [工具](#工具)
    - [Git](#git)
    - [Docker](#Docker)
- [资料](#资料)
    - [书单](#书单)
    - [Github榜单](#Github榜单)
- [待办](#待办)
- [说明](#说明)

## Java

### 基础 [待完成 ]

* [Java 基础知识回顾](docs/java/Java基础知识.md)[待完成 ]
* [J2EE 基础知识回顾](docs/java/J2EE基础知识.md)[待完成 ]

### 容器

* [常见面试题](docs/java/collection/Java集合框架常见面试题.md)[待完成 ]
* [集合相关知识学习](list/源码学习/集合源码学习.md)  
* [ArrayList 源码学习](list/源码学习/ArrayList.md)  
* [LinkedList 源码学习](list/源码学习/LinkedList.md)   
* [Vector 源码学习](list/源码学习/Vector.md)   
* [HashMap(JDK1.8)源码学习](list/源码学习/ArrayList.md)  [待完成 ]

### 并发 [待完成 ]

* [Java 并发基础常见面试题总结](docs/java/Multithread/JavaConcurrencyBasicsCommonInterviewQuestionsSummary.md)
* [Java 并发进阶常见面试题总结](docs/java/Multithread/JavaConcurrencyAdvancedCommonInterviewQuestions.md)
* [并发容器总结](docs/java/Multithread/并发容器总结.md)
* [乐观锁与悲观锁](docs/essential-content-for-interview/面试必备之乐观锁与悲观锁.md)
* [JUC 中的 Atomic 原子类总结](docs/java/Multithread/Atomic.md)
* [AQS 原理以及 AQS 同步组件总结](docs/java/Multithread/AQS.md)

### JVM

* [一 Java内存区域](list/JVM/JVM.md)
* [二 JVM垃圾回收](list/JVM/GC.md)
* [三 JDK 监控和故障处理工具](docs/java/jvm/JDK监控和故障处理工具总结.md) [待完成 ]
* [四 类文件结构](docs/java/jvm/类文件结构.md) [待完成 ]
* [五 类加载过程](docs/java/jvm/类加载过程.md) [待完成 ]
* [六 类加载器](docs/java/jvm/类加载器.md) [待完成 ]

### I/O [待完成 ]

* [BIO,NIO,AIO 总结 ](docs/java/BIO-NIO-AIO.md)
* [Java IO 与 NIO系列文章](docs/java/Java%20IO与NIO.md)

### Java 8  [待完成 ]

* [Java 8 新特性总结](docs/java/What's%20New%20in%20JDK8/Java8Tutorial.md)
* [Java 8 学习资源推荐](docs/java/What's%20New%20in%20JDK8/Java8教程推荐.md)

### 编程规范 [待完成 ]

- [Java 编程规范](docs/java/Java编程规范.md)

## 网络

* [计算机网络常见面试题](docs/network/计算机网络.md) [待完成 ]
* [计算机网络基础知识总结](list/计算机网络/计算机网络.md)
* [HTTPS中的TLS](docs/network/HTTPS中的TLS.md) [待完成 ]

## 操作系统

### Linux相关

* [后端程序员必备的 Linux 基础知识](docs/operating-system/后端程序员必备的Linux基础知识.md)  
* [Shell 编程入门](docs/operating-system/Shell.md)  

## 数据结构与算法

### 数据结构

- [数据结构知识学习与面试](docs/dataStructures-algorithms/数据结构.md)

### 算法

- [算法学习资源推荐](docs/dataStructures-algorithms/算法学习资源推荐.md)  
- [几道常见的子符串算法题总结 ](docs/dataStructures-algorithms/几道常见的子符串算法题.md)
- [几道常见的链表算法题总结 ](docs/dataStructures-algorithms/几道常见的链表算法题.md)   
- [剑指offer部分编程题](docs/dataStructures-algorithms/剑指offer部分编程题.md)
- [公司真题](docs/dataStructures-algorithms/公司真题.md)
- [回溯算法经典案例之N皇后问题](docs/dataStructures-algorithms/Backtracking-NQueens.md)

## 数据库

### MySQL

* [MySQL 学习与面试](docs/database/MySQL.md)
* [一千行MySQL学习笔记](docs/database/一千行MySQL命令.md)
* [MySQL高性能优化规范建议](docs/database/MySQL高性能优化规范建议.md)
* [搞定数据库索引就是这么简单](docs/database/MySQL%20Index.md)
* [事务隔离级别(图文详解)](docs/database/事务隔离级别(图文详解).md)
* [一条SQL语句在MySQL中如何执行的](docs/database/一条sql语句在mysql中如何执行的.md)

### Redis

* [Redis 总结](docs/database/Redis/Redis.md)
* [Redlock分布式锁](docs/database/Redis/Redlock分布式锁.md)
* [如何做可靠的分布式锁，Redlock真的可行么](docs/database/Redis/如何做可靠的分布式锁，Redlock真的可行么.md)

## 系统设计

### 设计模式

- [设计模式系列文章](docs/system-design/设计模式.md)

### 常用框架

#### Spring

- [Spring 学习与面试](docs/system-design/framework/spring/Spring.md)
- [Spring 常见问题总结](docs/system-design/framework/spring/SpringInterviewQuestions.md)
- [Spring中bean的作用域与生命周期](docs/system-design/framework/spring/SpringBean.md)
- [SpringMVC 工作原理详解](docs/system-design/framework/spring/SpringMVC-Principle.md)
- [Spring中都用到了那些设计模式?](docs/system-design/framework/spring/Spring-Design-Patterns.md)