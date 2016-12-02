---
layout: post
title: 打造一个简洁高效的 Windows 环境
tags: Windows
author: Liucheng Xu
disqus: "y"
published: true
tagline: utility

---

* TOC
{:toc}

工欲善其事，必先利其器。作为一个从事计算机行业的人，可能每天面对的最多的就是我们的电脑了，那么一个可心的操作环境显然必不可少。不仅仅是为了心情愉悦，同时也是为了提高工作效率。

目前我一般使用两台电脑：台式机加上一台Retina MacBook pro。台式机主要是屏幕大，配置高，什么时候不满意了还可以自主升级配置，而mac自然是类unix系统和视网膜屏幕让我爱不释手。虽然mac很漂亮，但win才是我的主力机。即使Windows没有os x外表那么华丽，不过经过一番努力还是能够提升它的颜值与实用性。

这里可能并不会做很多关于软件详细的介绍，希望大家能做个有心人，不断尝试发现新的东西。在这里我只是将我的一些使用经验分享出来供大家参考。

有时候我们觉得软件不好用可能只是因为我们自己不会用而已，如果有任何相关问题，欢迎点击上方发邮件给我一起交流。

下面就来介绍一下我的Windows环境配置，操作系统为Windows 10 64位.

![win屏幕截图](/assets/img/blog/2016/01-20/win-screen.png)



## 提高效率

### Everything

**快速检索文件**

相信很多人都知道everything，当我们需要找到某个文件时非它莫属，功能十分强大。
搜索几乎是秒出结果，堪比mac下的spotlight，而且支持正则表达式搜索。

### Executor

**自定义快捷键**，快速启动程序。

虽然知道这个软件是因为它能够像mac下面的Alfred能够快速唤出程序，但是因为这个功能体验并不好，实际我用的最多的是它的自定义快捷键功能。

executor的keywords设置很简单，在setting里面Add Keywords就可以设置相关快捷键。
我的一些快捷键设置如下：

| 快捷键                | 软件                 |
| --------              | --------:            |
| <code>ctrl</code> + 1 | Chrome               |
| <code>ctrl</code> + 2 | Firefox              |
| <code>ctrl</code> + 4 | 欧路词典 or 有道词典 |
| <code>ctrl</code> + 5 | 福昕阅读器           |
| <code>ctrl</code> + 7 | SumatraPDF           |
| <code>ctrl</code> + 0 | everything           |

这些快捷键时常也会增增减减，但大体就是如此了。

![executor](/assets/img/blog/2016/01-20/executor.jpg)

记忆起来也很简单，everything最常用就用<code>ctrl</code>+0，<code>ctrl</code>辅以1，2为常用的两个浏览器，4、5、7分别于对应的软件名字数。

另外关于everything的快捷键设置有些不同，不能勾选only one instance running选项，否则快捷键无效。

### QTTabBar

**增强文件资源管理器**

与QTTabBar类似的还有一个非常好的软件clover，可惜在win10下面的表现实在是糟糕，频繁崩溃，期待更新。

### SumatraPDF

**用类似vim的方式阅读pdf**，可以实现大部分全键盘操作。例如 jk 分别对应上下移动， / 对应查找。这也是CTeX套装里面自带的PDF阅读器。

### AltDrag

Linux系统有个 Focus-follows-mouse 功能：鼠标指哪，键盘的焦点就指向哪，甚至可以让鼠标所在的窗口实时处于最顶层，达到真正的焦点跟随鼠标。

Mac 也有类似的设计：鼠标指向哪个窗口就可以对那个窗口进行滚动操作。也就是说，当你正在编辑某个文档，想滚动旁边的网页时，不用再点一下才能滑动。对于经常同时编辑多个文档的人来说异常实用。
在 Win 10 有自带这个功能，而之前的 Win 版本则需通过 AltDrag 这样的软件来实现。

AltDrag 除了能实现鼠标指哪滚哪的功能之外，还有一项非常实用的功能 —— **按住 Alt 键就可以通过点击窗口的任何位置移动窗口**。
不必像之前非得将鼠标移动到窗体顶部，在一定程度上使得操作更加便利，并且不会让被移动的窗口获得焦点。
当在 Word 中打字到一半，临时想调整其它窗口位置再继续打字时，这个功能真的非常方便。

