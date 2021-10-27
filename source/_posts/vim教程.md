---
title: vim教程
date: 2020-03-20 21:27:45
tags: 
    - vim 
    - shell 
    - linux
categories: 技术
---

## 1 Vim逻辑

> Vim命令逻辑为operation + motion的方式，首先输入主要操作（命令），然后可以输入行为，比如输入c，然后输入5→表示修改右边5个字符

<!--more-->

## 2 快捷键

* 普通模式

| 快捷键 |        含义         |                  功能                  |
| :----: | :-----------------: | :------------------------------------: |
|  esc   |                     |              进入普通模式              |
|   h    |                     |                   左                   |
|   j    |                     |                   下                   |
|   k    |                     |                   上                   |
|   l    |                     |                   右                   |
|   i    |       insert        |           光标前一个字符插入           |
|   gg   |                     |               到文件头部               |
|   G    |                     |               到文件尾部               |
|   a    |       append        |           光标后一个字符插入           |
|   I    |    shift+insert     |                行首插入                |
|   A    |    shift+append     |                行尾插入                |
|   u    |        undo         |                  撤销                  |
|   d    |     delete(cut)     |              删除（剪切）              |
|   p    |        paste        |                  粘贴                  |
|   y    |        yank         |                  复制                  |
|   c    |       change        | 修改（删除光标中的内容并进入编辑模式） |
|   w    |        word         |           光标移动到下一个词           |
|   f    |        find         |               在行内查找               |
|   %    |                     |          快速定位括号的另一半          |
|   >>   |                     |                向前缩进                |
|   <<   |                     |                向后缩进                |
|   cw   |     change word     |       光标位于词开头，修改整个词       |
|  ciw   |   change in word    |        光标位于中间，修改整个词        |
|  ci"   |    change in " "    |   光标位于单引号中，修改单引号中的词   |
|  di"   |    delete in " "    |   光标位于单引号中，删除单引号中的词   |
|  yi"   |     yank in " "     |   光标位于单引号中，复制单引号中的词   |
|  cf"   | change until find " |    修改从光标到找到单引号字符的内容    |

* 命令行模式

| 快捷键  |  含义  |                                     功能                                     |
| :-----: | :----: | :--------------------------------------------------------------------------: |
|    :    |        |                                  命令行模式                                  |
|   :q    |  quit  |                              命令行模式下，退出                              |
| :r file |        |              将file文件的内容读到当前文件中（从光标后一行插入）              |
|   :w    | writen |                          命令行模式下，保存（写入）                          |
| :w file |        | 另存到file文件（如果在vision模型下选中的内容，会将这部分内容保存到file文件） |
|   :s    |        |                                     替换                                     |
|   :!    |        |                                执行shell命令                                 |

## 3 搜索与替换

* `/`搜索模式下，从光标开始往后搜索

  | 快捷键 |    含义    |     功能     |
  | :----: | :--------: | :----------: |
  |   n    |    next    | 下一个匹配的 |
  |   N    | shift+next | 前一个匹配的 |

* `?`搜索模式下，从光标开始往前搜索，快捷键的功能与`/`模型下刚好相反

* `:s`替换

  |     语法      |          功能          |
  | :-----------: | :--------------------: |
  |   `:s/a/b/`   | 光标后的第一个a替换成b |
  |  `:s/a/b/g`   |        替换整行        |
  | `:1,4s/a/b/g` |     1-4行的a换成b      |
  |  `:%s/a/b/g`  |      全文的a换成b      |

## 4 vim基础配置

> 1. home目录下新建.vim目录
> 2. 在.vim下新建vimrc文件
> 3. 添加配置内容（map a b）（noremap a b）(set a)（let a）（color）

```vim
syntax on		# 语法高亮
set number		# 显示行号
set relativenumber			# 显示相对行号
set norelativenumber		# 不显示显示相对行号
set wrap	# 避免超出行显示
set showcmd		# 右下角显示命令
set wildmenu	# 命令行模式下tab提示
set hlsearch	# 高亮显示搜索结果
exec "nohlsearch"		# 退出重新进入后不高亮显示之前的结果
set incsearch		# 边输入边搜索
set ignorecase		# 搜索时忽略大小写
set smartcase		# 如果既含有小写又含有大写，则准确匹配

# <CR>代表回车
map S :w<CR>		# 大S快捷键映射为保存功能
map Q :q<CR>		# 大Q快捷键映射为退出功能
map R :source $MYVIMRC		# 大R快捷键映射为刷新该vim配置

noremap J 5j		# 大J快捷键映射为光标向下移动5行
noremap K 5k		# 大K快捷键映射为光标向上移动5行

color slate			# vim配色改为slate
```

