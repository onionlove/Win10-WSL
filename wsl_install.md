# WIN10安装WSL(Ubuntu子系统)

## 步骤一：确定windos的操作系统

windos的操作系统为WIN10(2018.03及以后)。

## 步骤二：配置WIN10系统

1、设置(win+I)–->更新和安全-->开发者选项，勾选“**开发人员模式**”，如下图所示

![1551752731045](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551752731045.png)

2、控制面板-->程序–->程序和功能【启用或关闭Windows功能】，勾选【适用于Linux的Windows子系统】，如下图所示：

![1551753148954](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551753148954.png)

## 步骤三：下载linux系统

可以选择下载商城的ubuntu系统，搜索“**wsl**”,如下图所示：

![1551753894750](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551753894750.png)

然后选择你所需安装的linux系统获取并且安装启动，点击左下角“win”图标然后双击下载的linux系统进行安装，如图所示：

![1551755979398](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551755979398.png)

## 步骤四：安装wsl terminal

wsl-terminal是一个很好用的WSL终端

1、下载WSL:<https://github.com/goreliu/wsl-terminal/releases>

2、添加右键菜单：将下载的安装包解压到相应的目录如图所示：

![1551754999865](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551754999865.png)

打开tool目录在鼠标右键添加你需要的功能。例如添加“**1-add-open-wsl-terminal-here-menu.js**”功能，如下图所示：

![1551763129152](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551763129152.png)

## 步骤五：更新root用户密码

桌面点击鼠标右键，选择 Open WSL Here 打开Bash

更新root密码：

```cmd
sudo passwd
```

## 步骤六：更新软件源（apt源和pip源）

系统默认的apt源是国外的，下载软件速度慢，python的pip源也很慢，我们都换成阿里云的源。

1、更换apt源

```cmd
cd /etc/apt/
sudo cp sources.list sources.list.bak && sudo vim sources.list
```

删除其中的所有内容，替换成以下内容即可：

```reStructuredText
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

然后保存并退出，执行以下内容：

```cmd
sudo apt update
sudo apt upgrade
```

2、修改pip源;

创建pip配置文件：

```cmd
sudo mkdir ~/.pip && sudo vim ~/.pip/pip.conf
```

在打开的vim中输入下文并保存：

```reStructuredText
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

## 步骤七：安装和配置zsh

zsh能让你通过键盘的上键，查找以前输入的命令。
例如，你可以输入`sudo`然后按向上键，就会出现以前输入的以`sudo`开头的命令。

### 安装zsh

1、安装zsh:

```cmd
sudo apt -y install zsh
```

2、安装oh-my-zsh:

```cmd
sudo sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

3、让wsl-terminal默认启动zsh

在wsl-terminal安装目录的/etc中，修改wsl-terminal.conf中相应的部分为：

```reStructuredText
;shell=/bin/bash
shell=/bin/zsh
```

### 配置zsh

第一次运行时你会得到下列输出：

```reStructuredText
This is the Z Shell configuration function for new users,zsh-newuser-install.  

You are seeing this message because you have no zsh startup files(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory~). This function can help you with a few settings that shouldmake your use of the shell easier.  

You can:  

(q) Quit and do nothing. The function will be run again next time.  

(0) Exit, creating the file ~/.zshrc containing just a comment.That will prevent this function being run again.  

(1) Continue to the main menu.  

(2) Populate your ~/.zshrc with the configuration recommended by the system administrator and exit (you will need to edit the file by hand, if so desired).  
```

为了让配置保持生效，将下边命令添加到~/.zshrc：

```reStructuredText
# 开启补全系统
autoload -Uz compinit

compinit

# 改进补全风格
zstyle :compinstall filename '/home/capecchi/.zshrc'

zstyle ':completion:*:descriptions' format '%U%B%d%b%u'

zstyle ':completion:*:warnings' format '%BSorry, no matches for: %d%b'

# 开启纠错功能
setopt correctall

# 开启高级命令提示符功能
autoload -U promptinit

promptinit

# 选择一个提示符样本
prompt elite2 yellow

# 基础的历史记录配置
HISTFILE=~/.histfile

HISTSIZE=1000

SAVEHIST=1000

# 设置忽略重复的记录
setopt hist_ignore_all_dups

# 防止某些特定的输入被记录
setopt hist_ignore_space
cat /proc/cpuinfo

# 设置autocd选项
setopt autocd

