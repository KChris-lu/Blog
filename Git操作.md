---
title: Git 操作
date: 2018-03-10 21:53:39
tags: git
type: "type"
---

本文主要介绍了Git常用命令以及相关命令的理解。<br/>

## Git 常用命令

首先，先列出一系列经常需要使用到的命令：

```
git clone
git init
touch README.md
git remote add origin xxx
git checkout -b dev
git status
git add -A
git commit -m "xxx"
git push origin master

git log
git tag -a v1.0.1 -m "a stable version"
git push origin v1.0.1

git fetch origin master
git merge origin/master
git diff branch
```

其中，针对每个命令分析如下：<br/>1. 本地克隆远程仓库的项目：`git clone`<br/>2. 本地创建工程的代码管理仓库：`git init`<br/>3. 添加README.md文件到工程根目录：`touch README.md`<br/>4.添加远程仓库：`git remote add origin xxx`<br/>5.创建并且切换到新分支：`git checkout -b dev`<br/>6.查看当前状态：`git status`<br/>7. 将全部变动添加到工作区中：`git add -A`<br/>8. 将工作区中的代码提交到当前分支中并作出描述：`git commit -m "xxx"`<br/>9.将当前分支推送到远程仓库的master分支：`git push origin master`<br/>10. 查看提交日志：`git log`<br/>11. 为本地项目打上具有标注信息的标签并作出描述：`git tag -a v1.0.1 -m "xxx"`<br/>12. 将特定标签的项目分支上传到远程仓库：`git push origin v1.0.1 `<br/>13. 从远程origin的master分支下载最新的版本到origin/master分支上：`git fetch origin master`<br/>14. 将当前分支与origin/master分支进行合并：`git merge origin/master`<br/>15. 查看分支的改动：`git diff branch` <br/>

## Git命令详解

### git clone

```
git clone xxxxx
git clone <远程仓库地址>
```

将远程仓库克隆到本地。<br/>**Note：**<br/>　　通过克隆操作，Git自动将远程仓库命名为`origin`，并下载其中所有的数据，同时建立一个指向`origin` master分支的指针，在本地命名为`origin/master`。接着，Git在本地建立一个master分支，始于 `origin` master分支相同的位置。<br/>所以，克隆操作给本地带来的副作用有：<br/>1. 建立本地仓库，和远程仓库具有相同的数据<br/>2. 建立指向远程master分支的指针<br/>3. 建立本地master分支<br/>

### git pull

将本地分支和追踪分支合并以同步信息，具体的命令形式有：

```
git pull
git pull origin
git pull origin next
git pull origin next:master
git pull <远程主机名> <远程分支名>:<本地分支名>
```

　　需要注意的是，在使用`git pull`命令的时候，首先需要对当前分支进行提交操作，然后才使用。其中，针对每一条命令对应的使用场景，分析如下：<br/>1. 当前分支只有一个追踪分支时，可直接将当前分支与追踪分支合并：`git pull`<br/>2. 当前分支在特定远程仓库中已经指定过跟踪分支时，可直接通过指定远程仓库名，以完成和对应远程仓库跟踪分支的合并：`git pull origin`<br/>3. 将当前分支与特定远程仓库的特定追踪分支合并：`git pull origin next`<br/>4. 将本地特定分支和特定远程仓库的特定追踪分支合并：`git pull origin next:master`<br/>　　通过上述分析，我们可以发现，如果将本地分支和远程分支建立不同程度的追踪关系，合并分支时所需要书写的命令是不一样的。追踪关系越是精准，命令就越会是简单直接。那么，通常情况下，本地分支和远程分支的追踪关系是如何建立的呢？<br/>　　在某些场合，git会自动在本地分支与远程分支之间建立一种追踪关系。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支建立追踪关系，也就是说，本地master分支自动追踪origin/master分支。<br/>当然，我们也可以手动建立追踪关系，比如，指定master分支追踪origin/next分支：

```
git branch --set-upstream master origin/next
```

