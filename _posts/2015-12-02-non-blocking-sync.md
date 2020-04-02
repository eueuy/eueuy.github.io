---
layout: post
title: 非阻塞同步
author: zhangyue
---

## 什么是阻塞
所谓的阻塞，是指有线程被挂起，从而引起上下文切换、长时间中断等一系列性能开销 。重量级锁大多都会导致线程被挂起。阻塞的目的是让线程等待，另一种不需要挂起的等待方法是"忙等待" ，也就是自旋。 但是自旋也有它的问题，它不能等太久，因为会不断消耗CPU。非阻塞算法的目的就是：在没有任何线程被挂起的情况下，完成同步。非阻塞算法的思路一个非阻塞算法的目标是要保证以下几件事情：
* 安全性保证（一致性）
    * 保证可见性
    * 保证原子性
* 活性保证
    * 没有死锁、活锁和饥饿问题
* 性能保证
    * 没有线程被挂起

用 Volatile 保证可见性， 用轻量自旋保证活性 ，都是比较轻量的方法。但是原子性始终是个瓶颈。

幸好有CAS，CAS可以认为是强化版的Volatile，它既能保证可见性又能保证原子性。再配合自旋，完美的满足了要求。这就是为什么CAS是无阻塞算法的基础。但CAS只能作用于一个变量上，因此

> 设计基于非阻塞同步的数据结构时，关键是找出如何将原子修改的范围缩小到单个变量上，同时维护数据一致性。

如果一个数据结构无法将原子修改的范围缩小到单个变量上呢？ 这个时候问题就变的更复杂，每种数据结构的特点不同，需要采用的算法也不同。 这里给出一个示例参考，看看ConcurrentLinkedQueue的解法。

ConcurrentLinkedQueue的非阻塞算法由于LinkedQueue的插入操作需要涉及两个步骤和两个变量：
* 首先是在尾部插入一个新节点，涉及原尾部节点next指针的改变。
* 然后是将tail尾指针后移一位，涉及tail指针的改变。

由于无法缩小到单个变量上，为了实现非阻塞算法，需要巧妙的设计。简单来说，任何线程在插入时都会先判断lastNode.next是否为空，如果是空说明链表是稳定的，则执行自己的插入。如果不是，则先将tail指向 lastNode.next ，让链表变的稳定（这一步也叫“清理操作”），再干自己的事。而这个操作之所以不会破坏安全性，完全依靠CAS来保证。

而链表不稳定也有两种可能：
* 其它线程正插入了一半，即只插入了新节点，还没来得及更新tail指针。以下2点则保证了插手别人的事务也不会破坏数据：
    * 如果其它线程先于自己完成了尾指针的后移，CAS可保证什么都不改变
    * 如果其它线程慢于自己，则其它线程也可以通过CAS保证不会重复做后移
* 其它线程执行到一半挂掉了

## 非阻塞的性能
非阻塞的思想是“乐观”思想，因此在竞争不大的情况下，性能提升显著。 但如果竞争非常大，性能可能会落后于阻塞算法。因为阻塞算法会挂起线程，减少竞争的烈度。
## CAS的 ABA问题
ABA问题的本质是完全的并发关系导致，只需要通过一些手段将其转化为因果关系从而可以判断 。 添加版本号、向量等办法都是可行的。
##总结
得益于CAS和java提供的 atomic工具，在更新可以缩小到单个变量的数据结构上， 进行非阻塞改造是相对容易的，而且吞吐率提升喜人。 但是更复杂的结构需要很专业的能力，否则不是不正确就是性能差，实际运用中不建议造这方面的轮子。
## references
Java 理论与实践- 非阻塞算法简介
“Simple, Fast, and Practical Non-Blocking and Blocking Concurrent Queues”