AltDrag开源免费。

### SnapCrab

SnapCrab 是一款「十项全能」的截图软件，功能非常齐全。
除了全屏截图、窗口截图、选区截图的基本功能之外，SnapCrab 还能获取鼠标指向处的 RGB 值，是一个优秀的取色器，并且可以自定义选取部分屏幕来快速分享.

SnapCrab开源免费。


### Seer

Mac 还有一个相当方便的功能：通过单击空格键来对几乎所有格式的文件进行预览，Seer 这款软件将这个功能搬到了 Win 上。

在格式兼容性方面，Seer 表现不错，基本上像 jpg、png、MS Office、PDF、txt、音乐、视频等等基础格式都能很好地兼容。并且 Seer 提供了扩展功能，让我们对其未来的文件格式兼容性有了更美好的想象。

开源免费。

### VistaSwitcher

**简化任务切换窗口**

虽然win10的任务切换也还不错，但是窗口开多了以后实在是显得有些眼花缭乱。

虽然VistaSwitcher也是许久没有更新，不过在win10的表现除了一点小瑕疵，还算不错，很稳定。

- 注意一下外观设置：
	- 不要启用Aero blur效果，否则背景会很黑且不通透，显得很难看；
	- 显示任务编号

	![vistaswitcher](/assets/img/blog/2016/01-20/vistaswitcher.png)

### 其他推荐软件

LaTeX编辑器：TeXstudio，跨平台。

windows shell : [babun， a windows shell you will love](http://babun.github.io/)。模拟Unix shell，在Windows体验Unix terminal。

Mobaxterm : [Enhanced terminal for Windows with X11 server， tabbed SSH client， network tools and much more](http://mobaxterm.mobatek.net/)， 不要再用putty了。

翻墙：lantern，开源免费，在[GitHub](https://github.com/getlantern/lantern)下载即可；shadowsocks， www.ss-link.com 现在7块一个月，我买的时候还是5块。

影音播放器：Potplayer，加上下面百度网盘里面分享的Zune皮肤更显酷炫。

词典：有道词典，我喜欢的是它里面的柯林斯词典，但是功能稍显不足，不过胜在免费。欧陆词典，亮点在于它的**可下载词库**，里面有什么词根辞源记忆词典，GRE词汇等等很多词库我觉得很不错。移动端的这些词库是可以直接下载的，但是桌面应用需要购买才能够下载可扩充词库。每次购买可以激活3台电脑。

chrome插件：

Chrome插件    | 功能
:--------:    | :--------:
lastpass      | 保存网站密码，只用记住主密码
cVim          | 用vim的方式使用浏览器，比vimium更酷
广告终结者    | 屏蔽广告
Markdown Here | 用markdown写一封漂亮的邮件
Octotree      | Code tree for GitHub and GitLab

## 提升颜值

### StartIsBack

**美化任务栏**

[这里是分享的免注册版本，直接安装即可。在分享的链接中可能还有其他软件，不过还是大家尽可能的自行下载最新可用版本获得更好的体验](http://pan.baidu.com/share/home?uk=1731296444&view=share#category/type=0)

win10的任务栏实在是不好看，黑色的一条杠横在那儿。用了startisback以后可以将任务栏设置为透明状态，并且还可以设置开始菜单。
可能安装以后有人会找不到程序在哪里，用上面的everything搜索startisback就会发现StartIsBackCfg.exe，运行它就会进入到设置页面。

- 主要是设置任务栏的透明度。
	- **自定义任务栏颜色**改变透明度
	- 选择**使用较大的任务栏图标**。

	![startisback](/assets/img/blog/2016/01-20/startisback.png)

### NetSpeedMonitor

**在任务栏显示网速**

用这个软件主要是想关闭什么管家、杀毒之类的加速球，不喜欢而已。

### MacType

**美化Windows字体**

使用前后的确视觉感受有明显区别，很多软件都有不错的效果。不过也有没办法通过MacType渲染的软件，比如chrome。
我采用的是默认的**LCD**效果，选择以注册表方式加载。