　　然后，可以省略远程分支名，只通过`git pull origin`便可以将本地当前分支自动与对应的origin追踪分支进行合并。如果当前分支只有一个追踪分支，还可以省略远程主机名，使用`git pull`将本地当前分支自动与唯一一个追踪分支进行合并。<br/>

### git push

```
git push origin master
git push origin master:dev
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin :dev
git push <远程主机名> :<远程分支名>
```

1 将本地master分支推送到远程仓库的master分支：`git push origin master`<br>2 将本地master分支推送到远程仓库的dev分支：`git push origin master:dev`<br>3 删除远程仓库的dev分支，直接将本地分支名置空：`git push origin :dev`<br>**Note:** <br/>1. git pull 将远程分支合并到本地仓库：`git pull <远程主机名> <远程分支名>:<本地分支名>`<br>2. git push 将本地分支推送到远程仓库：`git push <远程主机名> <本地分支名>:<远程分支名>`<br/>

### git fetch

```
git fetch origin
git fetch <远程主机名>
```

1 下载远程仓库origin的数据到本地仓库：`git fetch origin`<br>2 通过fetch操作，下载新的远程分支到本地。<br/>**Note：**<br/>　　假设新的远程分支命名为next，那么通过fetch操作，本地会新建一个origin/next指针，指向远程仓库的next分支。<br/>１. 如果要把该远程分支的内容合并到当前分支，运行：`git merge origin/next`<br/>２. 如果要在本地建立一个与远程next分支内容相同的同名分支，运行：`git checkout -b next origin/next`<br/>

### git checkout

```
git checkout dev
git checkout -b dev
git checkout -b dev origin/dev
git checkout --track origin/dev
git checkout -b <本地分支名> <远程主机名>/<远程分支名>
```

1 从当前分支切换到dev分支：`git checkout dev`<br/>2 新建并切换到dev分支：`git checkout -b dev`<br/>3 新建并切换到dev分支，且该本地dev分支和远程仓库origin的dev分支内容相同：`git checkout -b dev origin/dev` 或者 `git checkout --trackout origin/dev`<br/>**Note:**<br/>1. `git checkout -b dev` 等价于：`git branch dev` 和 `git checkout dev` 的合并<br/>2. 通过checkout操作对本地分支设置远程追踪分支后，可以简化pull 和 push操作<br/>

### git reset

```
git log
git reset a4e215234aa4927c85693dca7b68e9976948a35e README.md
git reset <version> <filename>
```

1 查找文件的提交记录，获取回退的版本记录：`git log`<br/>2 将指定文件回退到指定版本：`git reset xxx README.md`<br/>

### git stash

```
git stash
git stash list
git stash pop stash@{num}
git stash apply stash@{num}
git stash clear
```

1 保存当前分支的工作现场：`git stash`<br/>2 查看保存的所有工作现场：`git stash list`<br/>3 恢复工作现场并将其从stash列表中删除：`git stash pop stash@{num}`<br/>4 恢复工作现场：`git stash apply stash@{num}`<br/>5 清空stash列表：`git stash clear`<br/>**Note：**<br/>　　在不提交当前分支的情况下，如果需要切换到其他分支，可通过保存当前分支的工作现场的方式，切换到其他分支上进行工作。<br/>

### git remote

```
git remote
git remote add origin xxx
git remote rename hotfix dev
git remote rm dev
```

1 查看所有远程仓库：`git remote`<br>2 添加远程仓库：`git remote add origin xxx`<br>3 远程仓库hotfix重命名dev：`git remote rename hoxfix dev`<br>4 移除远程仓库dev：`git remote rm dev`<br>

**参考文章：**<br/>* [git pull命令](http://blog.csdn.net/qq_15037231/article/details/77937402)<br/>* [git 远程分支](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)<br/>* [git stash保存工作现场](http://blog.csdn.net/wlchn/article/details/50524690)<br/>* [如何让单个文件回退到指定版本](http://blog.csdn.net/ikscher/article/details/43851643)<br/>