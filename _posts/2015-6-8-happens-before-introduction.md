---
layout: post
title: 读书笔记：happens-before原则
category: back
description: Java happens-before原则。
tags: [java memory model]
---

如果Java内存模型中，所有的有序性都仅仅靠volite和synchronized来完成，那么有一些操作将会变的非常烦琐，但是我们在编写Java并发代码的时候并没有感觉到这一点，这是因为Java语言中有一个“happens-before”的原则。

这个原则非常重要，它是判断数据是否存在竞争、线程是否安全的主要依据，依靠这个原则，我们可以通过几条规则一揽子地解决并发环境下，两个操作之间是否可能存在冲突的所有问题。

## **定义**

Happens-before原则是Java内存模型中定义的两项操作之间的偏序关系，如果说操作A先行发生于操作B，其实就是说在发生操作B之前，操作A产生的影响能被操作B观察到，“影响”包括修改了内存中共享变量的值、发送了消息、调用了方法等。