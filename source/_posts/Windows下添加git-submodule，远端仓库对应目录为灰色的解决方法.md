title: Windows下submodule添加失败的解决方法
layout: post
comment: true
date: 2015-01-08 01:01:41
categories: Git
tags: [Git,windows]
keywords: Git,submodule,windows
description: solving git problems
---
1. Windows下git操作目录分隔符的问题
    在Windows下添加git submodule，目录分隔符是"\\\\"：
    ```
        [submodule "themes\\TRNexT"]
            path = themes\\TRNexT
            url = https://github.com/uruir/TRNexT.git
        [submodule "themes\\hexo-theme-noderce"]
            path = themes\\hexo-theme-noderce
            url = https://github.com/willerce/hexo-theme-noderce.git
    ```
    这会导致远程仓库对应目录为灰色，无法点击:
    ![目录无法链接至对应仓库](http://7xoae4.com1.z0.glb.clouddn.com/2.png)

1. 修改目录分隔符，保持与Linux一致
    将目录分隔符修改为"/":
    ```
        [submodule "themes/TRNexT"]
            path = themes/TRNexT
            url = https://github.com/uruir/TRNexT.git
        [submodule "themes/hexo-theme-noderce"]
            path = themes/hexo-theme-noderce
            url = https://github.com/willerce/hexo-theme-noderce.git
    ```
    submodule建立成功：
    ![submodule建立成功](http://7xoae4.com1.z0.glb.clouddn.com/4.png)
