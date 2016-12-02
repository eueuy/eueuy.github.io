---
layout: post
title: vim 入门
author: Liucheng Xu
tags: vim
disqus: y
---

* TOC
{:toc}

## vim 基础知识

### vim概览
- vim的命令有如下特点：

    - 字母大小写有区别(大写与小写表示不同的意义，I与i功用不同)。
    - 在输入时不会显示在屏幕上。
    - 不需要在命令后加上<code>enter</code>键。

- 运作模式
当前"模式(mode)"的概念对vim的运作是最基础的。模式有两种:命令模式(command mode)与插入模式(insert mode).一开始是命令模式，此时所有的按键都代表命令；而在插入模式中，你输入的东西都成为文件的内容。

### vim命令的一般形式
如果对于vim不是一个完全的新手，大概能够发现大部分vim命令具有以下模式：<code>(command)(text object)</code>.对于更改命令(change)c，command部分就是指c，text object(文本对象)则是光标移动命令(输入时不需要加上括号)。删除命令d(delete)、复制命令y(yank)同样适用这种形式。

另外，text object（光标移动命令）可使用数值参数，因此可将数值加在c、d、y等命令的文本对象上。例如d2w与2dw都是删除两个单词的命令。在了解这一点后，其实大部分vim命令都遵循如下模式： <code>(command)(number)(text object)</code>或者其等效模式: <code>(number)(command)(text object)</code>. 它们的工作方式是这样的：number与command为可选项。如果没有这两部分，只是单纯的光标移动命令；如果加上number，则出现移动多次的效果；结合command（c、d、y等等）与text object， 则会得到编辑命令。

当你认识到这些组合的多样性后，vim就成为有强大威力的编辑器了！

### 按词性划分

#### 动词

动词代表了我们打算对文本进行什么样的操作。例如：

- d 表示删除（delete）
- r 表示替换（replace）
- c 表示修改（change）
- y 表示复制（yank）
- v 表示选取（visual select）

#### 介词

名词代表了我们即将处理的文本。Vim 中有一个专门的术语叫做文本对象（text object），下面是一些文本对象的示例：

- w 表示一个单词（word）
- s 表示一个句子（sentence）
- p 表示一个段落（paragraph）
- t 表示一个 HTML 标签（tag）
- 引号或者各种括号所包含的文本称作一个文本块。

#### 介词

介词界定了待编辑文本的范围或者位置。例如：

- i 表示“在…之内”（inside）
- a 表示“环绕…”（around）
- t 表示“到…位置前”（to）
- f 表示“到…位置上”（forward）

下面是几个有关范围的示意图，你们感受一下：

![vim-position](/assets/img/blog/2016/02-28/position.png)

#### 组词为句

有了这些基本的语言元素，我们就可以着手构造一些简单的命令了。文本编辑命令的基本语法如下：`动词 （介词） 名词`， 其中介词并非必要.

下面是一些例子（如果熟悉了上面的概念，你将会看到这些例子非常容易理解），请亲自在 Vim 中试验一番。

```
# 删除一个段落: delete inside paragraph
  dip

# 选取一个句子: visual select inside sentence
  vis

# 修改一个单词: change inside word
  ciw

# 修改一个单词: change around word
  caw

# 删除文本直到字符“x”（不包括字符“x”）: delete to x
  dtx

# 删除文本直到字符“x”（包括字符“x”）: delete forward x
  dfx
```

#### 数词

数词指定了待编辑文本对象的数量，从这个角度而言，数词也可以看作是一种介词。引入数词之后，文本编辑命令的语法就升级成了下面这样：`动词 (介词/数词) 名词`
下面是几个例子：

```
# 修改三个单词：change three words
c3w

# 删除两个单词：delete two words
d2w
```

另外，数词也可以修饰动词，表示将操作执行 n 次。于是，我们又有了下面的语法： `数词 动词 名词`.
请看示例：

```
# 两次删除单词（等价于删除两个单词）: twice delete word
2dw

# 三次删除字符（等价于删除三个字符）：three times delete character
3x
```

