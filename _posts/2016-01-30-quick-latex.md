---
layout: post
title: 从零开始LaTeX快速入门
tags: LaTeX
author: Liucheng Xu
disqus: "y"
math: y
donate: y
published: true

---

* TOC
{:toc}

对于理工科的学生来说，尤其是从研究生阶段开始，LaTeX应该会是日常中必不可少的写作工具。毕竟要写什么公式的话，不用LaTeX实在是不知道要怎么办。况且要是投稍微专业一点的论文，LaTeX是必须的，没人会接收Word文件。

此篇为写给一些想快速入门LaTeX的朋友，至于为什么要叫从零开始，因为我就是从零开始学会的LaTeX。如果你不是那么“聪慧”，LaTeX可能的学习曲线会显得比较曲折。但熟能生巧，这些不过些工具而已，没有学不会，用不好的道理。本人学识与能力有限，以下内容如有纰漏或错误，欢迎来信。

关于本文与其他一些介绍LaTeX的文章说明，我几乎没有看到有将LaTeX与web page进行比较学习，这也可以算作是迁移学习，只要稍微懂得一点网页的知识都可以了解LaTeX的运作方式。本文并不十分正统，仅当快速学习的经验分享。

LaTeX始终是个工具，快速使用活学活用才是硬道理。笔者从第一次使用LaTeX从完成几十页毕业论文的LaTeX大工程(文末有提供我的毕业论文LaTeX源文件)，期间不过两三个月而已， 而除去内容上的准备，在LaTeX调整样式上也不过几周时间。所以要相信只要得法， 其实LaTeX很简单，不要被网上一些LaTeX的学习曲线很陡的说法而心生畏惧。

## LaTeX概览

摘自维基百科：

>LaTeX， 是一种基于TEX的排版系统，由美国电脑学家莱斯利·兰伯特在20世纪80年代初期开发，利用这种格式，即使用户没有排版和程序设计的知识也可以充分发挥由TEX所提供的强大功能，能在几天，甚至几小时内生成很多具有书籍质量的印刷品。对于生成复杂表格和数学公式，这一点表现得尤为突出。因此它非常适用于生成高印刷质量的科技和数学类文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。

简单点说：LaTeX基于TeX，目的主要是为了方便排版。在学术界的论文，尤其是数学、计算机等学科论文都是由LaTeX编写。

**我的一点理解：**
在稍微了解一点LaTeX后，你会发现**LaTeX的工作方式类似web page**，都是由源文件（.tex or .html）经由引擎（TeX or browser）渲染产生最终效果（得到PDF文件 或者 生成页面）。两者极其神似，包括语法规则与工作方式。所以呢，与HTML一样，LaTeX其实很简单。

![sketch](/assets/img/blog/2016/01-30/sketch.png)

