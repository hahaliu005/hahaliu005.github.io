---
title: Vim Basic Config
tags:
  - Vim
date: 2020-06-24
---

这些基本的Vim配置能满足基本的编辑需求

<!-- more -->

Pub below config to ~/.vimrc file

```
" Enables syntax highlighting.
syntax on

" To have Vim jump to the last position when reopening a file
if has("autocmd")
   au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif

" To have Vim load indentation rules and plugins according to the detected filetype.
if has("autocmd")
   filetype plugin indent on
endif

" set tab indent width
" expand tab with space
set expandtab
set shiftwidth=2
set tabstop=2
set softtabstop=2

" Set row number , and relative number
set nu
set cursorline
set relativenumber

" Set autoindent , otherwise set noautoindent
set autoindent

" About Encoding
set enc=utf8
" File encoding
set fenc=utf8
" File encodings
set fencs=utf8,ucs-bom,gb18030,gbk,gb2312,cp936

" Set colorscheme
colorscheme desert

" Disable bell in MacVim
set vb

" Fix: when use vim in iterm2, 'shift+g' or 'gg' cause flash white screen
set noerrorbells visualbell t_vb=
autocmd GUIEnter * set visualbell t_vb=
```