---
layout: post
title: C、Java、Python如何查找相对路径文件
author: Liucheng Xu
tags: "programming"
disqus: y
---

* TOC
{:toc}

## 前序
之所以写这篇文章是因为遇到了一个Python的莫名其妙的bug折腾了好久，最后发现是由于Python查找相对路径的文件的方式与预想的不符。主要问题在于**如何确定相对路径的当前目录**(python中的os.getcwd()返回的即为当前工作目录)，也就是<code>./</code>。

在Java中，一个Java文件如果要打开相对路径上的一个文件，是以**当前的Java文件位置为基准**进行查找，例如一个hello.java的文件中需要打开一个相对路径为“./test.txt”的test.txt文件，那么在“./test.txt”中的“./”(意为当前目录下)即是指hello.java所在目录下。但是Python却不然，是以**执行Python命令的那个目录为基准**(也就是以执行Python命令的那个目录当做py文件中的相对路径的基准)。对于不了解其中玄机的人自然会跳进坑里。

很简单，做个实验就知道到底如何。下面就C、Java、Python这几种常见的编程语言为例。也可以先看后面的实验结论，有时间的话再仔细看实验过程。如有任何问题，欢迎来信。

## 实验论证
先来看一下实验的整个目录结构，当前用户下的Test文件夹(~/Test)就是整个实验的目录了，下面有两个目录：data目录下有一个train.txt，这就是我们在源文件中尝试读取的文件了，train.txt只有一行内容：

>This is a test.

language目录下面放的是用来测试使用的程序语言源文件，Hello.c， Hello.java， Hello.py：

![tree](/assets/img/blog/2016/03-13/tree.png)

### Java
先看一下Hello.Java，很简单，读取相对路径为“../data/train.txt”的文件并按行输出其内容。按照前面所说，以当前java文件Hello.java所在目录为当前目录的话，这么操作是没有问题的。

![java](/assets/img/blog/2016/03-13/java.png)

在language目录下，<code>javac Hello.java</code> 并 <code>java Hello</code>， 如预期，没有错误。

![java-out](/assets/img/blog/2016/03-13/java-out.png)

在Test目录下，<code>javac Hello.java</code>， 在language目录下，<code>java Hello</code>.没有错误。

![java-out-2](/assets/img/blog/2016/03-13/java-out-3.png)

### Python
测试机器使用的Python版本为 Python 2.7.10.
先看一下Hello.py的内容：

![python](/assets/img/blog/2016/03-13/python.png)

在language目录(也就是Hello.py所在目录，此时Hello.py中尝试打开文件的相对路径以此为基准)下面执行<code>python Hello.py</code>，读取正常，没有错误:

![python-out](/assets/img/blog/2016/03-13/python-out.png)

重点来了，我们换到Test目录下再次执行<code>python language/Hello.py</code>，发生了错误！

![python-out-2](/assets/img/blog/2016/03-13/python-out-2.png)

现在我们还在Test目录下，不过将Hello.py中的相对路径改为以执行Python命令的目录(也就是Test目录下)为基准，即将“../data/train.txt”改为“data/train.txt”. 读取正常:

![python-out-3](/assets/img/blog/2016/03-13/python-out-3.png)

由此可见，Python中查找相对路径上的文件是以执行Python命令的目录为当前目录，并以此为基准进行查找。这一点我不知道是否是Python故意为之，在个人看来并不合理。

进一步实验，如果输出当前工作目录，<code>print os.getcwd()</code>，你会发现执行Python命令的那个目录就是cwd，也就是说Python是以当前工作目录为基准，而当前工作目录就是执行Python命令的目录，py源文件需要读取相对路径上的文件的话，请注意是**以Python的当前工作目录为基准查找**。而这个当前工作目录可变性实在太多，故以为不甚合理。

可以直接打开Python shell，检测上述结论是否正确。

### C语言
先看一下Hello.c内容：

![c](/assets/img/blog/2016/03-13/c.png)

在language目录下，编译运行，没有问题：

![c-out](/assets/img/blog/2016/03-13/c-out.png)

换个目录，在Test目录下，正常编译，但是编译后的可执行文件无法正常打开文件：

![c-out-2](/assets/img/blog/2016/03-13/c-out-2.png)

仍然在Test目录下，将Hello.c文件中的“../data/train.txt”改为“data/train.txt”，再次编译运行. 编译正常，但仍然无法正常打开文件(注意与Python的区别)：

![c-out-3](/assets/img/blog/2016/03-13/c-out-3.png)

紧接着上一步，我们进入language目录，直接运行上一步编译生成的Hello.out（此时我们尝试打开的文件r相对路径为“data/train.txt”）， 居然读取成功！：

![c-out-4](/assets/img/blog/2016/03-13/c-out-4.png)

这下真是有点混乱了，C语言到底是怎么搞的？
经过下面的一番比较（过程比较反复，有兴趣的可以自行实验），我才发现：**C语言是以生成的可执行文件的目录为当前工作目录**！至于上面读取成功是我的一个错误，那个可执行文件是之前生成的。在能够正常读取的情况下，把可执行文件(out文件)放到其他目录的话，你会发现无法再正常读取。
![c-out-5](/assets/img/blog/2016/03-13/c-out-5.png)

## 实验结论

- Java：以Java源文件的所在目录为当前工作目录。Java源文件在哪个目录，那个目录就是用来查找相对路径的基准目录。

- Python：以执行Python命令的所在目录为当前工作目录。在哪个目录下面执行<code>python foo.py</code>， 那个目录就是当前工作目录。

- C语言：以可执行文件的所在目录为当前工作目录，在何处编译无关紧要。可执行文件在哪个目录下面被执行 <code>./foo.out</code>，那个执行的目录就是当前工作目录。

 知道了当前工作目录是什么，以此为基准，自然能够正确设置并找到相对路径上的文件。

