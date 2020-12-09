---
title: git简介
date: 2020-12-10 00:36:12
tags:
	[计算机,工具]
categories: 学习

---

# 1.什么是git？

git是一个分布式的版本控制工具，git的设计简单，是完全分布式的，允许成千上万的并行开发的分支（branch），有能力管理超大规模的项目，是首选的版本控制工具。

* 版本控制：一种记录一个或者若干文件内容的变化，以便查阅特定的版本修订情况的系统
* 分布式版本控制系统：把代码仓库完整的镜像下来，包括完整的历史记录。每一次克隆操作实际上都是一次对代码仓库的完整备份

当我们在使用git来管理我们的代码时，就可以很轻易的回溯到我们任意时间点所做的修改和提交，由于git的特性，我们可以很轻易就可以看到自己的修改，同时还可以利用分支管理在相同的时间做并行开发

<!--more-->

# 2.初次运行git前的配置

git自带一个git config工具可以用来帮助我们控制git的外观和行为的配置变量，一般来说，这些变量储存在三个位置上：

1. /etc/gitconfig文件：包含系统上每一个用户以及他们仓库的通用配置，如果在执行git config上加上--system选项，那么修改的就是这个文件的配置变量，这个的作用范围最广
2. ～/.gitconfig:只针对电脑的当前用户。可以传递--global参数来修改这个文件里的配置变量
3. 当前使用的git仓库里的.git会有一个config文件：这个只针对该仓库，可以传递--local选项修改，默认也是这个

每一个级别会覆盖上一个级别的配置，也就是说你给某个git仓库的user.name修改后，上面的两个gitconfig的相应配置变量就对这个git仓库无效了

可以使用 `git config --list --show-origin`查看所有的配置以及他们所在的文件

![屏幕快照 2020-12-10 上午1.11.41](https://tva1.sinaimg.cn/large/0081Kckwly1gli3to5yt4j314g03mwf9.jpg)

这个是我的所有git目录，当然我不是在某个git仓库里执行的命令,所以找不到第三个gitconfig，而我们进入一个git目录就会发现：

![屏幕快照 2020-12-10 上午1.14.54](https://tva1.sinaimg.cn/large/0081Kckwly1gli3x2idrmj314u0ay76o.jpg)

内容会多出一些，关于仓库的远程连接，一般在安装好git以后，我们只需要执行：

```
git config --global user.name "your username"
git config --global user.email yourEmail
```

把自己的用户信息写进gitconfig里，这样，当我们在git仓库里做修改或着提交时候就知道你的信息了

![屏幕快照 2020-12-10 上午1.18.34](https://tva1.sinaimg.cn/large/0081Kckwly1gli413hr5dj317e0gujtn.jpg)

​	比如这几个提交，我们就可以轻易的知道来自于哪里了

# 3.git的工作流程

git的三个工作区域对应着三个工作状态：

![6A5682E0F6FBFF11C78ADA00C48A2B3F](https://tva1.sinaimg.cn/large/0081Kckwly1gli45heht1j33340mmq9s.jpg)

工作流程：

1. 在工作目录里修改文件（modified）；
   1. 我们在工作区修改了文件，而没有用git add加入到暂存区时，就是modified
2. 暂存文件，将文件快照放入暂存区（staged）；
   1. 使用git add添加以后，就变成了staged
3. 提交更新，找到暂存区的工作文件，将文件永久的存储到git的仓库目录里（committed）

# 4.git的简单使用

* git init 初始化一个空的git仓库，这时候相应的文件夹下会自动生成.git的文件夹，里面就有刚刚说的gitconfig

* git status:查看文件的工作状态

* git add 跟踪文件，运行

  * 此时被add的文件已经被跟踪，处于暂存的状态，显示为changes to be committed

* git commit -m "提交所做的注解" 提交更新到仓库

* git diff:显示暂未暂存的改动，并非所有改动

* git rm 文件名：删除掉暂存区的文件,一般加上后缀--cached表示不删除原文件，只是从git暂存区删除，而-f则是直接删除掉这个文件

* git log:查看提交的历史，可以查看各个人的提交

* 版本回退：在git log里可以看到 commit后面跟着的这一大串散列码，这就是git的版本号，我们可以使用git reset --hard 版本号回退到之前的任何一个版本

  ![屏幕快照 2020-12-10 上午1.50.46](https://tva1.sinaimg.cn/large/0081Kckwly1gli4ydr3pzj30u00ufdlq.jpg)

# 5.git的分支管理

## 1.分支的简单图解

在版本回退里，我们可以回溯到之前的任意一个版本里，而这些版本都处在一条时间线上，这条时间线就是一条分支

默认的分支是main（之前是master，后来由于这个词有歧视黑人的意思，就换了这里的图都是用的master），本身可以把它看成一个指针，head指针就指向master指针

![image-20201210015840577](https://tva1.sinaimg.cn/large/0081Kckwly1gli5672o9lj308d0473yf.jpg)

我们在每次提交的时候，都会多出一个节点，指针也会随之移动，当我们创建一个新的分支，也就是创建一个新的指针，我们可以通过命令将head指针移动到新的指针上：

![image-20201210020044381](https://tva1.sinaimg.cn/large/0081Kckwly1gli58b6b3mj30dq06h3yj.jpg)

我们还可以将两个分支合并：

![image-20201210020232237](https://tva1.sinaimg.cn/large/0081Kckwly1gli5a76lkcj30br066jre.jpg)

还可以在这里八dev分支给删掉

![image-20201210020314110](https://tva1.sinaimg.cn/large/0081Kckwly1gli5awchx8j30br04fwef.jpg)

## 2.分支的使用

1. git branch 查看当前所有分支，及所在分支
2. git branch 分支名 ：创建新的分支
3. git checkout 分支名：切换到别的分支
4. （处于某个分支下）git merge 分支名，这就是将两个分支合并，此时两个分支就处在同一个节点下
5. git branch -d ：删除分支

题外：关于github仓库的一些关联

对仓库进行操作，使本地与github同步

> git remote add origin +git地址 (添加远程仓库到本地)
>
> git pull --rebase origin main(更新远程仓库的更新（mian分支)到本地）
>
> git push -u origin main将本地仓库与远程合并