一般的规范写法中都是在HTML文件中写入web page的结构内容，再由css控制页面生成的样式。当然你也可以选择在HTML直接写入样式内容，不过这并不提倡。同样，在LaTeX有着同样的情况，你可以在tex源文件中同时写入主题内容和样式，也可以内容与样式分离，以网络上流传广泛的[清华大学LaTeX模板](https://github.com/xueruini/thuthesis)为例，以.cls(class)结尾的thuthesis.cls便可看作是与css起到同样作用的样式文件。

我第一次修改清华大学模板就是直接修改的thuthesis.cls与thuthesis.cfg文件。直接从ins和dtx文件开始做的话要花费很多学习如何编写宏包的成本，我的本科毕业论文时间并不多，只能在cls文件上直接修改，虽然会很ugly。

LaTeX中还有宏包的概念，`\usepackage{foo}`即可使用宏包foo中定义的内容。这跟C语言的`include`是一样的，将文件加载进来进行使用。所谓宏包就是一些写好的内容打包出来以便大家使用而已。利用宏包，我们可以使用很多现成的好用的样式。当然了，如果要编写一个自己的个性化的宏包也是可以的，不过需要学习成本而已。  

初期的话，我们可以选择一个LaTeX模板进行改造。不过第一次见到一些模板的话，可能会对很多文件的作用一头雾水，下面是简单的介绍，详细内容可见[在LaTeX中进行文学编程](http://liam0205.me/2015/01/23/literate-programming-in-latex/)，当然更多介绍的话可以自行搜索。


LaTeX模板常见文件类型 | 功能简要介绍
:---:|:---:
.dtx|**D**ocumented La**T**e**X** sources，宏包重要部分
.ins | installation，控制 TeX 从 .dtx 文件里释放宏包文件
.cfg|config， 配置文件，可由上面两个文件生成
.sty | style files，使用<code>\usepackage{...}</code>命令进行加载
.cls |classes files，类文件，使用<code>\documentclass{...}</code>命令进行加载
.aux|auxiliary， 辅助文件，不影响正常使用
.bst | BibTeX style file，用来控制参考文献样式

class与style好像内容很像的感觉，在功能上的确很相似，但是也有区别。[这里是关于.cls与.sty文件的区别](https://tug.org/pracjourn/2005-3/asknelly/nelly-sty-&-cls.pdf)


[额外推荐阅读材料:来自北京大学李东风老师的LaTeX排版心得](http://www.math.pku.edu.cn/teachers/lidf/docs/textrick/tricks.pdf)

## 安装配置LaTeX

LaTeX配置环境很简单，只需2步即可：

1. 根据平台选择一个TeX发行版进行安装，建议选择最全功能最多的版本。TeX发行版的概念相当于Linux及其发行版，Linux内核虽然只有一个，但是有很多基于内核的不同特色的Linux发行版，Ubuntu，Fedora等等不胜枚举。

    - TeXLive for three platforms

        [TeXLive](https://www.tug.org/texlive/)
    - Windows

        [CTeX套装](http://www.ctex.org/CTeXDownload)
    - Mac

        [MacTeX](http://tug.org/mactex/)

    windows用户推荐TeXlive，不推荐CTeX。我一开始安装的是CTeX，在TeXstudio里面总有一些莫名其妙的错误，比如明明定义了一个命令，在log里面还是会显示error：undefined control sequence，换了TeXlive就没有那些莫名其妙的错误了。TeXlive在线安装太慢了，安装包太大，2.66G，这里是我分享的[2015 TeXlive 离线安装包 百度云盘链接](http://pan.baidu.com/s/1jHfUzWy)， 提取密码2cj2，解压缩后运行install-tl-windows.bat即可。mac用户推荐使用MacTeX.

2. 选择一个合适的LaTeX编辑器。

    在安装好LaTeX环境以后，通常都会有一个自带的编辑器，比如CTex的WinEdt， Mac下的TeXShop， 不过功能并不强大，就好比Windows记事本，只有一些基本的文本编辑功能。

    在这里推荐一个我目前觉得还不错的LaTeX编辑器：**TeXstudio**。我试过WinEdt，TeXnicle，不过都比不上TeXstudio。在WinEdt下面无法编译的文件，居然可以在TeXstudio中编译生成最终效果，虽然log里面显示error，但的确产生了效果。不管怎么说，用TeXstudio就对了。它使用qt写的，还跨平台。

    TeXmacs有兴趣的也可以了解一下，[王垠也在博客中推荐过](http://www.yinwang.org/blog-cn/2012/09/18/texmacs)。

## 开始第一个LaTeX文档

打开TeXstudio，新建一个TeX文件，写入以下内容：

``` tex
\documentclass{article}
\begin{document}
Here comes \LaTeX!
\end{document}
```

点击 <code>F5</code>（默认快捷键）compile and view，即可看到效果。

![TeXstudio](/assets/img/blog/2016/01-30/screen.png)

至此，一个极简易的LaTeX文档已经完成。以后的内容不过是多用多查，熟能生巧。
记得找本LaTeX的书籍看一下，一来对于更为精细的知识做一个了解，二来可以作为工具书有问题的时候查询。我经常查的是 <<LaTeX入门与提高 第二版>>。

## LaTeX数学公式

学习LaTeX的一大初衷其实是为了书写数学公式。
数学公式的练习始于markdown，因为很多markdown编辑器是支持LaTeX数学公式的，比如haroopad。那么不仅可以写出漂亮的公式，还能方便做笔记。

以下内容直接在支持数学公式的markdown编辑器中即可操作，而且是即时显示效果，对新手很有帮助。如果使用haroopad编辑器，请在**偏好设置**中**启用数学表达式**。

**学会书写LaTeX数学公式，只需要了解4个概念：**

1) 数学公式环境。
LaTeX 的数学模式有两种：行内模式(inline)和行间模式(display)。前者在正文的行文中，插入数学公式；后者独立排列单独成行。

在行文中，使用<code>\$ ... \$</code>可以插入行内公式，使用<code>\$\$ ... \$\$</code>可以插入行间公式，如果需要对行间公式进行编号，可以使用equation环境：

```
\begin{equation}
......
\end{equation}
```


2) 凡是键盘不能够直接表示的符号或者起着特定作用的皆有命令，类似转义，叫做**控制序列（control sequence）**。比如求和符号$\sum$对应的命令为 <code>\sum</code>.

3) 上下标。<code>_{...}</code>表示下标，<code>^{...}</code>表示上标。它默认只作用于之后的一个字符，如果想对连续的几个字符起作用，请将这些字符用花括号{}括起来， 也就是下面分组的概念。

4) 分组。很简单，就是用<code>{...}</code>将内容包含起来视作整体，比如上下标很长的时候。遇到什么时候得到的效果不是预期，那么很可能你需要加个分组，也就是添个大括号<code>{...}</code>.

| LaTeX命令 | 预览效果 |
|:--------:|:--------:|
|<code>\$ x_i \$</code>      |  $x_i$      |
|<code>\$ x^2 \$</code>|$x^2$|
|<code>\$ x^ {y^z}\$</code>|$x^{y^z}$|
|<code>\$ \int_a^b f(x)\$</code>|$\int_a^bf(x)$|
|<code>\$ \frac ab \$</code>|$\frac ab$|

有了这几个概念以后，再动手写几个就大概懂了。无论多么复杂的公式都是有一个个简单的东西构成。推荐一个网站：[MathJax basic tutorial ](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).

## LaTeX中文支持

不同环境具体操作有所不同，下面介绍Windows与Mac平台。

**Windows**
Windows平台比较简单， 引入CJK宏包并应用CJK环境即可。

```tex
\documentclass[11pt]{article}  %百分号表示注释
\usepackage{CJK}               %引入CJK宏包
\begin{document}               %begin与end成对出现
\begin{CJK}{UTF8}{song}        %应用CJK环境
你好
\end{CJK}
\end{document}
```

LaTeX将

``` tex
\begin{...}
content
\end{...}
```

称为<code>...</code>环境。在对应环境中 content 产生对应效果。

![winedt](/assets/img/blog/2016/01-30/winedt.png)

还有一个更方便的方式，直接使用<code>ctexart</code>模板即可:```\documentclass[UTF8]{ctexart}```

**Mac**
系统为osx 10.11.3， Mac添加中文支持稍微多几个操作，除了引入xeCJK宏包，还要设置字体名称。
关于设置字体名称，spotlight输入 zitice 打开 mac 的字体册，从字体中选择一个，将其名称填入，如华文楷体的名称为 STKaiti 。
如果没有显示字体名称，请 <code>command+I</code> 或在显示-->显示字体信息即可。

![font](/assets/img/blog/2016/01-30/font.png)

![macchinese](/assets/img/blog/2016/01-30/MacChinese.png)

## 几个LaTeX推荐网站

- [Detexify LaTeX handwritten symbol recognition](http://detexify.kirelabs.org/classify.html).

通过手写识别LaTeX符号，识别率很高。
尤其是当看到一个符号却不知道其LaTeX命令的时候它很有用。只要画出记忆中符号的样子，就会自动出现各种可能想要的表示方法。

- [LaTeX公式编辑器](http://zh.numberempire.com/texequationeditor/equationeditor.php)

对于尚不熟悉的人书写LaTeX公式提供一点便利。

- [在线LaTeX编辑器shareLaTeX](https://cn.sharelatex.com/)

好处就是不用本地搭建环境，有中文界面，直接在线操作。还有很多LaTeX模板可供选择。

##  完成本科论文后对于LaTeX初学者的建议

在本文前些部分我写到想通过修改清华大学的LaTeX模板改造为我的本科院校给出的word模板样式，不过最后这条路还是行不通。反思其原因是一直想写个宏包出来，从thuthesis.cls到szuthesis.cls，最好能够一次性做出一个模板出来。但是始终由于这样那样的原因没有时间给我去折腾了，太多错误无法解决，一口吃不成个胖子。最后我选择基于ctexart的基本样式进行修改，在tex源文件混杂了样式内容，从源代码的角度看虽然不漂亮，但是对于完成本科论文绰绰有余了。在tex源文件里修改ctexart的各种样式实在是容易上手的多。

说了这么多就是：**初学者轻易不要尝试修改现有的模板样式文件**，除非你知道如何写一个宏包。完成样式即可，不用在乎源代码是否优雅。

下面修改样式的过程的一些经验，最好在看一下下面的资料在对ctexart进行修改，会更有效：

- 查看宏包说明

比如我装的TeXLive2015在`C://texlive`目录下，打开`C://texlive/2015/doc.html`，你就会发现各种文档。请**仔细阅读一遍ctex.pdf**会很有用。

![font](/assets/img/blog/2016/01-30/doc.png)

- 一个很重要的查看宏包手册命令: 打开cmd， 输入`texdoc 你想要查询的宏包名 `， 比如<code>texdoc caption</code>，就会打开caption宏包手册。诚然可以网上查找解决办法，但是如果有空的话必然是查看官方手册更靠谱更全面.

- 多查多用，熟能生巧。

### 额外推荐

我的本科论文LaTeX源文件已经放到了GitHub上，对于初次使用LaTeX写论文的问应当具有一定的借鉴意义，在源文件中我做出了诸多注解。此外论文内容关于推荐系统，如果有人做相关方向也可看一下。

[论文GitHub地址](https://github.com/xuliuchengxlc/szuthesis)

[论文简介](xuliuchengxlc.github.io/szuthesis/)

[LaTeX写的beamer幻灯片](http://www.liuchengxu.org/szuthesis/presentation.pdf)

![](/assets/img/blog/2016/01-30/beamer.png)

如果您已经懂得了基础操作，不妨看一下我在CSDN记录的一些LaTeX使用注意点，里面积累了我在LaTeX使用过程中的很多经验：[LaTeX实战经验：新手须知](http://blog.csdn.net/simple_the_best/article/details/51244631)
