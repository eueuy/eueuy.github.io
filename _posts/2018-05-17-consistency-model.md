---
layout: post
title: “分布式一致性模型” 导论
author: zhangyue
---

## 这一切是怎么变复杂的
模型A ： 想象一个最简单的存储系统，只有一个客户端（单进程）和一个服务端（单副本）。客户端顺序发起读写操作，服务端也顺序处理每个请求，后一个操作都可以看到前一个操作的结果。这时系统没有一致性问题。

模型B ： 系统还是单个服务进程（单副本），但是有多个客户端并发进行操作。这时多个客户端的操作会互相影响，比如会读到不是自己写的数据。这时系统存在由并发竞争导致的一致性问题。

模型C ： 现在让数据拥有两个副本。假设还是一个客户端进程顺序发起读写操作，每个操作可以发给两个副本所在的任意一个。如果副本之间数据同步不及时，就会出现因多副本同步滞后而导致的不一致问题。

模型B+C ：真实的系统通常既不会只允许单客户端访问，也不会是单副本，而是前两种模型的结合。即多个同时执行读写操作的客户端同时读写多个副本的同一个数据。这时就会同时引入两类一致性风险。

模型D ： 更真实一些，如果系统是分区的。一个操作更改了位于不同分区的两个不同副本，那么读操作只能看到一个分区的结果还是两个都能看到？ 这时系统不仅有B+C的不一致问题，还额外增加了由于分区导致的不一致问题。

## 一致性模型索引
如此多的不一致问题真是让人头大，而且现实系统通常会放弃一部分一致性保证来换取性能，那么意味着存在的一致性可能性就更多了。

好在已经有人帮我们将这些一致性问题的脉络整理好了，Jepsen 在官网上面有一张Consistency Models Map，梳理了所有常见的一致性模型和它们的关系，可以说相见恨晚。

![](/assets/img/blog/2018-05-17-consistency-model1.png)

这里我简单介绍一下这张Consistency Models Map 。 图颜色代表了模型的可用性，箭头代表包含关系，例如Linearizable 包含了 Squential 的所有保证。

红色圆圈里面的模型属于 Unavailable，橙色的属于 Sticky Available，蓝色的就是 Highly Available。这里解释下相关的含义：

* Unavailable: 当出现网络隔离等问题的时候，为了保证数据的一致性，不提供服务。熟悉 CAP 理论的同学应该清楚，这就是典型的 CP 系统了。
* Sticky Available: 即使一些节点出现问题，在一些还没出现故障的节点，仍然保证可用，但需要保证 client 不能更换节点。
* Highly Available: 就是网络全挂掉，在没有出现问题的节点上面，仍然可用。

## 一致性(Consistency) 与 隔离性(Isolation)  

观察一致性模型的关系图可以发现，一致性模型主要有两个分支。

* Serializable 分支代表数据里的隔离性(Isolation)，是一种并发安全保证。这些模型聚焦于解决由于并发冲突而导致的数据可见性和原子性问题。例如Serializable保证并发事务表现的像没有并发一样依次执行。这个分支下的模型假设数据只有一个副本。 
* Linearizable 分支代表副本一致性，也是CAP中的C (Consisitency)，是一种数据实时性保证。这些模型聚焦于解决数据多个副本间因复制滞后所产生的实时性和有序性问题。例如最强的Linearizable保证数据像只有一个副本一样实时。这个分支下的模型假设数据的读写事务是原子性的，即没有并发问题。


因此最强一致性模型严格可串行模型Strict Serializable 就是保证数据读写像依次在一个副本上执行一样，换句话说：像单线程单机程序那样安全。 一些非分布式且有事务机制的数据库可以实现这个模型，比如MySQL。

隔离性问题和副本一致性问题共同组成了系统的数据安全保证。但是，他们完全是两类问题，在系统设计时应该单独考虑，以便降低复杂性。

接下来我将简单介绍这两个分支下的一致性模型。

## 隔离性相关的模型(Serializable)

在理解隔离级别模型前，需要先了解并发事务在没有隔离性保证时会导致哪些问题：

* Dirty Write - 一个事务覆盖了另一个事务还没提交的中间状态
* Dirty Read - 一个事务读到了另一个事务还没提交的中间值
* Non-Repeatable Read - 一个事务中连续读取同一个值时返回结果不一样（中途被其他事务更改了）
* Phantom - 当一个事务按照条件C检索出一批数据，还未结束。另外的事务写入了一个新的满足条件C的数据，使得前一个事务检索的数据集不准确。
* Lost Update - 先完成提交的事务的改动被后完成提交的事务覆盖了，导致前一个事务的更新丢失了。
* Cursor Lost Update - Lost Update的变种，和Cursor操作相关。
* Read Skew - 事务在执行a+b期间，a，b值被其他事务更改了，导致a读到旧值，b读到新值
* Write Skew - 事务连续执行if(a) then write(b) 时， a值被其他事务改了，类似超卖。


接下来看一下这些隔离级别提供的保证：

