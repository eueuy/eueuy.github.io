---
layout: post
author: Liucheng Xu
title: "Ubuntu shadowsocks 翻墙设置注意事项"
category: 
tags: []
---

* TOC
{: toc}


本文一开始写在CSDN，后来可能由于一些关键字眼被删除。由于有一些阅读量，将将文章再次放到这里以供参阅。

Linux下使用shadowsocks进行翻墙，除了安装好Linux下的ss客户端，扫描二维码配置好ss账号，还需进行**端口设置**才能完成整个操作。

![ss](/assets/img/blog/2016/10-22/ss.png)

如上图，连接后便在本地的1080端口建立起了到远程服务器的socks5连接，然后我们可以根据需要以多种方式调用该端口进行网络访问。比如修改的network proxy, 或采用浏览器加插件（比如chrome使用switchOmega，Firefox直接在高级设置中设置proxy）。

下面以Ubuntu修改network proxy为例：

1. 方法选择`手动`，socks主机设置为127.0.0.1，端口为1080，与上面的ss连接的端口相一致。 

    ![manual](/assets/img/blog/2016/10-22/manual.png)


2. 点击`应用到整个系统`。

network proxy就是这么简单了，然后应该就可以成功翻墙了，至少在我的环境下chrome可以。

对于Firefox可能上面的设置还不够，还需进一步设置代理：
 ![manual](/assets/img/blog/2016/10-22/firefox.png)
