---
layout: post
title: My First Post
author: Liucheng Xu
tags: Others
math: y
disqus: y
---
![win屏幕截图](/assets/img/blog/2016/01-20/win-screen.png)
本博客初次建立的时间为2016年1月初，当时采用的 markdown engine 是 redcarpet。但是Github pages自2016年5月1号起将仅支持kramdown，故将redcarpet修改为kramdown.本文的主要作用为测试最终渲染效果。

[github broadcasts相关内容节选：](https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0)

>Starting May 1st， 2016， GitHub Pages will only support Kramdown， Jekyll's default Markdown engine. If you are currently using Rdiscount or Redcarpet we've enabled Kramdown's GitHub-flavored Markdown support by default， meaning Kramdown should have all the features of the two deprecated Markdown engines， so the transition should be as simple as updating the Markdown setting to kramdown in your site's configuration (or removing it entirely) over the course of the next three months.


This is *red*{: style="color: red"}.

- 行间公式

    $$ a^2 + b^2 + c^2= d^2 $$

- 行内公式

     $ a^3+b^3=c^3 $

- 代码块

```ruby
 require 'redcarpet'
 markdown = Redcarpet.new("Hello World!")
 puts markdown.to_html
```

- 表格

 一些简单样式 | 效果
 :--------:   | :--------:
italics       | _italics_
删除线        | ~~删除线~~
强调          | *强调*
加粗          | **加粗**
行内代码      | `行内代码`
超链接        | [homepage](https://liuchengxu.github.io)


- 引用

> It is our light， not our darkness that frightens us.

- 代码高亮

```java
class Hello{

    pulblic void test(){
        System.out.println("test");
    }

    public static void main(String[] args){
        System.out.println("hello");
    }

}
```
