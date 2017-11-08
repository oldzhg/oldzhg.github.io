---
title: New MacBook Pro
date: 2018-11-24 16:46:03
tags: [Mac, 准备]
---


首先，我们安装 macOS Command Line Tools。

```
xcode-select --install
```

然后去App Store下载Xcode，安装完毕后打开Xcode并同意许可内容

[Homebrew](https://brew.sh/) 是 macOS 中必备的包管理工具。通过 Homebrew 可以无需 root 一键安装各类软件，省去复杂的依赖管理和编译参数配置。

同时，Homebrew Cask 还可以安装非命令行编译类程序，也就是平时日常用的软件都可以用 Homebrew Cask 进行安装。

Homebrew 强大的能力让自动初始化的第一步成为可能。

所以第一步，我们先安装 Homebrew：

```ruby
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

安装常用软件及工具

```ruby
#安装工具
brew update && brew install python3 wget cmake
#安装应用程序
brew cask install google-chrome sublime-text iterm2 dash iina hyperswitch karabiner-elements
```

安装oh my zsh

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

下载完成后编辑`.zshrc`文件并指定主题为`agnoster`

```
vim ~/.zshrc
set ZSH_THEME="agnoster"
```

安装Powerlevel9k主题

```
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

下载完成后编辑`.zashrc`文件并指定主题

```
vim ~/.zshrc
set ZSH_THEME="powerlevel9k/powerlevel9k".
```

安装插件

```

cd ~/.oh-my-zsh/custom/plugins/

#incr
mkdir ~/.oh-my-zsh/plugins/incr
wget http://mimosa-pudica.net/src/incr-0.2.zsh -O ~/.oh-my-zsh/plugins/incr/incr.plugin.zsh

#zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git

#zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions

#autojump
brew install zsh-autosuggestions autojump
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh

#把以上插件加入到 plugins 中
vi ~/.zshrc
在文件的最后一行添加：source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

编辑插件内容如图，请务必保证插件顺序，zsh-syntax-highlighting必须在最后一个。

![120393975-5b0511fcbec18_articlex](/Users/oldzhg/Documents/120393975-5b0511fcbec18_articlex.png)

若当前登入的帐号为你的帐号xxx，就不用特别显示出来

DEFAULT_USER="biyongyao"



### vim-plug

##### 安装

vim-plug的安装非常简单，一行命令完事。或者也可以将[plug.vim](https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim)中的内容复制到`~/.vim/autoload/plug.vim`中。

```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

##### 使用

使用vim-plug管理vim插件非常简单，具体命令见[vim-plug command](https://github.com/junegunn/vim-plug)。我们需要安装YCM插件，只需要在`~/.vimrc`文件中加入如下。

```
call plug#begin('~/.vim/plugged')
Plug 'Valloric/YouCompleteMe', { 'do': './install.py --clang-completer',  'for': ['c', 'cpp'] }
call plug#end()
```

其中`'do'`后面跟的命令是指YCM插件在使用vim-plug安装后，还需要执行的额外的命令（该命令在后面有解释），这也是YCM插件安装起来麻烦的原因。`‘for’`后面跟的内容是指YCM插件只在打开c和cpp文件时才会被激活。
接下来就可以在vim命令行中运行`PlugInstall`命令安装YCM

最后，还需要配置下YCM才可以正常使用，官方提供了一个默认的配置文件，号称可以解决99%的需求。在`.vimrc`文件中加入一行进行设置即可。YCM的配置非常丰富，具体可参考[YCM](https://github.com/Valloric/YouCompleteMe)。

```
let g:ycm_global_ycm_extra_conf='~/.vim/plugged/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'
```

#### 更新插件

要更新插件，请运行：

```
:PlugUpdate
```

更新插件后，按下 `d` 查看更改。或者，你可以之后输入 `:PlugDiff`。

#### 审查插件

有时，更新的插件可能有新的 bug 或无法正常工作。要解决这个问题，你可以简单地回滚有问题的插件。输入 `:PlugDiff` 命令，然后按回车键查看上次 `:PlugUpdate`的更改，并在每个段落上按 `X` 将每个插件回滚到更新前的前一个状态。

#### 删除插件

删除一个插件删除或注释掉你以前在你的 vim 配置文件中添加的 `plug` 命令。然后，运行 `:source ~/.vimrc` 或重启 Vim 编辑器。最后，运行以下命令卸载插件：

```
:PlugClean
```

该命令将删除 vim 配置文件中所有未声明的插件。

#### 升级 Vim-plug

要升级vim-plug本身，请输入：

```
:PlugUpgrade
```


