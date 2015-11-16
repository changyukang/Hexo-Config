title: 进一步思考：如何保存github托管的vim插件的个人修改
layout: post
comment: true
date: 2015-01-16 15:14:35
categories: Git
tags: [Git,Fork,Vim,Hexo]
keywords: fork,submodule
description: 承接上篇博文：如何保存submodule的修改
---

### 问题背景
之前在文章["如何保存`submodule`的修改"](http://changyukang.github.io/2015/01/10/%E5%A6%82%E4%BD%95%E4%BF%9D%E5%AD%98submodule%E7%9A%84%E4%BF%AE%E6%94%B9/)里，已经提到了作者的目的：保存`vim`插件（`git`库）的修改。

主要原因是，如果下载了独立成为一个`git`库的`vim`插件，并且本地做了个性化的修改，那么在上级自己的`git`库目录中提交时就会出现“未跟踪的内容”的问题。那篇文章中想到了使用`submoduel`的方式，添加自己`Fork`的库，自己对于`Fork`的库有`push`的权利，这样就能先把`vim`插件的修改`commit`，然后`push`到那个`Fork`的库上，就可以在`vim`上级目录`git add`，这样就不会有“未跟踪的内容”的问题了。

但是，如果每个库都要`Fork`，那么作者`vim`插件几十个，自己的`github`岂不是要混乱不堪，充斥着别人的内容。

### 解决思路
于是，又想到了下面的方式。

因为`github`是分布式的，分为远端和本地，那么`commit`是到本地，`push`是到远端，如果不`push`，只是`commit`，也是把插件的修改提交了啊，只不过不会`push`到远端仓库而已。那这样之后，再回到`vim`上级目录`git add`，是不是也可以解决“未跟踪的内容”的问题呢？

### 实施过程

>**说明**：*上级目录：*作者使用`dotfile`方式管理一些配置，包括`vim`，并将`dotfile`托管到`github`上，那么`dotfiles`目录就是一个`git`库，它就是这里提到的上级目录。

>*插件目录：*`vim`使用`Vundle`安装插件，插件的形式都是一个一个`git`库，这些库于是就都在`dotfile`库了。

现在，对`vim`插件`CCTree`做一个简单修改，然后在上级目录`git add`，出现“未跟踪的内容”：
![图1](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改1.jpg)

请知：这里即使用`git add`.都是无法添加跟踪的。

下面，进入`CCTree`目录，执行`git status`，可看到修改，然后使用`git add`，将它缓存起来：
![图2](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改2.jpg)

可见，在插件目录是可以`git add`的。

接下来，将修改提交，只是`commit`(别人的库，无权`push`)：
![图3](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改3.jpg)


再次回到上级目录`git status`查看，发现`CCTree`已经变为“新提交”状态：
![图4](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改4.jpg)


然后`git add`，可以缓存成功啦：
![图5](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改5.jpg)


将其`commit`，成功：
![图6](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改6.jpg)


接下来`push`到自己的`dotfile`远端仓库也是ok的，再次查看，可以看到`CCTree`无法跟踪的问题已经解决了：
![图7](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改7.jpg)


### 补充说明：
这里的问题是，你的本地如果有多个`CCTree`插件库，那么你提交的仅仅是你操作的那一个，在其他的库里，还是没有修改的，也就是说你`commit`的东西仅在你本地一个目录中。下图可以看到，你右侧提交的并不会影响其它库。这是与`Fork`的区别，`Fork`可以`push`，然后其它库可以更新。
![图8](http://7xoae4.com1.z0.glb.clouddn.com/进一步思考：如何保存github托管的vim插件的个人修改8.png)


于是：如果使用`Fork`，你可以把所有插件库都`Fork`下来，然后在`vimrc`中的`vundle`安装处，全部改成你自己`Fork`下来的库地址。于是你可以对他们`commit`，然后`push`。也不用添加`submodule`。

也可以：`Fork`下来插件库，然后添加为`submodule`，其实对于`vim`插件，这样做意义不大，因为插件是用`vundle`安装的，直接在`vim`中执行安装即可。`submodule`的功能，可能使库的管理更方便了，你可以下载比如`dotfile`库，然后`git submodule update --init --recursive`将包含的子模块下载或者更新下来（`vundle`已经帮忙实现了类似的操作，所以`vim`插件不需要添加为`submoule`）对于其它工程，使用`submodule`还是比较方便的。

总之，将库`Fork`下来，是对于本地修改保存是很方便的，如果不想让`github`变的太乱，那么退而求其次的方式就是仅在本地`commit`（正如本文主要介绍的那样）。

**总结一下**：

比较好的是：`Fork`一下（成为自己的）--添加为`submodule`--提交修改（`commit`&`push`）

退而求其次：`Fork`一下(成为自己的)--不添加为`submodule`，提交修改（`commit`&`push`，不用`submodule`，不方便工程管理，需要到对应目录下载和更新）

再次：不`Fork`，不添加为`submodule`，直接修改（仅`commit`，库不是自己的，无权`push`，修改在本地仓库，无法同步）

