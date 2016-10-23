---
layout: post
author: Liucheng Xu
title: "[译]使用pelican搭建一个数据科学博客"
category:
tags: []
---

* TOC
{: toc}


写博客是一个证明你的技能，进一步加深学习和积累受众的一个非常好的方式。已经有非常多的[数据科学](https://github.com/rushter/data-science-blogs)和[编程](https://www.quora.com/What-are-the-best-programming-blogs)博客帮助它们的作者找到工作，或是建立了非常重要的联系。撰写博客是任何一个有想法的programmer或数据科学家在日常基础之上非常重要的一件事情。

不幸的是，写博客一个不可忽视的障碍便是首先如何搭建一个博客。在本文，我们将会涉及到如何使用Python创建博客，如何使用Jupyter notebook写博客和如何使用GitHub Pages部署博客。读完本文，你应当能够创建属于你自己的博客，并以一种熟悉简单地方式写文章。

### 静态网站

根本上，一个静态网站只不过是一个由HTML文件构成的文件夹而已。我们可以运行一个服务器来使得其他人访问并获取这些文件。它的一个好处就是不需要一个数据库或是其他一些动态交互的部分，而且非常容易将其部署到像GitHub这样的网站。

将你的博客构建成为一个静态网站是一个非常好的想法，因为它维护起来极其简单。创建静态网站的一个方式是手写HTML, 然后将所有的HTML文件上传到服务器。在这样的情况下，你至少需要一个`index.html`文件。如果你的网站URL是`thebestblog.com`, 那么访问者访问`http://thebestblog.com`时将会被展示`index.html`的内容。下面是`thebestblog.com`可能的HTML构成：

```
thebestblog.com
│   index.html
│   first-post.html
│   how-to-use-python.html
│   how-to-do-machine-learning.html
│   styles.css
```

在上面的网站中，访问`http://www.thebestblog.com/first-post.html`将会展示`first-post.html`文件中的内容。`first-post.html`可能像这样：

```html
<html>
<head>
  <title>The best blog!</title>
  <meta name="description" content="The best blog!"/>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <h1>First post!</h1>
  <p>This is the first post in what will soon become (if it already isn't) the best blog.</p>
  <p>Future posts will teach you about data science.</p>

<div class="footer">
  <p>Thanks for visiting!</p>
</div>
</body>
</html>
```

你可能很快会发现手写HTML会有一些问题：

- 手写HTML相当痛苦。

- 如果要写多篇文章，你将不得不复制HTML的风格，和诸如标题，页脚等重复的元素。

- 如果想要集成评论或是其他一些插件，你不得不写JavaScript。

通常来说，当写博客的时候，你希望能够关注内容而不是将时间花费在调整HTML上。幸好，使用静态网站生成器这个工具，你就可以摆脱手写HTML了。

### 静态网站生成器

静态网站生成器允许你使用一个简单的格式写博客文章，比如markdown, 然后定义一些设置即可。生成器将会自动将你的文章转换成HTML。通过静态网站生成器，我们可以将`first-post.html`简化为`first-post.md`:

```
# First post!

This is the first post in what will soon become (if it already isn't) the best blog.

Future posts will teach you about data science.
```
这要比手写HTML要容易得多！一些通常的元素，比如标题或是页脚，可以被放到模板中，所以它们也很容易修改！

有一些不同的静态网站生成器，非常出名的一个便是用ruby写的[jekyll](https://jekyllrb.com/) (译者注：[我的jekyll blog，有兴趣的可以看一下](https://github.com/xuliuchengxlc/xuliuchengxlc.github.io))。由于想搭建一个数据科学博客，所以我们需要一个能够处理Jupyter notebook的静态生成器。

[Pelican](http://blog.getpelican.com/)是用Python写的一个静态网站生成器，它能够将Jupyter notebook文件转换成HTML博客文章。Pelican也十分容易部署到GitHub Pages, 其他人可以在那里阅读我们的文章。

### 安装Pelican

在开始之前，可以在[这里](https://github.com/dataquestio/jupyter-blog)先看一下我们最终完成的一个示例。(译者：[这里是译者搭建的pelican博客, 与原文稍有不同，部署在github的project下](https://liuchengxu.github.io/pelican-blog))

如果你还没有安装python, 那么在开始之前你需要进行一个准备工作的安装。推荐使用`python3.5`。

Python一旦安装完成，我们可以进行以下操作：

- 创建一个文件夹 -- 我们将把博客内容和风格文件放到这个文件夹中。在本篇教程里，我们取名为`jupyter-blog`, 你可以取为任何你喜欢的名字。

- `cd`进入到`jupyter-blog`

- 创建一个叫做`.gitignore`的文件，并添加入[这个文件](https://github.com/github/gitignore/blob/master/Python.gitignore)的内容。最终我们将会把文件提交到git, `.gitignore`将会排除指定类型的文件。

- 创建并激活一个[虚拟环境](http://docs.python-guide.org/en/latest/dev/virtualenvs/)(译者注：此步非必须，如发生问题可以跳过。)

- 在`jupyter-blog`中创建一个叫做`requirements.txt`的文件并写入以下内容：


```
  Markdown==2.6.6
  pelican==3.6.3
  jupyter>=1.0
  ipython>=4.0
  nbconvert>=4.0
  beautifulsoup4
  ghp-import==0.4.1
  matplotlib==1.5.1
```

 - 在`jupyter-blog`下执行`pip install -r requirements.txt`安装`requirements.txt`中的所有包。

### 创建属于你自己的数据科学博客

完成预备工作后，进入`jupyter-blog`目录并执行`pelican-quickstart`将会开始一个交互式的博客安装过程。你将会看到有一系列的问题来使得博客安装妥当。

对于大多数问题，直接点击`Enter`接受默认值即可，需要自定义的地方有the title of the website（网站标题）, the author of the website（作者）,`n` for the URL prefix（URL前缀选择`n`）, and the timezone（时间区）。下面是一个示例（译者：以下内容以后都可在`pelicanconf.py`中再次修改）：

```bash
# xuliucheng @ xlcdemac in ~/pelican-blog [14:39:44]
$ pelican-quickstart
Welcome to pelican-quickstart v3.6.3.

This script will help you create a new Pelican-based website.

Please answer the following questions so this script can generate the files
needed by Pelican.


> Where do you want to create your new web site? [.]
> What will be the title of this web site? LiuchengXu's Blog
> Who will be the author of this web site? LiuchengXu
> What will be the default language of this web site? [en]
> Do you want to specify a URL prefix? e.g., http://example.com   (Y/n) n
> Do you want to enable article pagination? (Y/n)
> How many articles per page do you want? [10]
> What is your time zone? [Europe/Paris]
> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n)
> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n)
> Do you want to upload your website using FTP? (y/N)
> Do you want to upload your website using SSH? (y/N)
> Do you want to upload your website using Dropbox? (y/N)
> Do you want to upload your website using S3? (y/N)
> Do you want to upload your website using Rackspace Cloud Files? (y/N)
> Do you want to upload your website using GitHub Pages? (y/N) y
> Is this your personal page (username.github.io)? (y/N) y
Done. Your new project is available at /Users/xuliucheng/pelican-blog

# xuliucheng @ xlcdemac in ~/pelican-blog [14:43:04]
$ ls
Makefile  content  develop_server.sh  fabfile.py  output  pelicanconf.py  publishconf.py  requirements.txt

```

运行完`pelican-quickstart`后，你会发现在`jupyter-blog`目录下多了两个文件夹：`content` 和 `output`， 还有几个文件。比如`pelicanconf.py`和`publishconf.py`，下面是应当出现的几个文件：

```
jupyter-blog
├── Makefile
├── content
├── develop_server.sh
├── fabfile.py
├── output
├── pelicanconf.py
├── publishconf.py
└── requirements.txt

2 directories, 6 files

```

### 安装Jupyter插件

Pelican默认情况下并不支持使用jupyter写博客 -- 我们需要安装[插件](https://github.com/danielfrg/pelican-ipynb)来进行支持。我们将把插件以[git submodule](https://git-scm.com/docs/git-submodule)的方式进行安装以便于管理。如果你还没有安装git, 可以在[这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)找到一些提示.

git安装好后：

- 执行`git init`将当前文件夹初始化为一个git仓库。

- 创建`plugins`文件夹。

- 执行`git submodule add git://github.com/danielfrg/pelican-ipynb.git plugins/ipynb`来添加插件。

```bash
# xuliucheng @ xlcdemac in ~/pelican-blog [14:45:19]
$ git init
Initialized empty Git repository in /Users/xuliucheng/pelican-blog/.git/

# xuliucheng @ xlcdemac in ~/pelican-blog on git:master x [14:52:21]
$ mkdir plugins

# xuliucheng @ xlcdemac in ~/pelican-blog on git:master x [14:52:37]
$ cd plugins

# xuliucheng @ xlcdemac in ~/pelican-blog/plugins on git:master x [14:52:48]
$ git submodule add git://github.com/danielfrg/pelican-ipynb.git plugins/ipynb
Cloning into '/Users/xuliucheng/pelican-blog/plugins/plugins/ipynb'...
remote: Counting objects: 387, done.
remote: Total 387 (delta 0), reused 0 (delta 0), pack-reused 387
Receiving objects: 100% (387/387), 299.26 KiB | 93.00 KiB/s, done.
Resolving deltas: 100% (190/190), done.
```

现在你应该有一个`.gitmodules`文件和一个`plugins`文件夹:

```
jupyter-blog
├── Makefile
├── content
├── develop_server.sh
├── fabfile.py
├── output
├── pelicanconf.py
├── plugins
├── publishconf.py
└── requirements.txt

3 directories, 6 files


```

为了启动插件，我们需要修改`pelicanconf.py`并将以下内容添加到尾部：

```python
MARKUP = ('md', 'ipynb')

PLUGIN_PATHS = [ './plugins' ]  # 如果像原文直接PLUGIN_PATH = `./plugins`而不使用列表会报warning
PLUGINS = ['ipynb.markup']
```

这几行代码是告诉pelican在生成HTML时激活插件。



### 撰写你的第一篇博文

插件安装完毕后，我们可以来创建第一篇文章：

- 新建一个jupyter notebook并写入一些内容。[这里](https://github.com/dataquestio/jupyter-blog/blob/master/content/first-post.ipynb)是一个示例。

- 将notebook文件复制到`content`文件夹。

- 创建一个跟notebook同名的一个文件，不过扩展名为`.ipynb-meta'。 [这里](https://github.com/dataquestio/jupyter-blog/blob/master/content/first-post.ipynb-meta)是一个示例。

- 将下面的内容添加到`ipynb-meta`文件中，请注意修改部分条目以适应你自己的博客。

```
Title: First Post
Slug: first-post
Date: 2016-06-08 20:00
Category: posts
Tags: python firsts
Author: Vik Paruchuri
Summary: My first post, read it to find out.
```

这里是对上面的一些解释：

- `Title`: 博客文章的题目

- `Slug`: 服务器上这篇文章的访问路径。如果slug是`first-post`, 你的服务器是`jupyter-blog.com`， 那么你可以通过`http://jupyter-blog.com/first-post`来进行访问。

- `Date`: 文章的发布时间

- `Category`: 文章所属目录，可以为空。

- `Tags`: 以空格分割的标签列表，可以为空。

- `Author`: 文章的作者

- `Summary`: 文章的一个简单概述。

### 生成HTML
为了生成博文的HTML，我们需要运行pelican将notebook转换成HTML，然后运行本地服务器就能够看到效果了：

- 切换到`jupyter-blog`文件夹

- 运行`pelican content`生成HTML

- 切换到`output`文件夹

- 运行`python -m pelican.server`

- 打开浏览器访问`localhost:8000`进行预览

你应该能够看到所生成的博客效果。


### 创建一个GitHub Pages
[GitHub Pages](https://pages.github.com/)是GitHub的一个特色，它能够快速部署一个静态网站并通过一个独一无二的URL访问。为完成安装，你需要：

- 如果还没有GitHub账号你需要先注册一个账号

- 创建一个GitHub仓库，名称为`username.github.io`, `username`是你的GitHub名称。[这里](https://help.github.com/articles/create-a-repo/)可以查看更多细节。

- 切换到`jupyter-blog`目录

- 运行`git remote add origin git@github.com:username/username.github.io.git`来为你的本地GitHub仓库添加一个远程仓库。注意将`username`替换为你的GitHub用户名。

GitHub Page将会显示推送到仓库`username.github.io`的所有HTML文件，并可通过`username.github.io`进行访问。

首先，我们需要修改pelican以便于它能够指向正确的地址：

- 修改`pelicanconf.py`中的`SITEURL`, 将它设置为`https://username.github.io`， `username`是你的GitHub用户名。

- 运行`pelican content -s publishconf.py`。当你想要在本地进行预览时，运行`pelican content`. 在部署之前，运行`pelican content -s publishconf.py`，这会使用正确的部署设置文件。

### 提交你的文件

如果你想要GitHub pages这个仓库保存实际的notebook和一些其他文件，你可以使用git分支。

- 运行`git checkout dev`切换到一个叫做`dev`的分支。我们不能使用`master`分支来保存notebook, 因为这个分支为GitHub pages所用。

- 像往常一样提交并推送到GitHub（使用`git add`, `git commit`, `git push`）

### 部署到GitHub Pages

我们需要将博客内容推送到GitHub pages的master分支来使之正常工作。现在，HTML内容已经在`output`文件夹中，不过我们需要它是仓库的根目录，而不是一个子目录。

我们可以用[ghp-import](https://github.com/davisp/ghp-import)：

- 运行`ghp-import output -b master`将`output`中的所有内容导入到`master`分支。

- 运行`git push origin master`将内容推送到GitHub

- 尝试访问`username.github.io` -- 你应该看到你的博客了！

任何时候当你的博客内容有所改变时，重新运行上面的 `pelican content -s publishconf.py`, `ghp-import`和`git push`命令，你的GitHub page就会得到更新。

### 接下来的工作

当博客内容逐渐增多并开始有访客时，你可能会在下面内容上进一步深入：

- 主题
pelican支持主题，你可在[这里](https://github.com/getpelican/pelican-themes)看到很多主题，并选择一个喜欢的使用。

- 定制URL
使用`username.github.io`的确是很好，不过有时候你可能会想要一个更加个性化的域名。[这里](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)是GitHub pages自定义域名访问的介绍。


- 插件
在[这里](https://github.com/getpelican/pelican-plugins)查看插件列表。插件能够帮助添加统计访问，评论，等等很多功能。

- 博客推广

	尽力在一些网站上推广你的博客来获取观众，比如CSDN, 简书，知乎等等。

------

译者：

上面的部署部分只讲了部署到`username.github.io`, 这里讲一下部署到`username.github.io/project`的注意事项（[因为有坑](https://github.com/getpelican/pelican/issues/1526)）。

- 【注意点】在`pelicanconf.py`中设置`SITEURL`, 格式为 `https://liuchengxu.github.io/project`.

```
SITEURL = 'https://liuchengxu.github.io/pelican-blog'

```

- 在pelican-blog目录下，将output目录下的内容推送到gh-pages分支：

	```
	ghp-import output -b gh-pages
	git push origin gh-pages
	```

- 现在可以在`username.github.io/project`进行访问了，比如我的[pelican-blog](https://liuchengxu.github.io/pelican-blog) . 至于日常更新在master分支即可。

[原文地址:Building a data science portfolio: Making a data science blog](https://www.dataquest.io/blog/how-to-setup-a-data-science-blog/)
