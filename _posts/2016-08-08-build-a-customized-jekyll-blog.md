---
layout: post
author: Liucheng Xu
title: "build a customized jekyll blog"
category: 
tags: []
---

* TOC
{: toc}

### 简单了解并快速构建一个jekyll blog
会简单使用git，GitHub，就可以搭建一个GitHub博客了。在GitHub上搭建博客有多种方式，官方的jekyll, hexo, octopress等多种方式，我选择的是jekyll。

[利用GitHub pages构建一个极简易的页面](https://pages.github.com/)，上面的完成后可以在`your_username.github.io`地址进行访问。接下来便可以进行下一步的工作: [Blogging with jekyll](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/). 网上有很多jekyll blog的教程，可以先看一下了解概况，在这里并不会进行详细介绍。

跟着教程走，顺利的话，在jekyll blog目录下，比如下图中的liuchengxu.github.io目录，`bundle exec jekyll serve --watch`就可在本地的`localhost:4000`看到效果

![github-io](/assets/img/blog/2016/08-08/github-io.png)

### 个性化blog风格
[jekyllthemes](http://jekyllthemes.org/)上有很多jekyll blog的主题，都很漂亮，喜欢的话可以先试一下。

![jekyllthemes](/assets/img/blog/2016/08-08/jekyllthemes.png)

不过时间久了以后，就会想自己构建一个主题，需要什么便添什么，因为现有的jekyll theme可能有很多样式并非我们想要。比如[Yummy Theme](https://github.com/DONGChuan/Yummy-Jekyll), 里面会有各种文件和文件夹，一开始可能不太知道这些文件的作用，想修改也不是很方便。

下面以我的[GitHub博客](https://github.com/xuliuchengxlc/xuliuchengxlc.github.io)为例，简要介绍一下其中各部分：

![structure](/assets/img/blog/2016/08-08/structure.png)


文件(夹)                            | 作用
:---:                               | :---:
_config.yml                         | 必须，显然是一个配置文件，里面的内容可以通过jekyll定义的方式进行读取
_posts                              | 必须,该文件夹下放置博文, 命名形式为2016-08-08-your-post-name.md.
_includes                           | 非必须，但是为了重用一些内容，可以将它们放到这里，再通过include命令进行引用
_layouts                            | 必须，里面为博客里面使用的一些模板样式，比如你在md文件里面的yml头使用了`layout: post`, 那么`_layouts`里面就该有一个post.html。
CNAME                               | 自定义域名。不了解的话可以delete
Rakefile                            | 下一节会介绍
其他文件, 诸如404.html, feed.xml... | 从已有的主题保留即可, 不改动应该也不会影响


### 自动化新建post
jekyll blog中markdown博文的文件头, 比如下面这样，每篇如果手写的话显得比较费时费力。

``` yml
---
layout: post
author: Liucheng Xu
title: "build a customized jekyll blog"
category: 
tags: []
---
```

可以使用Rakefile来自动化完成这样的操作，Rakefile类似Makefile, 不过是针对ruby而已。可以参考[jekyll-bootstrap Rakefile](https://github.com/plusjade/jekyll-bootstrap/blob/master/Rakefile), 从中抽取所需内容照葫芦画瓢进行修改。比如我的Rakefile:

``` ruby
require "rubygems"
require 'rake'
require 'yaml'
require 'time'

SOURCE = "."
CONFIG = {
    'layouts' => File.join(SOURCE, "_layouts"),
    'posts' => File.join(SOURCE, "_posts"),
    'post_ext' => "md",
}

# Usage: 
# rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]] [category="category"]
desc "Begin a new post in #{CONFIG['posts']}"
task :post do

    abort("rake aborted: '#{CONFIG['posts']}' directory not found.") 
    unless FileTest.directory?(CONFIG['posts'])
    title = ENV["title"] || "new-post"
    tags = ENV["tags"] || "[]"
    category = ENV["category"] || ""
    category = "\"#{category.gsub(/-/,' ')}\"" if !category.empty?
    slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
    
    begin
        date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    rescue => e
        puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
        exit -1
    end
    
    filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
    if File.exist?(filename)
        abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end

    puts "Creating new post: #{filename}"
    open(filename, 'w') do |post|
        post.puts "---"
        post.puts "layout: post"
        post.puts "author: Liucheng Xu"
        post.puts "title: \"#{title.gsub(/-/,' ')}\""
        post.puts "category: #{category}"
        post.puts "tags: #{tags}"
        post.puts "---"
        post.puts "\n"
        post.puts "* TOC"
        post.puts "{: toc}"
        post.puts "\n\n"
    end
    exec "vim + #{filename}" # 新建后打开并定位到最后一行
end # task :post
```

这样，便可以通过`rake post title="new post" `新建post.
