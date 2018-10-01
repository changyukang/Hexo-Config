title: Windows下安装Vundle
layout: post
comment: true
date: 2015-05-26 15:24:36
categories: Vim
tags: [Vim,vundle,Windows]
keywords: Vim,vundle,Windows
description: How to install vundle in Windows operating system.
---
### 背景

打算在Windows下写博客，所以需要配置常用的编辑器Vim，由于有些插件需要安装，所以使用Vundle进行插件管理。

### 安装过程

当前Vundle正在进行接口变更：
![图1](Windows下安装Vundle1.png)

git路径和目录结构已经有了变化，安装需要根据官方的指南进行。

这里主要对Windows下的安装做下记录：

首先说明，在Windows下gvim安装后，路径是c:\program files\vim，里面会有一个对应版本的vim安装目录形如：vim74；还有一个vimfiles文件夹在c:\program files\vim下，还会有_vimrc文件，在家目录是没有的，如果你在家目录下生成一个，优先级比这里的高。家目录一般是：c:\user\yourusername，对应的是%USERPROFILE%，如果在环境变量中设置了$HOME，那么将会默认其为"~/"。

安装时，有两种方式：

1. 将vundle安装在%USERPROFILE%：

    ```
    cd %USERPROFILE%

    git clone https://github.com/VundleVim/Vundle.vim.git  %USERPROFILE%/vimfiles/bundle/Vundle.vim

    gvim _vimrc

    设置rtp为：set rtp+=%USERPROFILE%/vimfiles/bundle/Vundle.vim

    如果设置了$HOME，则可以设置为：set rtp+=~/vimfiles/bundle/Vundle.vim
    ```

2. 将vundle安装在$VIM（这是一个vim变量，在VIM界面通过：echo $VIM可以看到它是指：c:\program files\vim）

    ```
    cd c:\program files\vim

    git clone https://github.com/VundleVim/Vundle.vim.git  c:\program files\vim/vimfiles/bundle/Vundle.vim

    gvim _vimrc

    设置rtp为：set rtp+=$VIM/vimfiles/bundle/Vundle.vim

    如果设置了$HOME，则可以设置为：set rtp+=~/vimfiles/bundle/Vundle.vim
    ```

其它的不变，列在下面，这个在官方指南中可以找到，其中下面的一部分是插件示例，可以按需要安装：
![图2](Windows下安装Vundle2.png)

安装完后，执行BundleInstall后，会在%USERPROFILE%下生成.vim目录，这里存放着下载的插件。
