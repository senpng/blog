title: 终端折腾：Oh－My－ZSH
date: 2015-12-03 17:36:18
tags: [zsh,powerline,vim,Terminal]
category: 
---

## 了解 [Oh-My-ZSH](https://github.com/robbyrussell/oh-my-zsh)

### [Shell](http://zhuanlan.zhihu.com/mactalk/19556676)

Shell是Linux/Unix的一个外壳，你理解成衣服也行。它负责外界与Linux内核的交互，接收用户或其他应用程序的命令，然后把这些命令转化成内核能理解的语言，传给内核，内核是真正干活的，干完之后再把结果返回用户或应用程序。Linux/Unix提供了很多种Shell，为毛要这么多Shell？难道用来炒着吃么？那我问你，你同类型的衣服怎么有那么多件？花色，质地还不一样。写程序比买衣服复杂多了，而且程序员往往负责把复杂的事情搞简单，简单的事情搞复杂。牛程序员看到不爽的Shell，就会自己重新写一套，慢慢形成了一些标准，常用的Shell有这么几种，sh、bash、csh等，想知道你的系统有几种shell，可以通过以下命令查看：

    cat /etc/shells

显示如下：

    /bin/bash
    /bin/csh
    /bin/ksh
    /bin/sh
    /bin/tcsh
    /bin/zsh

在 Linux 里执行这个命令和 Mac 略有不同，你会发现 Mac 多了一个 zsh，也就是说 OS X 系统预装了个 zsh。

目前常用的 Linux 系统和 OS X 系统的默认 Shell 都是 bash，但是真正强大的 Shell 是深藏不露的 zsh， 这货绝对是马车中的跑车，跑车中的飞行车，史称『终极 Shell』，但是由于配置过于复杂，所以初期无人问津，很多人跑过来看看 zsh 的配置指南，什么都不说转身就走了。直到有一天，国外有个穷极无聊的程序员开发出了一个能够让你快速上手的zsh项目，叫做「oh my zsh」。这玩意就像「X天叫你学会 C++」系列，可以让你神功速成，而且是真的。

## 安装
选其中一个即可

**Curl**

    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

**Wget**

    sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

## 配置 *\.zshrc*
文件位置: `~/.zshrc`

### 主题

oh-my-zsh有很多[主题](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)。

可以根据自己的喜好设置主题:

    ZSH_THEME="amuse"

### 插件

oh-my-zsh有很多[插件](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins)。

这里就着重介绍几个不错的插件，后续会补充。

#### autojump

首先需要先安装 `autojump`
    
    brew install autojump

修改 `.zshrc` 配置文件

    plugins=(git autojump)

常用命令

    j [目录的名字或名字的一部分] ＃cd 某目录路径

    jo [目录的名字或名字的一部分] #打开某目录

    j -a [目录] #添加目录

    j -s #显示所有的记录

#### 待补充

## [PowerLine](https://github.com/powerline/powerline)
在安装配置zsh的时候，我发现了一个强大的功能插件：`PowerLine`，先贴张我已经配置好的截图。

![Vim](/images/zsh/vim.png)

注意到Vim界面下的那条状态栏了没？就是那个，目前我也就当显示好看用的:)

zsh终端也可以配置成使用PowerLine的。不过个人觉得不怎么好看，就没使用。我这里就再贴张zsh使用PowerLine的截图

![Terminal](/images/zsh/terminal.png)

### 安装
需要先安装 `python`
    
    brew install python
    pip install git+git://github.com/powerline/powerline

然后安装 `macvim`

    brew install macvim --env-std --override-system-vim

安装 [字体](https://github.com/powerline/fonts) 文件，不然一些字符无法再终端上显示，比如分支字符

    git clone https://github.com/powerline/fonts.git
    cd fonts
    ./install.sh
    cd .. && rm -rf fonts 

### 配置
终端需要设置下字体，需要选择带Powerline的字体，比如：

    Meslo LG S for Powerline

配置Vim `~/.vimrc`

    set rtp+=/usr/local/lib/python2.7/site-packages/powerline/bindings/vim #这里需要改成自己的安装路径
    set guifont=Meslo\ LG\ S\ for\ Powerline
    set laststatus=2
    let g:Powerline_symbols = 'fancy'
    set encoding=utf-8
    set t_Co=256
    set number
    set fillchars+=stl:\ ,stlnc:\
    set term=xterm-256color
    set termencoding=utf-8



### Vim颜色主题配置
安装 `Pathogen`

    mkdir -p ~/.vim/autoload ~/.vim/bundle && \
    cd ~/.vim/
    git clone https://github.com/tpope/vim-pathogen.git
    mv vim-pathogent/autoload ./
    rm -rf vim-pathogent

在 `.vimrc` 中添加
    
    execute pathogen#infect()

下载颜色配置

    cd ~/.vim/bundle
    git clone git://github.com/altercation/vim-colors-solarized.git #solarized颜色主题
    git clone https://github.com/endel/vim-github-colorscheme.git #github颜色主题

这里的颜色配置文件可以选择其中一个clone;


修改 `.vimrc`，在 `execute pathogen#infect()` 下面添加

    syntax enable
    set background=light
    colorscheme github

最终配置文件示例

    set rtp+=/usr/local/lib/python2.7/site-packages/powerline/bindings/vim
    set guifont=Meslo\ LG\ S\ for\ Powerline
    set laststatus=2
    let g:Powerline_symbols = 'fancy'
    set encoding=utf-8
    set t_Co=256
    set number
    set fillchars+=stl:\ ,stlnc:\
    set term=xterm-256color
    set termencoding=utf-8
    
    execute pathogen#infect()
    syntax enable
    set background=light
    colorscheme github
