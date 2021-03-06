---
layout: post
title: JMM与Happens-Before
author: zhangyue
---
JMM指Java内存模型，它决定了JVM内部对内存的使用，而对并发程序来讲，它决定了一个重要的特性：内存可见性。 

## JMM与串行一致性
程序的串行一致性，是指程序按照唯一的顺序严格执行，并且对任何寄存器的读写都是实时可见的。开发人员经常错误的假设程序默认满足串行一致性，但是任何一款现代多处理器架构都不提供串行一致性保证，JMM也是如此。这是很多隐患的来源。

JMM只能保证为程序提供一个偏序关系，称为happens-before。要想保证A线程能够看到B操作的结果，也就是B对A满足可见性，则A和B必须满足happens-before关系，否则JVM可能任意重排序，这将会导致A看到的B的操作和B的真实操作顺序不一致，自然内存结果也看起来不一样。

另一个保证可见性的办法，是进行同步（加锁），经过正确同步的程序会表现出全局顺序，有完整的可见性。

## Happens-Before
我们可以利用这些happens-before规则，为2个偏序的线程附加因果关系，让他们拥有逻辑上的顺序和可见性，而不必进行高昂的同步处理。

首先，JMM能保证一个线程中的操作顺序是按照代码顺序执行的（根据程序顺序规则）。也就是说线程内部是全序的，线程和线程间是偏序的。之后我们可以利用如下规则，让两个线程间产生因果顺序：

* 监视器锁规则：同一个锁的解锁肯定在加锁之前
* volatile变量规则：同一个volatile变量的写入肯定在读取之前
* 线程启动规则：Thread.start 肯定在该线程的代码之前执行
* 中断规则：A调用B的Interrapt 的代码肯定在 B的Interrapt 代码之前执行
* 终结器规则：对象的构造函数肯定在终结器调用之前执行
* 传递性规则：A在B之前， B在C之前，则A在C之前

例如如果想让线程A的操作结果对线程B可见， 也就是让A的代码一定在B之前执行 ，则可以让A在结束前写入一个volatile变量， 然后B在开始执行时首先读取同一个volatile变量。通过volatile变量，为两个完全不相关的并行程序附加了因果关系。这是一种借助happens-before规则实现的轻同步技术。

让我们看一个实际例子。

## 共享变量的发布问题

要想将一个由线程A初始化的变量X传递到线程B，并不是那么简单。 除了不可变对象，一个线程使用被另一个线程初始化过的对象，都是不安全的。

我们看一个面试中小有名气的反例“不安全的单例”

```java
public class SingletonRes {
    private static Resource instance;
    public static Resource getResourceInstance() {
        if (instance == null) {
            instance = new Resource();
        }
        return instance;
    }
}
```

在这个例子中，人们通常都比较关注原子性问题， 也就是会产生竞争导致创建两个实例。于是诞生了一种有漏洞的优化手段：DCL双重检查锁。但可见性问题却被忽略了！A线程创建了单例，B线程获得了A创建的Resource实例，但是由于可见性原因，这个Resource实例在B线程这里很可能是个只初始化了一半的半成品。

由此可见，一个安全的单例要同时保证原子性和可见性才行。下面列举一些安全发布共享变量的方法：

* 进行同步，同步程序是串行一致的，但是性能是个问题
* 使用CAS，CAS可以同时保证可见性和原子性
* 使用PlaceHolder 占位模式 
* DCL + Volatile ： 不必要，因为JVM引入了锁消除技术，在无竞争的情况下synchronize同步已经没有多余开销
* 利用类的静态域进行初始化： 由于JVM在类加载时会获得一个锁JLS，每个线程都会拥有一次这个锁来判断类是否已加载，因此是可见的。
* 利用final域进行初始化： 构造函数是不会被重排序的，因此是安全的，但是构造函数之外的改变将不保证。




