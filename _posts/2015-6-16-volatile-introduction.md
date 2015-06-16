---
layout: post
title: 读书笔记：Java关键字volatile
category: back
description:
tags: [java volatile]
---

关键字volatile是Java虚拟机提供的最轻量级的同步机制，但是它并不容易被完全正确、完整的理解，以至于许多程序员在处理并发问题时，乱用、错用。于是，花一些时间弄清楚volatile的语义就显得至关重要。

Java内存模型对volatile专门定义了一些特殊的访问规则，主要包括两个特性：可见性和禁止指令重排序优化。

## **可见性**

可见性是指，**当一个变量被定义为volatile后，如果有一条线程对这个变量进行了修改，新值对于其他线程是立即可见的。**

关于volatile变量的可见性，经常会被开发人员误解。看看下面这段论述：

***"volatile变量对于所有的线程是立即可见的，对volatile变量所有的写操作都能立刻反应到其他线程之中，换句话说，volatile变量在各个线程中时一致的，所以基于volatile变量的运算在并发下是安全的"***

