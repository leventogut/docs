---
title: Vim
summary: Vim related
tags:
    - vim
    - editor
---
## Install Vundle Plugin Manager

```shell
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

## Create `.vimrc` file

```other
set number
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" PLUGINS

Plugin 'tpope/vim-fugitive'
Plugin 'bling/vim-airline'
set laststatus=2
Plugin 'majutsushi/tagbar'
nmap <C-t> :TagbarToggle<CR>
"Plugin 'Valloric/YouCompleteMe'
Plugin 'fatih/vim-go'
Plugin 'tomasr/molokai'
Plugin 'StanAngeloff/php.vim'
Plugin 'scrooloose/nerdTree'
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
"au VimEnter *  NERDTree "To open Tree at startup always
map <C-n> :NERDTreeToggle<CR>


" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line

syntax enable
"set background=dark
colorscheme molokai
```

## Install CTAGS

```shell
sudo apt-get install -y exuberant-ctags 
```

## Install plugins

Run Vim, and then type ":PluginInstall" (without quotes).

Also you can install from shell.

```shell
vim +PluginInstall +qall
```
