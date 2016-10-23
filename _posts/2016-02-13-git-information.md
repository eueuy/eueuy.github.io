---
layout: post
title: git备忘录
author: Liucheng Xu
tags: git
disqus: y
---
* TOC
{:toc}

## git 常用命令速查表

![git常用命令速查表](/assets/img/blog/2016/02-14/Git常用命令.jpg)

[Pro git book](https://git-scm.com/book/zh/v2) 里面的内容应该是足够日常使用了，也是查阅的好地方。本文内容在其中应该都能够找到。

## git 常见场景下的用法

### 撤销修改

- 放弃刚刚对一个文件的改动
刚刚修改了一个文件，但是现在发现还是修改之前的好，打算放弃刚才对这个文件的修改。
在这种情况下，其实只要执行一下 <code>git status</code>就会有所提示，比方我们刚刚修改了一个叫做index.html的文件，在terminal中执行<code>git status</code>：

   ![git status](/assets/img/blog/2016/02-14/git-status.png)

根据提示，使用<code>git add index.html</code>则是将此次修改提交到了暂存区，使用<code>git checkout -- index.html</code>则是放弃这次的修改，将文件还原到此次修改前的状态。

- 提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。
此时，可以运行带有 <code>--amend</code> 选项的提交命令尝试重新提交.这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。比如，在<code>git commit -m "modified inndex.html"</code>后， 你发现index写错了或者还有文件没有提交，如果还有文件additional-files需要提交，那么输入<code>git add additional-files</code>.如果没有任何文件需要修改提交，只是修改提交信息，那么直接输入<code>git commit --amend</code>，文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。
最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

### 版本回退
版本回退大致分为2种情况:

1：一切操作都还停留在本地仓库，<code>add</code> --> <code>commit</code>后并未 <code>push</code> 推送到远端；
2：已经执行 <code>push</code> 操作将修改推送到远端。

#### 尚未<code>push</code>
 这种情况发生在你的本地代码仓库，可能在执行 add --> commit 以后发现代码有点问题，准备取消此次commit， 这时用到 <code>reset</code> 命令
<code>git reset -- soft | -- mixed | -- hard</code>

- <code> -- mixed</code>
<code>git reset</code> 默认选项， 会保留源码， 只是将 git commit 和 index 信息回退到了某个版本. <code>git reset --mixed</code>  等价于  <code>git reset</code>

- <code> -- soft</code>
保留源码，只回退到 commit 信息到某个版本.不涉及 index 的回退，如果还需要提交，直接commit即可.
- <code> -- hard</code>
源码也会回退到某个版本，commit 和 index 都回退到某个版本.(注意，这种方式是改变本地代码仓库源码)

比方说我们想回到上一个版本。

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交版本，上一个版本就是<code>HEAD^</code>，上上一个版本就是<code>HEAD^^</code>，当然往上100个版本写100个<code>^</code>也不太容易，所以写成<code>HEAD~100</code>。

```bash
git reset --hard HEAD^  //  回到上一个版本
```

当然有人在 push 代码以后，也使用 reset --hard <commit...> 回退代码到某个版本之前，但是这样会有一个问题，你线上的代码没有变，线上commit，index都没有变，当你把本地代码修改完提交的时候你会发现全是冲突.....

 所以，这种情况你要使用下面的方式:

#### 已经<code>push</code>

对于已经把代码push到线上仓库，你想回退本地代码的同时也回退线上代码，回滚到某个指定的版本，线上，线下代码保持一致.这时要用到下面的revert命令

执行 revert 命令时要求工作树必须是干净的，git revert 用一个新提交来消除一个历史提交所做的任何修改.

revert 之后你的本地代码会回滚到指定的历史版本，这时你再 git push 即可以把线上的代码更新.(这里不会像reset造成冲突的问题)

revert 使用，需要先找到你想回滚版本唯一的 commit 标识代码，可以用 git log 查看.
<code>git revert c011eb3c20ba6fb38cc94fe5a8dda366a3990c61</code>
通常，前几位即可 <code>git revert c011eb3</code>
git revert 是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit

看似达到的效果是一样的，其实完全不同.

1. 上面我们说的如果你已经 push 到线上代码库， reset 删除指定 commit 以后，你 git push 可能导致一大堆冲突.但是 revert 并不会.

2. 如果在日后现有分支和历史分支需要合并的时候，reset 恢复部分的代码依然会出现在历史分支里.但是revert 方向提交的commit 并不会出现在历史分支里.

3. reset 是在正常的commit历史中，删除了指定的commit，这时 HEAD 是向后移动了，而 revert 是在正常的commit历史中再commit一次，只不过是反向提交，他的 HEAD 是一直向前的.

#### 参考资料
- [git reset revert 回退回滚取消提交返回上一版本](http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html)
