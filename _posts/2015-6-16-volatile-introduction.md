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

***"volatile变量对于所有的线程是立即可见的，对volatile变量所有的写操作都能立刻反应到其他线程之中，换句话说，volatile变量在各个线程中时一致的，所以基于volatile变量的运算在并发下是安全的。"***

这句话的论据并没有错，但是并不能得出“基于volatle变量的运算在并发下是安全的”这个结论。因为Java里面的运算并非是原子的，导致volatile变量的运算在并发下一样是不安全的，可以通过一段简单的示例来说明，代码清单如下：

<pre class="brush: java">
</pre>

这段代码发起了20个线程，每个线程对race变量进行了10000次自增操作，如果这段代码能够正确并发的话，最后输出的结果应该是200000，然而真实的结果并非如此，而且每次运行的结果也都不一样，这是为什么呢？

问题出在自增运算“race＋＋”中，通过将这段代码Javap反编译后，发现increase()方法在Class文件中是由4条字节码指令构成的，并不是原子的操作。

<pre class="brush: java">
</pre>

由于volatile只能保证可见性，在不符合以下两条规则的运算场景中，我们仍然要通过加锁来保证原子性。

* 运算结果并不依赖变量的当前值，或者能够确保只有单一的线程修改变量的值；
* 变量不需要与其他状态变量共同参与不变约束。

下面的代码展示了volatile的使用场景，用作线程见的信号传递，当shutdown()方法调用时，能保证所有线程中执行的doWork()方法都立即停下来。

<pre class="brush: java">
volatile boolean shutdownRequested;

public void shutDown(){
    shutdownRequested = true;
}

public void doWork(){
    while(!shutdownRequested){
    // do stuff
    }
}
</pre>

## **禁止指令重排序**

使用volatile变量的第二个语义是禁止指令重排序优化。普通的变量仅仅会保证在该方法的执行过程中，所有依赖赋值结果的地方都能获取到正确的结果，而不能保证变量的赋值操作的顺序与程序代码的执行顺序一致。因为在一个线程的方法执行过程中无法感知到这点，这也就是Java内存模型中描述的所谓“线程内串行语义”。

通过一个例子来看看为什么指令重排序会干扰程序的并发执行，代码清单如下：

<pre class="brush: java">
Map configOptions;
char[] configText;
// 此变量必须定义为volatile
volatile boolean initialized = false;

// 假设以下代码在线程A中执行
// 模拟读取配置信息，当读取完成后将initliazed设置为true
configOptions = new HashMap();
configText = readConfigFile(fileName);
processConfigOptions(configText, configOptions);
initialized = true;

// 假设以下代码在线程B中执行
// 等待initialized为true，代表线程A已经把配置信息初始化完成
while(!initialized){
    sleep();
}

// 使用线程A中初始化好的配置信息
doSomethingWithConfig();
</pre>

上述代码的描绘的场景十分常见。