# 设置扩展的文件名替换
setopt extendedglob
```

更改用户默认的登录shell:

```cmd
chsh -s /bin/zsh username  普通用户可以更改自己的,root用户则可更改所有用户的
```



## 步骤八：安装Vim plug和YouCompleteMe

Vim plug是vim插件管理工具，YCM是很好用的C、python代码补全工具，也支持Go、Rust等其他语言.

1、下载Vim Plug:

```cmd
curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

2、在vimrc中添加安装配置

```cmd
vi ~/.vimrc
```

在其中添加以下内容：

```reStructuredText
" Specify a directory for plugins
call plug#begin('~/.vim/plugged')

" Make sure you use single quotes
Plug 'jiangmiao/auto-pairs'
Plug 'davidhalter/jedi-vim', {'for': 'python'}
Plug 'Valloric/YouCompleteMe', {'for': 'python'}

"可以添加已经手动安装的插件
"Plug '~/.vim/bundle/YouCompleteMe'
" Initialize plugin system
call plug#end()
```

保存退出，并进入vi

```cmd
vi
```

按下`：`键进入命令行，输入

```cmd
PlugInstall
```

**注意：**YCM的python补全依赖`jedi-vim`，因为YCM包有200多M，所以需要下载很久。

## 步骤九：编译YCM

1、安装YCM编译需要的依赖：

```cmd
sudo apt -y install cmake build-essential python3-dev
```

2、切换到YCM目录：

```cmd
cd ~/.vim/plugged/YouCompleteMe
```

3、更改编译使用python3（使用python2.7时跳过本步骤）：

```cmd
vi ./install.py
```

把第一行末尾的python改为python3，如下所示：

```reStructuredText
#! /usr/bin/env python 改成 #! /usr/bin/env python3:
```

4、编译YCM core

```cmd
./install.py
```

## 步骤十：配置YCM

YouCompleteMe配置后才能发挥出它强大的功效。

1、在vimrc中设置YCM使用python3支持：

```cmd
vim ~/.vimrc
```

添加以下内容：

```reStructuredText
"python版本设置，使用python2.7时去掉末尾的数字3
let g:ycm_python_binary_path='/usr/bin/python3'
" 语法关键字补全
let g:ycm_seed_identifiers_with_syntax=1
"从第2个键入字符就开始罗列匹配项
let g:ycm_min_num_of_chars_for_completion=2
"最小候选标识符长度
let g:ycm_min_num_identifier_candidate_chars = 5
"按下Ctrl+空格直接弹出所有类和函数，要先关闭系统中的输入法切换快捷键
let g:ycm_key_invoke_completion = '<C-Space>'
"配置文件路径
let g:ycm_global_ycm_extra_conf='~/.vim/.ycm_extra_conf.py'
"把ycm识别的标识符和语法关键字放在一起
let g:ycm_seed_identifiers_with_syntax = 1


"注释和字符串中的文字也会被收入补全，默认关闭
let g:ycm_collect_identifiers_from_comments_and_strings=1
"字符串输入补全，字符串中输入.加字母时会引发错误，默认开启
let g:ycm_complete_in_strings=1
```

不同于很多vim插件，YCM需要有配置。在vim启动后，YCM会找寻当前路径以及上层路径的.ycm_extra_conf.py。在~/.vim/bundle/YouCompleteMe/third_party/ycmd/examples/中提供了默认的模板。

复制配置文件：

```cmd
cp ~/.vim/plugged/YouCompleteMe/third_party/ycmd/examples/.ycm_extra_conf.py ~/.vim/
```

## 步骤十一：python和pip配置

1、修改默认python3

```cmd
sudo ln -s /usr/bin/python3 /usr/bin/python
wget bootstrap.pypa.io/get-pip.py
```

2、安装pip

```cmd
sudo python get-pip.py
```



## 过程中遇到的问题

### 安装wsl-terminal中的问题

（1）如果出现“**没有文件扩展 .js 的脚本引擎**”错误，可以参考该方式解决：

win+R输入regedit，找到路径”**\HKEY_CLASSES_ROOT\.js**“，配置如图所示：

![1551773201523](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551773201523.png)

也可以参考以下链接：[https://jingyan.baidu.com/article/ff42efa93a7ad9c19e2202f0.html](https://jingyan.baidu.com/article/ff42efa93a7ad9c19e2202f0.html)

（2）右键open-wsl-terminal-here出现”**找不到应用程序**“，可参考以下方法：

win+R输入regedit，找到路径”**\HKEY_CLASSES_ROOT\Directory\Background\shell\open-wsl**“，**数据**为安装wsl的对应目录，配置如图所示：

![1551764477109](C:\Users\cong_tao\AppData\Roaming\Typora\typora-user-images\1551764477109.png)

