title: TC自定义快捷键：使用 Ctrl + k 或 Alt + d 访问地址栏
layout: post
comment: true
date: 2015-02-03 11:44:29
categories: TC
tags: [TC]
keywords: total commander,ctrl+k,alt+d,ctrl+e
description: Jump to the address bar and search bar quickly.
---
### 问题背景

作者想在TC的地址栏输入地址，跳转到指定目录，但是TC没有这条命令，后来尝试使用ctrl+downarrow，但是使用起来并不是很顺畅，所以思考之下，最终找到了定义快速定位到地址栏的方法：**快捷键自定义**。

### 解决方法

接下来，就要看快捷键设定成哪个比较合适了。在资源管理器和其他一些浏览器上，一般的快捷键设定如下：
>1. `Alt+d`:快速跳转到地址栏；
>2. `Ctrl+e/k`:快速跳转到搜索栏。

那我最终根据实际使用情况，设置如下：
![图1](http://7xoae4.com1.z0.glb.clouddn.com/TC自定义快捷键：使用%20Ctrl%20+%20k%20或%20Alt%20+%20d%20访问地址栏1.png)

设定后就可以方便的使用ctrl+k快速定位TC的地址栏了，其中`cm_EditPath`就是定位到地址栏进行编辑的意思。
