---
layout: post
author: Liucheng Xu
title: "重建macOS使用与开发环境"
category: 
tags: []
---

* TOC
{: toc}


近来由于mac出了点小问题一直没有解决，加上用了已经有好长一段时间，一直想清理一下(精神洁癖作祟)，所幸重装了事。没有使用TimeMachine，所以需要先行将一些觉得尚有价值的文件拷贝下来。

写下这篇文章，也是为了以后再次遇到问题能够有章可循，不至于胡乱一通重装。本文也会持续更新，欢迎大家分享更好的经验。

### 重装macOS

在线安装系统实在太慢，进行在windows下制作macU盘启动盘重装系统.  额外所需的物理设备为U盘一个，8G及以上为宜。

#### 1. 准备软件与镜像

- 下载软件TransMac
[TransMac官网下载地址](http://www.acutesystems.com/scrtm.htm)，有15天试用期。

- 下载镜像文件
如果身边有mac, 可以直接从app store进行下载，如果没有也可以自行搜索从网盘进行下载。这里是我使用的镜像文件: 链接: https://pan.baidu.com/s/1eRXmcMI 密码: s5u6.  版本不重要，安装成功以后更新即可。

#### 2. 制作mac的U盘启动盘

1.  将U盘接入电脑，打开TransMac，在左侧窗口找到U盘。

2. 将U盘格式化为Mac下的格式：选中U盘 >> 右键选择 `Format Disk for Mac`。以前版本的TransMac可能在这个时候还会出现一些选项让你进行选择，现在的最新版本已经没有了。

3. 格式化完成后，选中U盘 >> 右键选择`Restore with Disk Image`选项，选择刚刚下载的macOS镜像文件(.dmg).

4. 时间可能会有点久，耐心等待TransMac将镜像文件写入U盘。大概需要20分钟。

#### 3. 使用制作好的U盘安装macOS

1. 将制作好的U盘接入mac并启动，按住option键，系统显示可用启动盘后选择U盘并进入。

2. 选择`Install OS X`，接下来的操作都很简单，一看就知道怎么做。

3. 耐心等待安装完成。

**注意事项!!! 注意事项!!! 注意事项!!!**
在革命即将取得胜利的时候，很可能会遇到这么一股顽固分子：

>安装 OS X Yosemite”应用程序副本不能验证，可能下载过程中有损坏之类的问题出现导致安装终止。

这个问题导致我重装两次失败，浪费了很多时间，最后在v2ex发帖有位朋友给出了知乎的链接：
打开终端输入：`date 062614102014.30`, 这里是[解决方案的知乎链接](https://www.zhihu.com/question/19812727).

### 重建软件环境

系统重装好以后就是安装软件。软件大致可分为两类，一是普通用户日常所需的一些软件，二是程序员身份所对应的开发环境。先来恢复开发环境，因为使用的homebrew的`brew cask`可以帮助安装一些常用软件。

在恢复开发环境之前，有一些问题可能还是必须要先解决的，比如网络问题(翻墙)，不翻墙邮箱连gmail等没法使用。

- Shadowsocks
我使用shadowsocks，所以需要手动下载ShadowsocksX软件，然后配置完成翻墙功能。这部分就不多说了。

- chrome
我以前试过使用brew cask进行安装chrome，不过可能由于墙的原因，始终无法下载。故采用手动下载安装。能够翻墙以后，在chrome安装完成后登录google账号就会自动恢复你的书签，插件等等了，十分方便。

- 输入法
如果要输入中文，要么在 `系统偏好设置` >> `键盘` >> `输入源` >> 左下`+`号添加`简体拼音输入源`。要么自行下载输入法，比如常用的搜狗输入法, 或者可以待会儿使用`brew cask install sogouinput`进行安装。

- xcode
  xcode的command line tool在后续操作中用到，先安装整个xcode吧，有备无患， `App Store`  >> `xcode`。Xcode动辄4到5个G，网速不快的话这里还是挺闹心的。

  安装完成后，记得要先打开一次完成components的安装。否则homebrew可能无法顺利安装。

- 使用习惯
  我习惯将左上触发角设置为Launchpad, `系统偏好设置` >> `Misson Control` >> 左下`触发角` >> 左上`Launchpad` >> `好`。同样设置左下触发角为`桌面`。

  将F键区设置为标准功能键， `系统偏好设置` >> `键盘` >> 选中`将F1、F2 等键用作标准功能键`。

  移除Docker中能够移除的所有图标，这样Docker中显示的就都是已经打开的应用程序。

#### 开发环境恢复

首先安装[homebrew](http://brew.sh/): 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
```

`brew install`安装命令行软件，`brew info APP` 可以查看有哪些安装选项， `brew cask install`安装GUI软件。目前`brew cask`更新软件可能没那么优雅，可以根据APP自己更新，也可先卸载再重新安装`brew cask uninstall APP && brew cask install APP`, 更多用法可自行搜索。

安装好brew以后，有一些软件是必备品，比如git, wget. 我把dotfiles放在了github上，里面维护了一个`brew_for_new.sh`放置brew的部分安装清单, [这里是我的 dotfies github地址](https://github.com/liuchengxu/dotfiles). 因此执行下面的命令即可安装brew必备的一些软件：

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/liuchengxu/dotfiles/master/brew_for_new.sh)"`
```

顺便再将dotfiles克隆到本地：
```
git clone https://github.com/liuchengxu/dotfiles.git ~/dotfiles
sh ~/dotfiles/bootstrap.sh
```

3. [oh-my-zsh](http://ohmyz.sh/)
   `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

  安装powerline fonts, 我的vim配置与[powerline fonts](https://github.com/powerline/fonts)搭配效果更佳：
      
 ```
git clone https://github.com/powerline/fonts.git ~/.fonts && bash ~/.fonts/install.sh
 ```  

4. iterm2
  `brew cask install iterm2`, 安装完成后克隆终端主题，毕竟默认主题太“素”，且item2，系统自带terminal都可以用。

  在用户目录下新建一个GitHub目录，以后从GitHub克隆的repo都可以放到这里：
```
cd ~/GitHub && git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
```
  为iterm2设置一个类似Guake的功能，
  `iTterm2` >> `Profiles`, 添加一个叫做Guake的profile >> `Window` >> `Style`选择 `Fullscreen` , 然后设置ITerm2的热键，`iTerm2` >> `Keys` >> `Hotkey`, 我习惯将Hotkey设置为F12.

![iTerm2 Fullscreen](http://upload-images.jianshu.io/upload_images/127313-8ad56a756be714ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Guake Hotkey](http://upload-images.jianshu.io/upload_images/127313-d079e2a19eae40c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




5. vim
我有一个vim配置放在github上，执行下述命令即可一键安装：
```
brew install vim --with-lua --with-override-system-vi --with-python3
brew install macvim --with-lua --with-override-system-vim --with-python3
# YouCompleteMe prerequisites
brew install cmake
```



6. [spacemacs](https://github.com/syl20bnr/spacemacs)
  安装emacs:
  ```
  brew tap d12frosted/emacs-plus
  brew install emacs-plus
  brew linkapps emacs-plus
  ```
  克隆spacemacs repo:
  ```
  git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
  ```

7. java
  `brew cask install java`会安装最新版本的java. 如果需要指定版本，自行搜索具体做法即可。

8. python
  `brew cask install anaconda`, 所安装的版本为anaconda3. 如果需要anaconda2, 需要自行从[continum.io](https://www.continuum.io/downloads#osx)下载. 

  非常推荐大家使用anaconda安装python环境，里面的很多工具都非常好用，比如Jupyter Notebook. 如果下载太慢，尝试开启全局ss.

9. sourcetree
```
brew install sourcetree
```
10. 一个文本编辑器
按照个人使用习惯从brew进行安装即可，比如`brew install  sublime-text`， 可以使用`subl`从命令行启动sublime text.

#### 日常使用软件

1. 视频播放器

2. qq
```
brew cask install qq
```

5.  Alfred
```
brew cask install alfred
```

3. 搜狗输入法
```
brew cask install sogouinput
```
仅仅这样还没有完成搜狗输入法的安装，brew有提示:

  ```
  To complete the installation of Cask sogouinput, you must also run the installer at
  '/usr/local/Caskroom/sogouinput/3.7.0.1459/安装搜狗输入法.app'
  ```
  因此，我们需要在终端中执行 `open /usr/local/Caskroom/sogouinput/3.7.0.1459/安装搜狗输入法.app` 才能进一步完成安装。

  我喜欢将候选词个数设置为最大。

4. 欧陆词典
```
brew cask install eudic
```
我买了一个欧陆的注册码，因为它的很多词库很好用。另外因为词典比较常用，给它设置一个快捷键 `Ctrl + 1`，`欧陆词典` >> `偏好设置` >> `快捷键` >> `显示|隐藏《欧陆词典》窗口` >>按`Ctrl + 1`. 以后只要 `Ctrl + 1`， 就可唤出词典。我还将`翻译选中内容`的快捷键设置为`Ctrl + T `。默认的`激活 Light Peek`快捷键与Alfred冲突，去除。

  ![Eudic](http://upload-images.jianshu.io/upload_images/127313-a783bc4f978cd84f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. markdown编辑器
```
brew cask install macdown
```
设置macdown可选项。

6. cheatsheet
```
brew cask install cheatsheet
```

7. mactex
```
brew cask install mactex
```