* Read Uncommitted - 能读到另外事务未提交的修改。
* Read Committed - 能读到另外事务已经提交的修改。
* Cursor Stability - 使用 cursor 在事务里面引用特定的数据，当一个事务用 cursor 来读取某个数据的时候，这个数据不可能被其他事务更改，除非 cursor 被释放，或者事务提交。
* Monotonic Atomic View - 这个级别是 read committed 的增强，提供了一个原子性的约束，当一个在 T1 里面的 write 被另外事务 T2 观察到的时候，T1 里面所有的修改都会被 T2 给观察到。
* Repeatable Read - 可重复读，也就是对于某一个数据，即使另外的事务有修改，也会读取到一样的值。
* Snapshot - 每个事务都会在各自独立，一致的 snapshot 上面对数据库进行操作。所有修改只有在提交的时候才会对外可见。如果 T1 修改了某个数据，在提交之前另外的事务 T2 修改并提交了，那么 T1 会回滚。
* Serializable - 事务按照一定顺序执行。

最后，将问题和方案对应起来，看看不同隔离级别下，能解决的问题
![](/assets/img/blog/2018-05-17-consistency-model2.png)

## 副本一致性相关的模型(Linearizablity)：两种视角
![](/assets/img/blog/2018-05-17-consistency-model3.png)

涉及副本一致性模型时，模型会被分为两种：以系统提供者的角度（以数据为中心）和以系统使用者的角度（以客户为中心）。

提供者视角聚焦于系统内部同步过程和副本之间的通信，因此被称为以数据为中心的一致性。而使用者视角聚焦于系统的外部表现，即从使用者的角度来衡量系统提供的保证，不关心内部细节。

另外，提供者视角聚焦于系统内部，需要面对多个进程同时对数据进行读写下的一致性问题。 而使用者视角只关心本进程即可，假设不会同时发生更新操作，或者同时发生更新操作时，能够比较容易的化解。

很明显，系统提供者的一致性模型会从根本上影响使用者的一致性模型。因此提供者视角的模型通常比使用者的模型更强。

通常大家将 Linearizablity 和 Sequentail 归为强一致性，因为他们保证了顺序性（虽然Sequentail牺牲了实时性）。 而将以客户为中心的一致性模型 (Client-centric consistency models)和因果一致性 (Causal consistency) 归为弱一致性。


## 副本一致性的相关模型：以数据为中心部分(Data-Centric)
以数据为中心的模型期望提供这样一种保证：多个进程的读写操作的结果可以立即复制到其它副本。
![](/assets/img/blog/2018-05-17-consistency-model4.png)

这些模型包括：
* 线性一致性（Linearizability）- 最强的单对象一致性模型，要求任何写操作都能立刻同步到其他所有进程，任何读操作都能读取到最新的修改。简单说就是实现进程间的 Java volatile 语义。这是一种对顺序和实时性都要求很高的模型。要实现这一点，要求将所有读写操作进行全局排序。
* 顺序一致性（Sequential Consistency）：在线性一致性上放松了对实时性的要求。所有的进程以相同的顺序看到所有的修改，读操作未必能及时得到此前其他进程对同一数据的写更新。
* 因果一致性（Causal Consistency）：进一步放松了顺序要求，提高了可用性。读操作有可能看到和写入不同的顺序 。只对附加了因果关系的写入操作的顺序保证顺序性。“逻辑时钟”常用来为操作附加这种因果关系。
* PRAM一致性(Pipeline Random Access Memory) ：多进程间顺序的完全放松，只为单个进程内的写操作保证顺序，但不保证不同的写进程之间的顺序。这种放松可以大幅提高并发处理能力。例如Kafak保证在一个分区内读操作可以看到写操作的顺序 ，但是不同分区间没有任何顺序保证。

## 副本一致性的相关模型：以客户为中心部分(Client-Centric)
很多时候并不要求系统内所有的数据都保持一致，以客户为中心的一致性放弃了整体的一致性要求，只为单一用户的读写保证一致性。这些模型不处理并发更新，只保证了单个客户端进程从不同位置访问不同副本时能观察到一致的数据。
![](/assets/img/blog/2018-05-17-consistency-model5.png)
这些模型包括：
* 单调读一致性（Monotonic-read Consistency），如果一个客户端读到了数据的某个版本n，那么之后它读到的版本必须大于等于n。
* 单调写一致性（Monotonic-write Consistency），保证同一个客户端的两个不同写操作，在所有副本上都以他们到达存储系统的相同的顺序执行。单调写可以避免写操作被丢失。
* 写后读一致性、读己之写一致性（Read-your-writes Consistency），如果一个客户端写了某个数据的版本n，那么它之后的读操作必须读到大于等于版本n的数据。
* 读后写一致性（Writes-follow-reads Consistency）保证一个客户端读到版本n数据后（可能是其他客户端写入的），随后的写操作必须要在版本号大于等于n的副本上执行。

另外，经常看到有人将 弱一致性(weak consistency) 归为此类，实际上弱一致性并不是一个一致性模型，它只是一个概念，因此不能相提并论。

PRAM一致性(Pipeline Random Access Memory)完全等同于读你的写、单调写和单调读。如果要追求读后写一致性，只能选择因果一致性。如果你需要完全的可用性，可以考虑牺牲阅读你的写，选择单调读 + 单调写。


## Reference 

* Consistency Models
* Strong Consistency Models
* Explain the difference between data centric and client centric consistency models. 
* Client-centric Consistency Models
* A Critique of ANSI SQL Isolation Levels (MSR-TR-95-51.PDF)