---
layout: post
title: 自动动手实现Raft ：实现Raft的过程记录
author: zhangyue
---
## 实现一个raft 算法的思路
	
* 实现RPC，因为Raft需要节点间的自由通信
* 实现正确的日志状态机复制
    * 参考资料
        * https://marcoma.xyz/2018/12/17/raft-log-replication/
        * https://juejin.im/post/5af066f1f265da0b715634b9
        * https://thesquareplanet.com/blog/students-guide-to-raft/
    * Follower 接收日志时的一致性检查
        * Term必须一样或更加新
        * 先前的日志必须对齐，即PrvLogIndex 和 PrvLogTerm 必须一致
    * 核心数据结构
        * AppendEnties（随心跳携带的数据）
            * PrvLogIndex 前一条日志的index，用于保证follower前面的日志和leader能对齐，才能保证正确复制。考虑下面几个场景
                * 回滚： 比如leader尝试同步日志18，但未收到大多数成功票，所以回滚了日志18 。但投成功的follower C没法知道leader已经回滚了日志18，它的log中还有日志18，这时通过PrvLogIndex=17，follower C就知道18被放弃了（通常follower会直接删除PrvLogIndex 之后的日志）
                * 落后： 比如follower C 卡死了，恢复后，收到的请求显示 PrvLogIndex = 30 ，但是自己的LastIndex 只有19 ，则通知Leader不能复制，要求从19开始提供日志，追平后才能复制。
            * LastCommitIndex， 用于通知follower 哪些日志可以被应用到状态机了。
            * PrvLogTerm， 还用不到，因为没实现选举
            * LeaderId \ TermId ，还用不到，因为没实现选举
        * ServerState （每个Node自身的状态）
            * CurrentTerm 保存node当前的Term用于验证
            * LogEntries []保存自身日志序列
            * LastComittedIndex 保存可以apply到状态机的索引位置
            * LastAppliedIndex 保存已经apply到状态机的索引位置
            * Leader 还需要
                * NextIndex[]  用于知道下次要从哪里开始给每个Follower发送日志
                * MatchIndex[]  用于知道每个 Follower 从哪里往前是和自己对齐的
                    * 如果新Leader刚上台，它需要知道follower对齐到哪里，用于计算NextIndex
* 实现leader 选举
    * 问题： 前面我们实现了复制，但是主是手工指定的，一旦主崩溃，集群无法自动指定新的主。
    * https://marcoma.xyz/2018/12/09/raft-leader-election/
* 实现leader crash 正确性保证

to be continues...

## References
* Students' Guide to Raft -- Jon Gjengset