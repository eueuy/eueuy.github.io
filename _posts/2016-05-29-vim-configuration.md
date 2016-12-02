---
layout: post
title: 终极vim配置：space-vim
author: Liucheng Xu
tags: vim
disqus: "y"
published: true
---

* TOC
{:toc}

[>>>> 终极 vim 配置  space-vim](https://github.com/liuchengxu/space-vim)
==============


<br />
<div class="github-card" data-github="liuchengxu/space-vim" data-width="400" data-height="150" data-theme="default"></div>
<script src="//cdn.jsdelivr.net/github-cards/latest/widget.js"></script>
<br />

![这里写图片描述](http://img.blog.csdn.net/20161201123649104)


![screenshot](https://github.com/liuchengxu/space-vim/blob/master/doc/img/screenshot.png?raw=true)

[spacemacs](https://github.com/syl20bnr/spacemacs) 可能已经成为 emacs 社区中 “唯我独尊”的配置，在 github 上有近万的 star， contributor 众多。它的 “社区驱动”(community-driven) 真的是很强大，贡献的人很多，很漂亮，也很强大。作为 emacs 长久以来的对家 vim, 如果也能有一个这样一个社区驱动的配置，相信也会给大家带来很多便利。

这里也并不想引起“两派战争”，正如 spacemacs 所称，“The best editor is neither Emacs nor Vim, it's Emacs **and** Vim!”, 最好的编辑器既不是 Emacs 也不是 Vim, 而是 Emacs 和 Vim!  所以不管是从实用角度，还是从设计概念，操作哲学的角度，这两个都是非常值得学习的。此外，“编辑器”始终是编辑器，取代不了 IDE，因为吸引我们的更多是深入其中的过程。

vim 社区中，虽有 [spf13-vim](https://github.com/spf13/spf13-vim), [k-vim](https://github.com/wklken/k-vim) 等一些比较有名的 vim 配置，但始终整合的不够，散落着很多适用特定环境的很好的配置，比如针对 c-c++, python, ruby 等等不同语言的 vim 配置。还有大多也不够漂亮（当然了，这个有点主观，不过像我辈肤浅的就是要挑“好看”的-_-）。

随着 vim8 的升级，也会有很多新的更好的插件诞生，比如我用来替代 [syntastic](https://github.com/vim-syntastic/syntastic) 的 [ale](https://github.com/w0rp/ale), ale 使用了异步特性，再也因为语法检查而拖慢速度了。spf13-vim 等的更新似乎不太跟得上步伐，或者说给我们“开箱即用”的选择性不太多。

希望集体智慧能够给我们带来一个更好用的 vim 配置。

### 特色

既然是从 spacemacs 启发而来，自然借鉴了非常多的东西，最重要的一个概念便是 “Layer”. [space-vim](https://github.com/liuchengxu/space-vim) 目前实现了 Layer 加载配置：

```vim
call LayersBegin()

Layer 'fzf'
Layer 'ycmd'
Layer 'html'
Layer 'unite'
Layer 'emoji'
Layer 'c-c++'
Layer 'colors'
Layer 'python'
Layer 'airline'
Layer 'markdown'
Layer 'text-align'
Layer 'programming'
Layer 'better-defaults'
Layer 'syntax-checking'

call LayersEnd()
```

所谓的一个 Layer ，其实很简单，就是加载了一些 vim 的相关插件及其配置，比如 `Layer better-defaults`, 目前包括的插件有：

```vim
Plug 'liuchengxu/vim-better-default'

Plug 'SirVer/ultisnips'
Plug 'honza/vim-snippets'

Plug 'Raimondi/delimitMate'
Plug 'tpope/vim-surround'
Plug 'easymotion/vim-easymotion'

Plug 'Shougo/denite.nvim'
Plug 'Shougo/unite.vim'

Plug 'mhinz/vim-startify'
Plug 'scrooloose/nerdtree',                     { 'on': 'NERDTreeToggle' }
Plug 'Xuyuanp/nerdtree-git-plugin',             { 'on': 'NERDTreeToggle' }
Plug 'tiagofumo/vim-nerdtree-syntax-highlight', { 'on': 'NERDTreeToggle' }

Plug 'bronson/vim-trailing-whitespace', { 'on': 'FixWhitespace' }
```

一个 Layer 下有两个重要的文件：

- `config.vim` : 插件配置
- `packages.vim` : Layer 所需插件

    调整，增加 Layer都非常方便。

### 命令

- `LayerStatus` : 查看启用了哪些 Layer.

### 个性化

个人设置可在 `private` 目录下,  `private` 可以看做是一个 Layer ，可以有两个文件：

- `packages.vim` : 放置自己想要安装的 Layer 和插件，因为使用 vim-plug, 所以按照它的方式来添加插件即可，比如：

```vim
Layer 'python'

Plug 'tpope/vim-fugitive'
Plug 'junegunn/vim-github-dashboard'
```

- `config.vim` : 关于自己添加插件的相关配置放在这里即可。

    如果 `private` 目录下没有 `packages.vim` 与 `config.vim`, 那么 space-vim 仅会加载默认的 Layer.

### TODO

目前还没有在 Windows 下测试，实现了仅 Layer 的按需加载，后续应当还支持一些选项的设置，比如同类插件选择哪一个，以及还有很多文档工作。对于初学者而言，文档可能比什么都重要，装了一些插件不是什么难事，重要的是学会使用这些插件。一个人的精力始终是有限的，欢迎大家分享自己的使用经验。



