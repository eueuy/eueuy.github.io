---
layout: post
author: Liucheng Xu
title: "我的第一个 vim 插件"
category: 
tags: []
---

* TOC
{: toc}

### 简介

我的第一个 vim 插件：[vim-better-default](https://github.com/liuchengxu/vim-better-default), 说是 "插件", 有点投机取巧的意思，其实一点技术含量也没有，只不过是为了简化我的 `.vimrc` 文件将一些比较通用的部分包装起来而已。

为什么会想到这么做呢？因为看到了这个插件: [vim-sensible](https://github.com/tpope/vim-sensible), 作者将一些几乎通用的设置包装成一个插件。那如果我也将一些常用的设置包装成一个插件进行加载，那么这样可以精简很多 `.vimrc` 的设置呢. 

[space-vim](https://github.com/liuchengxu/space-vim) 原本单 `.vimrc` 就将近 400 行, [vim-better-default](https://github.com/liuchengxu/vim-better-default) 目前接近200行，这意味着 `.vimrc` 中减少了近 200 行内容，很有脱掉重武器装备，轻装上阵的感觉.

尽管没什么技术含量，不过对于 vim 初学者而言，我想这些设置还是非常有帮助的。就像 vim-sensible 的作者所说，这样就不用到处随机拷贝粘贴一些别人的 `.vimrc` 内容。安装好 vim 后的默认配置，还是非常不人性化的。如果是长期使用，裸 vim 毕竟还是不太方便，多多少少都会有些配置。

比如 [vim-better-default](https://github.com/liuchengxu/vim-better-default) 的部分内容:

```
set shortmess=atI " No help Uganda information
set incsearch     " Find as you type search
set hlsearch      " Highlight search terms
set ignorecase    " Case sensitive search
set smartcase     " Case sensitive when uc present
```

可能上面这几行在很多人的 `.vimrc` 都存在, 因此把这些共有的内容抽离出来也未尝不可。


### 用法
像正常插件一样安装即可，比如 [vim-plug](https://github.com/junegunn/vim-plug):

```
Plug 'liuchengxu/vim-better-default'
```

然后 `:so $MYVIMRC` 或退出再打开vim, 执行 `PlugInstall` 就可以安装了。

如果不想添加 vim-better-default 的键位映射部分，可以将其设置为0.不过还是建议您尝试一下，说不定会有新体验。

```
let g:vim_better_default_key_mapping = 0
```

此外还有针对部分功能的键位映射：

- `vim_better_default_basic_key_mapping`

- `vim_better_default_buffer_key_mapping`

- `vim_better_default_file_key_mapping`

- `vim_better_default_fold_key_mapping`

- `vim_better_default_window_key_mapping`

更多内容可以查看 [default.vim](https://github.com/liuchengxu/vim-better-default/blob/master/plugin/default.vim), 不用看到 "插件" 二字，会担心看不懂，它非常简单，跟你的 `.vimrc` 一样.

你也可以选择 fork 然后自行修改相应部分，这也非常容易。 希望 vim-better-default 也能简化您的 `.vimrc`.

### 键位绑定

键位绑定遵循的原则其实学习了 [spacemacs](https://github.com/syl20bnr/spacemacs) 的 “Mnemonics”, 英文解释是：the art or practice of improving or of aiding the memory，帮助记忆的方法。 

比如说 `<Leader> w` 就是涉及到 Window 的一些操作:

Key binding| Operation
:----:|:----:
`<Leader> w h` | 移动到左边 (h) 的window
`<Leader> w j` | 移动到下边 (j) 的window
`<Leader> w k` | 移动到上边 (k) 的window
`<Leader> w l` | 移动到右边 (l) 的window

这些键位绑定非常容易记忆。

另外，关于 `<Leader>` 的设置，推荐设置为 `SPC` ，即空格键，会减轻不少手指的压力。

```
let mapleader="\<Space>"
```

关于更多键位绑定的内容可以直接查看 [default.vim](https://github.com/liuchengxu/vim-better-default/blob/master/plugin/default.vim)， 里面的很多键位设置相信会带来一些不错的体验。