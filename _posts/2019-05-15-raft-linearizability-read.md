---
layout: post
title: Raft 实现线性一致性读的思路
author: zhangyue
---

Raft 协议内部基于原子广播Log复制来实现共识，因此基于Raft的写本身就满足线性一致性。但是Raft的读取并不满足线性一致性，只满足顺序一致性，缺失了实时性的保证，也就是不一定读的到最新的值。
原因是读取请求理论上可以落到任何一个副本（未经过大多数决议，任何节点也无法保证自己当前时刻是Leader），而这个副本无法保证是最新的。

为了能读到最新的值，主要的思路有以下两种：

* Raft Log Read 
    * 读请求也作为一个Log Entry 进入Raft Log走一遍共识流程。本质是利用Log的全序性，来保证读请求之前的操作都已经在所有节点间达成了共识，那么就一定可以读得到。但是整个走一遍Raft Log复制流程，开销很大。
* Leader Read ： 
    * 由于Raft所有的写操作都走Leader，所以如果能够确定谁是Leader，直接在Leader上读肯定可以读到最新的值。
    * 如何确定Read时刻谁是Leader ？ 
        * ReadIndex Read :  读时先记录当前的Log CommitIndex为 ReadIndex，然后走一轮心跳（大多数决议）确定自己还是leader ，那么截至ReadIndex之前，自己一定是Leader。 但ReadIndex之后的读不保证，因为有可能Leader已经飘走。相比于走一遍Raft Log共识流程，心跳要快的多。
        * Lease Read： 当走一轮心跳以后，根据租约过期时间，可以算出一个短暂的窗口期，这期间Leader是不可能飘走的，那么这个期间的Read就不用再走心跳了，节省了一部分心跳请求。但各节点间的CPU时钟差问题也必须考虑，因此窗口期的设定必须保守。用公式表达就是 Safty Window = Heart Beat Start Time + Election Timeout(Lease) - Clock Drift Bound