### 按操作划分

#### 保存退出
下列操作都是在命令行模式下，即退出操作为输入<code>:q</code>.

- q ( quit ): 退出，如果有未保存的修改则无法退出
- q! ( force quit ): 强制退出
- w ( write edits to disk (save file) ): 保存文件。<code>:w FILENAME</code>即是将当前正在编辑的文件另保存为FILENAME文件，并存储在进入vim的目录下。
- w! ( force write ): 强制保存
- ZZ ( quit and save edits ): 保存文件并退出。等同于wq.
- e! (  revert your changes ): 回滚所有修改至原始状态，也就是说消除所有的编辑结果，回到原来的文件。


#### 在当前行 ( current line ) 有效的移动光标
当光标从一点移动到另外一点，在这两点之间的文本（包括这两个点）称作被“跨过”，这里的命令也被称作是 **motion**。（简单说明一下，后面会用到这个重要的概念）

- fx ( forword to x or find x ): 移动光标到 *当前行* 的 下一个 x 处，x 表示任意单个字符，区分大小写。例：fa:移动光标到当前行的下一个字母a处。
- Fx : 同上，区别在于方向相反，移动光标到当前行的上一个 x 处。
- w ( word ) : 光标向前移动一个词。
- b ( back word ): 光标向后移动一个词。
- e ( end of word ) : 移动到字尾。
- 0 ( 数字0 ) ： 移动光标到当前行首。
- $ : 移动光标到行尾。
- ^ ：移动光标到本行第一个非blank字符处。
- g_ : 移动光标到本行最后一个非blank字符处。
- )：移动光标到下个句子。
- (：移动光标到上个句子。

#### 在整个文件 ( file ) 里有效的移动光标
- < c-f > （ Ctrl+forward ）: 向下移动整屏。
- < c-b > （ Ctrl+backward ）：向上移动整屏。
- < c-d > ( Ctrl+down ) : 向下移动半屏。
- < c-u > ( Ctrl+up ) : 向上移动半屏。
- zz : 使光标所在的行成为屏幕的中间行。
- <code>enter</code> : 使光标移动到下一行的第一个字符。
- \+ ：同<code>enter</code>.
- \- : 使光标移动到上一行的第一个字符。
- gg （ go ）: 移动光标到文件首。
- G ：移动光标到文件尾。
- numgg ( num go go ) : 移动光标到指定行即num行。num表示数字，比如 10gg 就是移动到第10行。等价于 numG/:num 。10gg/10G/:10 都是移动光标到第10行。
- \* : 读取光标处的字符串，并且移动光标到它再次出现的地方.
- /text：从当前光标处开始搜索字符串 text，并且到达 text 出现的地方。必须使用回车来开始这个搜索命令。如果想重复上次的搜索的话，按 n。如果想要精确查找的话，不妨在text的前后加上空格。比如我想查找back，但是不想要诸如background之类的词出现，可以输入<code>:/ back </code>，而不是<code>/back</code>。
- ?text：和上面类似，但是反方向。

#### 快速进入插入模式 ( insert mode )
- i （ insert ）：在当前字符的左边插入
- I：在当前行首插入
- a （ append ）：在当前字符的右边插入
- A：在当前行尾插入
- o （ open ）：在当前行下面插入一个新行
- O：在当前行上面插入一个新行
- c （ change ）{motion}：删除 motion 命令跨过的字符，并且进入插入模式。比如：c$，这将会删除从光标位置到行尾的字符并且进入插入模式。ct！，这会删除从光标位置到下一个叹号（但不包括），然后进入插入模式。被删除的字符被存在了剪贴板里面，并且可以再粘贴出来。
- d （ delete ）{motion}：和上面差不多，但是不进入插入模式。

#### 在可视模式 ( visual mode ) 下选中
在 visual mode 选中的内容会被高亮，可能经常会有以下几个操作。

- d：剪贴选择的内容到剪贴板。
- y：拷贝选择的内容到剪贴板。
- c：剪贴选择的内容到剪贴板并且进入插入模式。

#### 在非可视选择模式下剪切和拷贝
- d( delete ){motion}：剪切 motion 命令跨过的字符到剪贴板。比如，dw 会剪切一个词; dfS 会将从当前光标到下一个 S 之间的字符剪切至剪贴板。
- y( yank ){motion}：和上面类似，不过是拷贝。
- c( change ){motion}：和 d( delete ){motion} 类似，不过最后进入插入模式。
- dd ：剪切当前行(至剪贴板)。
- dw : 删除一个单词，不适用于中文。由于vim中对于单词，句子，段落等定义以及像单词的跳转一般距离很小，此类很“细致”的命令似乎并不是十分受用。
- yy：拷贝当前行（至剪贴板）。
- Y：拷贝当前行（至剪贴板）。
- D：剪切从光标位置到行尾(到剪贴板)。
- C：和 D 类似，最后进入插入模式。
- x：剪切(当前字符到剪贴板)。
- s：和x类似，不过最后进入插入模式。

#### 替换(更改)文本

- <kbd>~</kbd> : 游标所在处字符进行大小写替换。
- r ( replace ) : 替换单个字符，不必进入插入模式(insert mode)。 在 normal mode 下将光标停在想要替换的字符处，输入<code>r</code>紧接着再输入想要替换后的字符即可。完成后仍然在normal mode。
- R : 大写的R表示连续替换，直到按下<code>esc</code>.
- cc ( change )：替换整行，即删除游标所在行，并进入插入模式。
- s ( substitute ) : 替换。在normal mode下的<code>s</code>将会删除光标处的字符并进入 insert mode，此时便可进行重新编辑。

#### 粘贴

- p ( paste or put )(小写p) : 在当前行后粘贴。
- P（ 大写P ）: 在当前行前粘贴。

## vim 进阶

### 使用数字

在很多 vim 的命令之前都可以使用一个数字，这个数字将会告诉 vim 这个命令需要执行几次。比如：

- 3j : 将会把光标向下移动三行。
- 10dd : 将会删除十行。
- y3″ : 将会拷贝从当前光标到第三个出现的引号之间的内容到剪贴板。 数字是扩展 motion 命令作用域非常有效的方法。

### 用vim写代码

vim 是程序员专用，自然有一些特性是专门为程序员而设计的。这里是一些常用的：

- \>：缩进所有选择的代码
- \<：和上面类似，但是反缩进
 
### 查找替换

s指substitute（代替，替换的意思），g指global。

- `:s/hello/sky/` 替换当前行第一个 hello 为 sky
- `:s/hello/sky/g` 替换当前行所有 hello 为 sky
- `:n，$s/hello/sky/` 替换第 n 行开始到最后一行中每一行的第一个 hello 为 sky
- `:n，$s/hello/sky/g` 替换第 n 行开始到最后一行中每一行所有 hello 为 sky(n 为数字，若 n 为 .，表示从当前行开始到最后一行)
- `:%s/hello/sky/` 替换所有行的第一个 vivian 为 sky
- `%s/hello/sky/g` 替换所有行中所有 hello 为 sky


### 小技巧

- 比如对于`hello(test)`，**光标停留在括号处**，那么`di(` 表示删除括号里面的内容，即删除括号里面的test内容， 简记为`delete in (`。同理，`(`也可以换成`[`，删除里面的内容。还有类似的`da(`是连同周围的括号一起删除，`delete around (`.

### 调整本行内容位置

- <code>:ce</code> : 在命令行模式下输入 <code>:ce</code> (center)将本行内容居中。
- <code>:ri</code> : 将本行内容居右(right).
- <code>:le</code> : 将本行内容居左(left).


Footnotes:

1.  [Vim学习笔记](http://mturing.com/wiki/wikihtml/Vim%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.html)
2.  学习vi与vim编辑器 第七版 中文 东南大学出版社
3.  [一起来说vim语](http://www.codeceo.com/article/vim-language.html)
