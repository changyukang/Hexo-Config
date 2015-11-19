title: 如何保存submodule的修改
layout: post
comment: true
date: 2015-01-10 20:57:10
categories: Git
tags: [Git,Vim,Hexo]
keywords: Submodule,Fork
description: About Fork and Submodule
---

### 背景
个人使用`Vundle`管理插件，并且安装了`oh-my-zsh`，因为这种安装方式是`git clone`原作者的库到本地，所以可以实时跟踪作者的更新；我在使用插件时，会做一些个性化修改，但是这些修改不能提交到`github`，只能本地保存；

更进一步的需求是：将`.vim`使用`dotfiles`方式托管到`github`上，并且将自己对于插件的修改也托管到`github`上去;这样就要在`dotfiles`下建个仓库，并`push`到`github`上。

出现的问题：上述想法在实施的过程中，无法全部`git add`，个人做过修改的插件（每个插件目录都是一个`git`库），就会出现无法跟踪，所有`git add`的方式，都添加不进去。那这样我个人做的修改就无法`push`到`github`远端仓库了。

### 解决过程
首先想到使用`submoudle`的方式来添加插件。

插件本身就是一个`git`库，将其以子模块的方式添加进去，但是还有个问题，子模块的库是人家原作者的，你无权`push`，修改还是在本地。

那又想起一个方法：将原作者的库`fork`下来，添加子模块时，添加自己`fork`下来的库，那么你对这个库是有权`push`的，那么你把修改`push`到这个库上，并不会影响原作者的主分支，这样就实现了将你的`.vim`里面`Vundle`安装的插件以`dotfiles`托管到`github`的目的，并且将自己的修改`push`到了远端仓库。更进一步，如果你的个人配置真心不错，你还可以`pull request`，请原作者审核一下，将你的修改`merge`过去，为开源做贡献！

#### 添加子模块遇到的问题
想法是美好的，现实却很残酷，添加过程各种问题。

首先，`windows`下添加的子模块在远端仓库对应目录为灰色，无法链接，这个原因已在["`Windows`下`submodule`添加失败的解决方法"](http://changyukang.github.io/2015/01/08/Windows%E4%B8%8B%E6%B7%BB%E5%8A%A0git-submodule%EF%BC%8C%E8%BF%9C%E7%AB%AF%E4%BB%93%E5%BA%93%E5%AF%B9%E5%BA%94%E7%9B%AE%E5%BD%95%E4%B8%BA%E7%81%B0%E8%89%B2%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)里描述过了。

其次，添加的明明是自己`fork`的库，但是在对应插件目录`git remote -v`看到的却还是原作者的仓库地址。
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法1.png" width = "520" alt="图1" align=center />
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法2.png" width = "520" alt="图2" align=center />

后来想想，可能是原来自己用作者的库地址添加过一次`submodule`，已经有了残留，实际上，这种情况下再添加你自己`fork`的库为`submodule`时，会有提示：

<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法3.png" width = "520" alt="图3" align=center />

提示已经有一个`remote`在你想添加为子模块的本地目录中，并且如果想使用那个，就用`--force`选项，如果想自己定义，就用`--name`。确实，自己原来添加过一次，并且原来的目录中`git remote -v`可以看到`origin`地址是原作者的库地址。怎么办呢？使用`--name`选项似乎也不行，添加不成功，不知道是否书写方法不对？
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法4.png" width = "520" alt="图4" align=center />

#### 解决方法
1. 去那个目录，`git remote remove origin`，将那个`origin`删除，可单是这个似乎不行；
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法5.png" width = "500" alt="图5" align=center />
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法6.png" width = "500" alt="图6" align=center />

    这样搞了之后，引出上面`git directory *** is found locally with remote: origin`的问题。

2. 删除`.git/modules/mysubmodulefolder`（这里替换成上面提示的已经有`orgin`地址的那个目录，也就是你想添加子模块到的目录）
删除后，再根据删除`submodule`的方法，将各种配置清理干净（主要是`.gitmodules`和`.git/config`里的`submodule`信息以及子模块的存放目录），就可以再次添加`submodule`了，这次大功告成。

### 补充
另外提一点，子模块提交和`push`都要在子模块所在目录进行：
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法7.png" width = "520" alt="图7" align=center />

子模块修改提交成功后，返回你自己的库，`git add`，发现可以添加上了：
<img src="http://7xoae4.com1.z0.glb.clouddn.com/添加submodule并且可以保存自己的修改的方法8.png" width = "520" alt="图8" align=center />





                                                                      



