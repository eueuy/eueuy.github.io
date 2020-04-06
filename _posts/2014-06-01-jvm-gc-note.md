---
layout: post
title: 非阻塞同步 Lock-Free
author: zhangyue
---

## GC堆在JVM内存中的位置
Jvm 中内存被分为下面几个区域，GC作用于Java堆，所以也叫GC堆。GC堆是所有线程共享的一块区域。 
![](/assets/img/blog/2014-06-01-jvm-gc-note.png)
## Java内存分配策略
GC堆内部的区域划分
![](/assets/img/blog/2014-06-01-jvm-gc-note4.png)
> JDK 1.8 之后已经没有永久区 Perm

* 内存的分代机制
    * 对象生命周期有明显的差异，新产生的对象95%都会很快死亡，因此根据生命周期，分为新生代和老年代，便于分别安排合适的算法
    * 新生代死亡快对象少，适合用复制算法。 老年代生命长对象多，适合用标记-清除 或 标记-整理
    * 因此通常内存被分为以下几个区
        * young gen : minor GC  （占25%-50%)
            * eden  (占80%)
            * survivor x2 (各占10%)  
                * 分别标记为 survivor from 和 survivor to ，每次gc后互换标记  
        * old gen / tenured gen : Major GC/Full GC （占50%-75%)
* 对象的分配策略
    * 优先在Eden区
    * 大对象直接进老年代
    * 长期存活进老年代
    * 同年龄的对象占到survivor区的一半，直接进老年代
    * 空间分配担保机制
        * minor gc 前先检查老年代是否有足够的空间

## 判定垃圾对象
* 引用判断算法
    * 引用计数算法
        * 简单但JVM不用，因为它没法解决互相引用问题
		* 可达性分析法
			* 从GC Root 对象向下构建引用链，凡无法链接到链上的都被GC，以图论来说就是对GC Root对象不可达
		* 逃脱死亡的最后一次机会finalize()
			* 如果对象有finalize()方法，且方法中实现了可达性，则可以避免回收
* 对引用的分级
    * 目的是实现当内存不足时，为对象分级，比如用作缓存的对象虽然有引用存在，但是依然可以被GC。
    * 分级包括
        * strong ，普通引用
        * soft， 内存OOM前会干掉这部分
        * weak，多生存一轮GC，第二轮还是会干掉
        * phantom ，和没引用一样，很快会被干掉

## GC 算法
* 标记-清除算法 Mark-Sweep
    * 先标记无用对象 ，然后一次性清除它们
    * 缺点：会产生碎片
* 复制算法 Copying
    * 将内存分为两半，平时只用一半，满了就将对象复制到另一半上紧密排列。相当于两个杯子，每次GC时都将一个杯子里的东西倒进另一个。
    * JVM通常用这个算法来GC年轻代， 但是比例不是一半一半，一般是8:1
    * 缺点：空间有浪费，存活对象太多则复制慢
* 标记-整理算法 Mark-Compact
    * 结合标记清除和复制， 将存活的对象集中往一端挪动，将不要的对象覆盖或清除
    * JVM通常用这个算法来GC老年代，它不会像复制算法那么费时间和空间，也能连续排列

## GC 的触发条件
* MinorGC
    * Eden区空间不足
* FullGC
    * System.gc()
    * 老年代空间不足
    * MinorGC空间分配担保失败
    * CMS的Concurrent Mode Failure（老年代碎片太多没有足够的连续空间）

##垃圾收集器

Java的垃圾收集器
![](/assets/img/blog/2014-06-01-jvm-gc-note1.png)

Java收集器间的配合关系
![](/assets/img/blog/2014-06-01-jvm-gc-note3.png)

常见的组合
* 服务端重响应时间的应用:  ParNew + CMS
* 服务端重计算吞吐量的引用: Parallel Scavenge + Parallel Old
* 单CPU或Client: Serial + Serial Old

## GC优化思路
> 过早优化是万恶之源
> GC tuning is the last task to be done.

* 目标
    * GC执行非常迅速（minor 50ms以内, full 1s以内）
    * GC没有过于频繁执行（minor 10s一次, full 10min 一次）
* 监控和分析GC状态
    * APM系统
    * jstat -gcutil 21719 1s
    * jstat -gcutil -verbosegc 生成日志
    * heap dump
    * gc log
* 设置GC类型和内存大小
    * 更换垃圾收集器类型
    * -Xms -Xmx
        * 大空间=GC频率低，GC时间长
        * 小空间=GC频率高，GC时间短
    * -NewRatio
        * 2-4之间尝试
* 执行24小时结果分析
* 重复

附表：
![](/assets/img/blog/2014-06-01-jvm-gc-note2.png)

## References
* 《深入理解JAVA虚拟机》周志明