## 5 vim插件安装

### 5.1 插件安装步骤

1. 找到插件的github地址，然后clone下来放到`~/.vim/bundle`目录下
2. 在`~/.vimrc`中写入`Plugin 'plugname'`，其中plugname为github中的项目名字

### 5.2 例子（安装vim-airline插件）

```shell
cd ~/.vim/bundle
git clone https://github.com/vim-airline/vim-airline.git
vim ~/.vimrc

# 在vimrc中加入一行
Plugin 'vim-airline/vim-airline'
```

> 也可以先安装vundle（插件管理工具）插件，只需要按照vundle的语法在vimrc中编辑语句后，执行PluginInstall可以自动下载插件，具体可以看vundle的github文档

### 5.3 vim主题安装

1. 下载相应的themename.vim到`~/.vim/colors`目录下
2. 然后在`~/.vimrc`中写入`color themename`即可

## 6 附录（.vimrc文件）

```vim
let mapleader=" "

syntax on
set encoding=utf-8
set number
set norelativenumber
set wrap
set showcmd
set wildmenu
"搜索相关
set hlsearch
exec "nohlsearch"
set incsearch
set ignorecase
set smartcase
noremap <LEADER><CR> :nohlsearch<CR>
"缩进相关
set smartindent
set tabstop=4
set autoindent
set expandtab
"保存退出快捷键
map S :w<CR>
map Q :q<CR>
map R :source ~/.vimrc<CR>
"快速浏览
noremap J 5j
noremap K 5k
"分屏相关
map sl :set splitright<CR>:vsplit<CR>
map sh :set nosplitright<CR>:vsplit<CR>
map sk :set splitbelow<CR>:split<CR>
map sj :set nosplitbelow<CR>:split<CR>
"分屏跳转
map <LEADER>l <C-w>l
map <LEADER>h <C-w>h
map <LEADER>k <C-w>k
map <LEADER>j <C-w>j
"分屏大小调节
map <up> :res +5<CR>
map <down> :res -5<CR>
map <left> :vertical resize-5<CR>
map <right> :vertical resize+5<CR>
"vundle插件相关
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'preservim/nerdtree'
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'connorholyday/vim-snazzy'
Plugin 'dense-analysis/ale'
Plugin 'Xuyuanp/nerdtree-git-plugin'
Plugin 'nathanaelkane/vim-indent-guides'
Plugin 'ycm-core/YouCompleteMe'

call vundle#end()            " required
filetype plugin indent on    " required
" nerdtree相关设置
let NERDChristmasTree=0
let NERDTreeChDirMode=2
let NERDTreeIgnore=['\~$']
let NERDTreeShowBookmarks=1
let NERDTreeAutoCenter=1
let NERDTreeShowLineNumbers=1
let NERDTreeShowHidden=1
let NERDTreeWinSize=25
let g:nerdtree_tabs_open_on_console_startup=1
let NERDTreeIgnore=['\.pyc','\~$','\.swp']
let g:NERDTreeGitStatusIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ 'Ignored'   : '☒',
    \ "Unknown"   : "?"
    \ }

autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
map tt :NERDTreeMirror<CR>
map tt :NERDTreeToggle<CR>
" airline相关设置
set laststatus=2
let g:airline_theme = 'simple'
let g:airline_powerline_fonts = 1
let g:airline#extensions#tabline#enabled = 1
" ale相关设置
let b:ale_linters = ['pyline']
let b:ale_fixers = ['autopep8', 'yapf']
" intent_guides相关设置
let g:indent_guides_guide_size = 1
let g:indent_guides_start_level = 2
let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_color_change_percent = 1
silent! unmap <LEADER>ig
autocmd WinEnter * silent! unmap <LEADER>ig
" youcompleteme相关设置
nnoremap gd :YcmCompleter GoToDefinitionElseDeclaration<CR>
nnoremap g/ :YcmCompleter GetDoc<CR>
nnoremap gt :YcmCompleter GetType<CR>
nnoremap gr :YcmCompleter GoToReferences<CR>
let g:ycm_autoclose_preview_window_after_completion=0
let g:ycm_autoclose_preview_window_after_insertion=1
let g:ycm_use_clangd = 0
let g:ycm_python_interpreter_path = "/usr/bin/python3"
let g:ycm_python_binary_path = "/usr/bin/python3"
" 主题相关设置
"color snazzy
colorscheme molokai
```